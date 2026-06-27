注：对象的操作可查看：<https://juejin.cn/post/7175524754894356540>

[new 运算符](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/new)：<https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/new>

# 一、构造函数

## 1、认识构造函数

```javascript
function  fun(){
    console.log('初步认识构造函数')
}

// 通过 new 关键字去调用一个函数。那么这个函数就是一个构造函数
new  fun // 不需要传参的时候，就可以不加括号
new fun()
```

注：函数被 new 执行后的操作：

1）在内存中创建一个新的对象（空对象）；

2）这个对象内部的[[prototype（原型）]]属性会被赋值为该构造函数的prototype属性；

```javascript
function fun(){

}

const  p = new fun

console.log(p.__proto__ === fun.prototype) // true
```

3）构造函数内部的this，会指向创建出来的新对象；

4）执行函数的内部代码（函数体代码）；

5）如果构造函数没有返回非空对象，则返回创建出来的新对象（自己会 return 返回）。

![image-20221211000200454](./images/202212110002357.png)

## 2、构造函数初步认识

注：<font color=red>构造函数的函数名，首字母大写（方便看到函数的时候就知道 他为构造函数），并且命名为大驼峰命名。</font>

```javascript
function  Fun(name, age, sex){
    /**
     *  this.name = name
     *  这样的赋值，就相当于 构造函数自己创建的那个空对象中赋值，因为 构造函数中的 this 指向构造函数自己创建的那个 空对象
     *  构造函数中，会自己返回他创建的那个对象，当然 你也可以指定返回
     */
    this.name = name;
    this.age = age;
    this.sex = sex;
}

const p = new Fun('里斯', 11, '男') // 你可以 new 多个

console.log(p.name) // 里斯
```

## 3、对象的原型

```javascript
let obj = {name: '里斯', age: 25} // 每个对象中都有 [[prototype]]，但是 这个为隐式原型，我们 不会直接使用

console.log(obj.__proto__) // {} __proto__ 暂时指向一个空的对象
```

注：但是早期的 ECMA 是没有查看 [[prototype]] 的 API 的，但是浏览器提供了这个属性 <font color=red> __proto__ </font>。

1）获取对象的原型

注：ES5 之后提供的。

```javascript
let obj = {name: '里斯', age: 25} // 每个对象中都有 [[prototype]]

console.log(Object.getPrototypeOf(obj))
```

## 4、函数的原型

注：函数也是一个对象。

```javascript
function fun(){

}
console.log(fun.__proto__); // __proto__ 隐式原型
```

1）函数的显式原型

函数可以使用 prototype 获取原型。

```javascript
function fun(){

}

console.log(fun.prototype)
```

2）原型指向

所有的函数，只要你创建出来了，隐式原型最后也会指向显式原型。

3）属性

fun.prototype 这个对象中有一个 `consatructor` 属性。

```javascript
function Fun(){

}

// Fun.prototype 可以使用 Object.defineProperty 去修改，他的内置属性，但是 第二个 参数，要为 constructor
console.log(Fun.prototype.constructor) // [Function: Fun] 他是构造函数本身
```

```javascript
function Fun(){

}

console.log(Fun.prototype.constructor) // [Function: Fun]
console.log(Fun.prototype.constructor.name) // .name 可以得到函数名
console.log(Fun.name) // .name 可以得到函数名
```

## 5、prototype 的使用

1）自己添加属性

```javascript
function fun(){

}

fun.prototype.name = '里斯';
fun.prototype.eat = function (){
    console.log('添加的方法')
}

const  p = new fun();

console.log(p.name, p.eat())
```

 ![image-20221211223219009](./images/202212112232305.png)

2）直接修改 prototype 整个对象

```javascript
function fun(){

}


// 这样写的话 constructor 对应的fun函数的原型会进行销毁的
fun.prototype = {
    // constructor: fun, // 如果里面不写这个，fun 里面没有 constructor 属性了（但是不建议）
    name: '里斯',
    age: 25
}

// 这就和 没有全 prototype 时一样了
Object.defineProperty(foo.prototype, 'constructor', {
    enumerable: false, // 是否可枚举
    constructor: true,
    writable: true,
    value: fun
})
```

3）批量创建构造函数

```javascript
function Fun(name, age, sex){
    // 基础的绑定，不能使用原型链，负责 它本身定义的返回值，就存在问题了
    this.name = name
    this.age = age
    this.sex = sex
}

// 这样写，就不会在每次 new 的时候，都去堆内存中去创建对象了（推荐），因为 你在外面的使用的时候，他的查找顺序为 对象——> 原型
Fun.prototype.eating = function () { // 这个可以写多个
    // 这里的 this 不会存在问题，因为他是动态绑定的
    console.log(this.name + '吃 💩')
}

Fun.prototype.runing = function () { // 这个可以写多个
    // 这里的 this 不会存在问题，因为他是动态绑定的
    console.log(this.name + '打飞机')
}

let p = new Fun('里斯', 25, '男'); // new 函数名 他构造出来的是对象

p.eating()
```

# 二、面向对象

面向对象的三大特性：<font color=red>封装、继承、多态</font>。

[文档](https://developer.mozilla.org/zh-CN/docs/Learn/JavaScript/Objects/Object-oriented_programming)：<https://developer.mozilla.org/zh-CN/docs/Learn/JavaScript/Objects/Object-oriented_programming>

## 1、原型链

1）概念

注：在对象中去获取属性的时候，如果当前对象中不存在，就去原型对象上找，并且会沿着原型链一直去查找，直到最顶层。

 ![image-20221214000551013](./images/202212140005202.png)

2）原型链的顶层是什么？来自什么地方？

` [Object: null prototype] {} ` 就是最顶层的原型。

<font color=red>注：</font>

① Data、函数等类型都是 Object 的子类；

② Object.prototype 是最顶层的原型，可参考下面的例子：

![image-20221220234048311](./images/202212202343749.png)

③ Object 是一个构造函数；

④ Object.prototype 不是一个空对象，二叔里面的属性默认值为不可枚举的状态；

![image-20221221234213171](./images/image-20221221234213171.png)

⑤ 对象的原型是：__proto__ ，函数的原型是：prototype；

⑥ 当原型指向 null 的时候，就代表他为最顶层的原型；

3）构造函数原型

```javascript
function Fun(){

}

/**
 * __proto__ 不一定有
 * __proto__ 指向的 就是顶层原型
 */
console.log(Fun.prototype) // {}
console.log(Fun.prototype.__proto__) // [Object: null prototype] {} 指向 Object 的原型对象
```

## 2、继承

1）使用原型链实现继承

```javascript
// 父类 公共的 属性 和 方法

function Person(){
    this.name = '里斯'
}
Person.prototype.eating = function (){
    console.log(this.name + '吃 💩')
}

// 子类 特有的 属性 和 方法
function Student(){
    this.age = 22
}

const p = new Person();

// 创建完 Student 之后，就给他的原型上加了 Person 这个类（不随写在别的地方）
// 现在 Student.prototype 指定 p 对象的原型
Student.prototype = p; // 简化：Student.prototype = new Person();

Student.prototype.studying = function (){
    console.log(this.age + '学习吃💩')
}

const stu = new Student()

console.log(stu.name)
console.log(stu.eating())
```

缺点：

① 打印 stu 对象，某些属性是看不到的，例上面的 stu 只能打印出来 console.log(stu) // Person { age: 22 }，他打印不出来原型上的东西；

② 创建两个对象

![image-20221227000056950](./images/image-20221227000056950.png)

a、直接修改对象上的属性，是给本对象添加了一个新属性；

b、获取引用，修改引用中的值，会相互影响；
