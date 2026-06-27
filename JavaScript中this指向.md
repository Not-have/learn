# 一、this的作用

<https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/this>

1、与其他语言相比，函数的  this  关键字在 JavaScript 中的表现略有不同，此外，在[严格模式](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Strict_mode)和非严格模式之间也会有一些差别。

2、在绝大多数情况下，函数的调用方式决定了  this 的值（运行时绑定）。this 不能在执行期间被赋值，并且在每次函数被调用时 this  的值也可能会不同。ES5 引入了 [bind](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Function/bind) 方法来设置函数的  this 值，而不用考虑函数如何被调用的。ES2015 引入了[箭头函数](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Functions/Arrow_functions)，箭头函数不提供自身的 this 绑定（this 的值将保持为闭合词法上下文的值）。

```javascript
let obj = {
    name: "哈哈哈",
    eating: function () {
        /**
         * 这块不使用this，也是可以的
         * 但是如果 你 obj 改变了，下面就要改变，这样会很不方便（在开发过程中，要切记不要低代码开发）
         */
        console.log(obj.name + "吃东西")
    },
    running: function () {
        console.log(this.name + "运动")
    },
    studying: function () {
        console.log(this.name + "学习")
    }
}
obj.eating() // 哈哈哈吃东西
obj.running() // 哈哈哈运动
obj.studying() // 哈哈哈学习
```

可查看下面这个案例（引用于MDN）：

<iframe height="490" src="https://interactive-examples.mdn.mozilla.net/pages/js/expressions-this.html" title="MDN Web Docs Interactive Example" loading="lazy"></iframe>

# 二、this的作用域

## 1、在全局作用域下

<font color=red>注：在全局作用域下，this指向window</font>

```javascript
console.log(this)
console.log(window)
```

## 2、在node环境下，this的指向

1）在node下，指向空的对象

2）在node下执行（其实源码里面是call来实现的）

![image-20220224235630762](./images/202203272352288.png)

## 3、<font color=red>this在函数中的使用</font>

注：<font color=red>this指向什么，跟函数所处的位置是没有关系的，跟函数 被调用的方式是有关系的；他是在运行时被绑定的</font>

```javascript
function fun(){
    console.log(this)
}
/**
 * 下面提供了三种调用方式
 */
fun()

let obj = {
    name:"哈哈",
    /**
     * foo现在指向上面的函数
     */
    foo: fun
}
obj.foo()

fun.apply("啊啊啊啊")
```

 ![image-20220227191556344](./images/202203272353932.png)

# 三、this的绑定规则

注：this的指向，与调用的位置（或方式）有关

## 1、默认绑定

注：独立函数调用，也就是在window下 函数名/变量名() 这样子去运行的；未加任何绑定（或者 .XXX() 去进行运行的绑定）

```javascript
/**
 * 独立函数调用，这个时候 指向window
 * 下面的案例，一块不太好看，你可以挨个去运行，查看this的指向
 */
// 案例1
function fun1() {
    console.log("fun1", this)
}

fun1() // Window

/**
 * 下面都是 独立被调用
 */
// 案例2
function fun2() {
    console.log("fun2", this)
}

function fun3() {
    console.log("fun3", this)
    fun2()
}

function fun4() {
    console.log("fun4", this)
    fun3()
}

fun4()

// 案例3 *
let obj = {
    name: "哈哈哈",
    fun5: function () {
        console.log("fun5", this)
    }
}
// obj.fun5() // 这个有调用主题（他不是 独立的，这个时候，它指向obj）
const fun6 = obj.fun5
fun6() // 指向window

// 案例4
function fun7(){
    console.log("fun7", this)
}
let obj2 = {
    name: "哈哈哈",
    fun: fun7
}
// obj.fun5() // 这个有调用主题（他不是 独立的，这个时候，它指向obj）
const fun8 = obj2.fun
fun8() // 指向window

// 案例5
function fun9(){
    function bar(){
        console.log("fun9", this)
    }
    return bar
}

const fun10 = fun9()
fun10()
```

![image-20220303002942614](./images/202203272353126.png)

## 2、隐式绑定

注：通过某个对象，进行的函数调用

 ![image-20220227224234941](./images/202203280024538.png)

```javascript
// case1
function fun1(){
    console.log(this) 
}
let obj1 = {
    age:222,
    foo: fun1
}
obj1.foo() // obj1
let bar = obj1.foo
bar() // window

// case2
let obj2 = {
    age:22,
    foo: function () {
        console.log(this, this.age + obj2.age)
    }
}
obj2.foo() // {age: 22, foo: ƒ} 44

// case3
let obj3 = {
     name: "obj3",
     foo: function () {
         console.log(this)
     }
}
let obj4 = {
    name: "obj4",
    // 对象中的属性，指向函数
    foo: obj3.foo
}
obj4.foo() // {name: 'obj4', foo: ƒ}
```

## 3、显示绑定

call、apply、bind 改变函数体内的this指向

语法：函数.call()、函数.apply()、函数.bind()

### 1）call

![image-20220227225543659](./images/202203280025236.png)

① call立即执行

```javascript
let name = "你好"
let obj = { age: 22, name: "小明" }
function fun(){
    console.log(this, this.name)
}
fun.call(obj) // 立即执行 {age: 22, name: '小明'} '小明'
fun() // Window ""
```

② call有多个参数时需要罗列（也就是按个书写）

```javascript
let name = "你好"
let obj = { name: "小明" }
function fun(age, gender){
    // 这个 是传入的
    this.age = age
    this.gender = gender
    console.log(this, this.name)
}

fun.call(obj, 222, "boy") // {name: '小明', age: 222, gender: 'boy'} '小明'
```

![image-20220227230554191](./images/202203280025795.png)

### 2）apply

① apply立即执行

```javascript
let name = "你好"
let obj = { age: 22, name: "小明" }
function fun(){
    console.log(this, this.name)
}
fun.apply(obj) // 立即执行
fun()
```

② apply的第二个参数是数组

```javascript
let name = "你好"
let obj = { name: "小明" }
function fun(age, gender){
    // 这个 是传入的
    this.age = age
    this.gender = gender
    console.log(this, this.name)
}
fun.apply(obj, [ 222, "boy" ])
```

 ![image-20220227230929616](./images/202203280026490.png)

### 3）bind

① bind不立即执行

```javascript
let name = "你好"
let obj = { age: 22, name: "小明" }
function fun(){
    console.log(this, this.name)
}
fun.bind(obj) // 不立即执行(他返回出来 是一个函数)
fun.bind(obj)() // 立即执行要jia 括号
fun()
```

② bind有多个参数时需要罗列（也就是按个书写）

```javascript
let name = "你好"
let obj = { name: "小明" }
function fun(age, gender){
    // 这个 是传入的
    this.age = age
    this.gender = gender
    console.log(this, this.name)
}
fun.bind(obj, 222, "boy")()
```

 ![image-20220227231141923](./images/202203280026815.png)

```javascript
function fun(){
    console.log(this)
}
/**
 * 1、bind的返回值 是一个函数
 * 2、bind是函数的方法
 * 3、传入的参数，就是绑定的this
 */
let newFun = fun.bind("传入的参数(也就是绑定this)，就是绑定的this")
newFun() // String {'传入的参数(也就是绑定this)，就是绑定的this'}
```

### 4）call、apply、bind使用场景

```javascript
let arr = [ 1, 2, 4, 6, 8, 11, 134, 0 ]
// console.log(Math.max(arr)) // NaN
/**
 * 这块不涉及this的绑定，所以穿空
 * 这块只能时apply，因为apply的第二个参数 是数组
 * Math.XXX 的apply只能写在类似于下面的这个位置
 */
console.log(Math.max.apply(null ,arr)) // 134
```

```javascript
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
    </head>
    <body>
        <input type="button" value="按钮">
        <br>
        <h2>内容</h2>
    </body>
</html>
<script>
    const but = document.querySelector("input")
    const content = document.querySelector("h2")
    /**
     * 这块就要使用 bind ，因为 你不想让他自动触发
     */
    but.onclick = function (){
        console.log(this) // 指向h2
    }.bind(content)
</script>
```

## 4、自己实现call、apply、bind方法

### 1）call的实现

```javascript
let fun = {
    getName: function () {
        return this.name
    }
}

let obj = {
    name: "小明"
}
/**
 * 1、手写call，要使用重新的这个  方法挂载到Function的属性上
 * 2、这个里面的this时谁？
 * 谁调用的他，这个this 就指向谁，这块的话，是getName调用的，所以 this就是 getName这个函数
 */
Function.prototype.myCall = function (context) {
    // console.log(arguments)
    // console.log(this)
    /**
     * 判断是否为函数，不是函数的时候，抛出错误
     */
    if (Object.prototype.toString.call(this).match(/\[object (.*?)\]/)[1].toLowerCase() !== 'function') {
        throw new Error("错误")
    }
    context = context || window
    let args = [ ...arguments ].slice(1)
    // console.log(args)
    /**
     * 在这 把 this赋值给context这个参数
     * 但是要假设 context 上有一个方法
     */
    context.fn = this
    let result = context.fn(...args)
    // console.log(result)
    return result
}
console.log(fun.getName.myCall(obj)) // 小明
```

### 2）apply的实现

```javascript
let fun = {
    getName: function (a, b) {
        console.log(a + b)
        return this.name
    }
}

let obj = {
    name: "小明"
}
/**
 * 1、手写apply，要使用重新的这个  方法挂载到Function的属性上
 * 2、这个里面的this时谁？
 * 谁调用的他，这个this 就指向谁，这块的话，是getName调用的，所以 this就是 getName这个函数
 */
Function.prototype.myAplly = function (context) {
    // console.log(arguments)
    // console.log(this)
    /**
     * 判断是否为函数，不是函数的时候，抛出错误
     */
    if (typeof this !== 'function') {
        throw new Error("错误")
    }
    context = context || window
    /**
     * 在这 把 this赋值给context这个参数
     * 但是要假设 context 上有一个方法
     */
    context.fn = this
    let result = null
    // 判断是否有第二个canshu,也就是判断 它是否为空值
    if (arguments[1]) {
        result = context.fn(...arguments[1])
    } else {
        result = context.fn()
    }
    // 删除这个(用完删除)
    delete context.fn
    // console.log(result)
    return result
}
console.log(fun.getName.myAplly(obj, [ 1, 2])) // 小明
```

### 3）bind方法的实现

```javascript
function fun() {
    console.log(this);
}

var obj = {
    name: "why"
}

function bind(func, obj) {
    return function () {
        // arguments传递过来的 所有参数
        console.log(arguments);
        return func.apply(obj, arguments);
    }
}

var bar = bind(fun, obj);

bar(); // obj对象
```

```javascript
let fun = {
    getName: function () {
        return this.name
    }
}

let obj = {
    name: "小明"
}
Function.prototype.myBind = function (context) {
    if(typeof this !== "function"){
        throw new Error("错误")
    }
    const that = this
    const args = [...arguments].slice(1)
    return function Fn(){
        if (this instanceof Fn){
            return new that(...args, ...arguments)
        }
        return that.apply(context, args.concat(...arguments))
    }
}
console.log(fun.getName.myBind(obj)()) // 小明
```

## 5、new绑定

```javascript
/**
 * new构造函数
 * 1、this = 创建出来的对象
 * 2、这个绑定过程，就是new绑定
 * 3、添加属性的时候，也就是属性添加进 this = {} 中
 */
function Person(val1, val2){
    this.val1  = val1
    this.val2  = val2
}
let f1 = new Person("呵呵", "哈哈哈")
console.log(f1.val1, f1.val2) // 呵呵 哈哈哈

let f2 = new Person("age", 25)
console.log(f2.val1, f2.val2) // age 25
```

 ![image-20220228231212002](./images/202203280027756.png)

## 6、this绑定优先级

1）默认规则的优先级最低

2）显示绑定优先级高于隐式绑定

3）new绑定优先级高于隐式绑定

4）new绑定优先级高于bind（new绑定和call、apply不能一块使用，bind可以和new一块使用）

```javascript
function fun(){
    console.log(this) // fun{}
}
let fn = fun.bind("哈哈哈")
let obj = new fun()
```

<font color=red>总结：</font>

*new绑定 > 显示绑定 > 隐式绑定 > 默认绑定*

# 四、内置函数的绑定

## 1、setTimeout

```javascript
/**
 * setTimeout相当于下面的回调函数
 */
setTimeout(function (){
    console.log(this) // window
}, 2000)
// 1、函数
function fun(fn, duration) {
    fn.call("指定this指向")
}
// 2、调用
fun(function () {
    console.log(this)
}, 2000)
```

## 2、元素中的this指向

```javascript
const box = document.querySelector("div")
// 把下面的这个点击传入给这个DOM元素作为属性使用
box.onclick = function (){
    console.log(this) // this指向元素对象
    /**
     * 这个this的指向，相当于一个隐式绑定
     * 元素对象.onclick()
     */
}
// 元素事件的监听(这个可以写多个，上面的就不可以写多个)
box.addEventListener("click", function (){
    /**
     * 调用的理解：
     * 得到当前的函数，使用call改变函数中this的指向，让其指向当前元素
     * 得到当前的函数.call(当前元素)
     */
    console.log(this)
})
```

 ![image-20220302230449499](./images/202203280027287.png)

## 3、数组方法的this指向

```javascript
let arr = [ 1, 2, 44, 66, "哈哈哈", "heheh " ]
/**
 * forEach、map 入第二个参数的时候，就可改变this指向
 */
arr.forEach(function (item){
    console.log(item, this)
}, "改变forEach的this的指向")
arr.map(function (item) {
    console.log(item, this)
}, "改变map的this的指向")
```

 ![image-20220302231111847](./images/202203280027142.png)

# 五、this指向的特殊情况

## 1、this的忽略显示绑定

```javascript
function fun(){
    console.log(this)
}
fun.apply(null) // Window
fun.apply(undefined) //Window

fun.call(null) // Window
fun.call(undefined) //Window

fun.bind(null)() // Window
fun.bind(undefined)() //Window
let fn1 = fun.bind(null)
fn1() //Window
let fn2 = fun.bind(undefined)
fn2() //Window
```

## 2、this的间接函数引用

```javascript
let obj1 = {
    name: "obj1",
    fun: function (){
        console.log(this)
    }
}
let obj2 = {
    name: "obj2"
};
/**
 * 把obj1的fun赋值给obj2的bar
 * 打印的是obj2
 */
// obj2.bar = obj1.fun
// obj2.bar() //{name: 'obj2', bar: ƒ}
/**
 * 下面把这个赋值写成自运行
 * (obj2.bar = obj1.fun)() 这样的写 是无法运行的，无法运行的原因如下：
 * 1、使用小括号括起来的话，你就把他看作为一个整体
 * 2、无法运行的原因是 在obj2后面没加分号的话，他就会  理解他们是一个整体，类似下面的效果：
     let obj2 = {
        name: "obj2"
     }(obj2.bar = obj1.fun)()
     所以，你加上分号 就不报错了（具体的原因，请阅读： 你不知道的javascript）
 * 3、所以在代码中使用小括号、中括号的时候，请查看自己的代码块 是否正确
 * 4、在写代码的时候，请尽量加分号
 */
(obj2.bar = obj1.fun)() // Window
```

# 六、箭头函数

注：① 箭头函数不会绑定this、arguments属性；

​        ② 箭头函数不能作为构造函数来使用（也就是不能和new 一块使用）

​     ③ 箭头函数不适用于上面的四种绑定规则（new绑定、显示绑定、隐式绑定、默认绑定）

## 1、箭头函数的this根据外层作用域来决定

## 2、箭头函数中this的指向

```javascript
let obj1 = {
    data: "",
    getData: function (){
        console.log(this) // {data: '', getData: ƒ}
        setTimeout(function (){
            console.log(this) // Window
        }, 2000)
    }
}
obj1.getData()

let obj2 = {
    data: "",
    getData: () => {
        console.log(this) // Window
        setTimeout(function () {
            console.log(this) // Window
        }, 2000)
    }
}
obj2.getData()

let obj3 = {
    data: "",
    getData: () => {
        console.log(this) // Window
        setTimeout(() => {
            console.log(this) // Window
        }, 2000)
    }
}
obj3.getData()
```

# 七、call、apply、bind的实现

注：① 给所有函数上挂载一个方法，需要使用到原型链；

​        ② 他们本质的实现是c++代码，但是 我们可以使用js来模拟；

## 1、给函数上添加一个 自定义的方法

```javascript
    /**
     * 自定义的方法
     * 把方法挂载到 函数上
     */ 
    Function.prototype.hello = function(){
        console.log("自定义的方法");
    }
    function fun(){
        console.log("fun");
    }
    // 在这调用
    fun.hello()
```

## 2、call的实现

1）基本使用，只传入一个参数(这个参数是对象)

```javascript
/**
 * 自定义的方法
 * 把方法挂载到 函数上
 */ 
Function.prototype.myCall = function(thisArg){
    console.log("自定义的方法")
    // 1、获取需要被执行的函数
    let fn = this
    /**
     * 2、调用被执行的函数
     * 给传入的参数（thisArg）上添加一个属性，代表在thisArg这个对象上，有了一个fn的属性
     * 
     */
    thisArg.fn = fn
    // 这样相当于给调用做了一个隐式绑定
    thisArg.fn()
    // 删除
    delete thisArg
}
function fun(){
    console.log("fun", this);
}
/**
 * 系统的方法:
 * 1、调用系统的方法，fun可以执行
 */
// fun.call()
/**
 * 自已的方法：
 * 1、fun方法没有执行
 * 2、他其实 是一个隐式的绑定,所以 myCall 里面的this，指向fun这个函数
 * 
 */
fun.myCall({})
```
