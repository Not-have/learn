# 一、迭代器（Iterator protocol）

## 1、Iterator 描述

迭代器（iterator），是确使用户可在容器对象（container，例如链表或数组）上遍访的对象，使用该接口无需关心对象的内部实现细节。

注：迭代器可以帮助我们去遍历某个数据结构。

[文档](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Iterators_and_Generators#%E8%BF%AD%E4%BB%A3%E5%99%A8)

## 2、实现一个简单的迭代器

迭代器是一个对象，但是需要符合[迭代器协议](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Iteration_protocols#%E8%BF%AD%E4%BB%A3%E5%99%A8%E5%8D%8F%E8%AE%AE)。

[文档](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Iteration_protocols#%E8%BF%AD%E4%BB%A3%E5%99%A8%E5%8D%8F%E8%AE%AE)
注：在使用的过程中，根据需求，进行修改。

```javascript
/**
 * 下面就是一个迭代器
 * 但是他是毫无作用的
 */
/*
const iterator = {
    next: function () {
        return {done: true, value: "test"};
    }
};
*/

/**
 * 创建一个迭代器，去访问一个数组
 * 如果迭代器能够生成序列中的下一个值，则返回 false 布尔值。（这等价于没有指定 done 这个属性。）
 *
 * 如果迭代器已将序列迭代完毕，则为 true。这种情况下，value 是可选的，如果它依然存在，即为迭代结束之后的默认返回值。
 */
function createArrayIterator(arr) {
    let index = 0;
    const arrIterator = {
        next: function () {
            // return {done: false, value: "你好"};
            // return {done: false, value: "哈哈"};
            // return {done: false, value: "啊啊"};
            // return {done: true, value: undefined};
            if (index < arr.length) {
                return {done: false, value: arr[index++]};
            } else {
                return {done: true, value: undefined};
            }
        }
    };
    return arrIterator;
}

const arr1 = ["你好", "哈哈", "啊啊"];
const arr1Iterator = createArrayIterator(arr1);

console.log(arr1Iterator.next());
console.log(arr1Iterator.next());
console.log(arr1Iterator.next());
// 前面的都能访问到，后面的就无法访问了
console.log(arr1Iterator.next());
console.log(arr1Iterator.next());

const arr2 = [1, 2, 3, 4, 5, 6];
const arr2Iterator = createArrayIterator(arr2);
console.log(arr2Iterator.next());
console.log(arr2Iterator.next());
console.log(arr2Iterator.next());
console.log(arr2Iterator.next());
console.log(arr2Iterator.next());
console.log(arr2Iterator.next());
console.log(arr2Iterator.next());
console.log(arr2Iterator.next());
```

 ![image-20240202230330197](./images/image-20240202230330197.png)

# 二、可迭代对象 (Iterable protocol)

## 1、认识可迭代对象

1）能  for...of... 遍历的，就是一个可迭代的对象；

2）数组本身就是一个可迭代对象，也就是说数组中本身就有 Symbol.iterator

3）Map/Set 也是可迭代对象

4）函数中 arguments 也是一个可迭代对象

5）字符串...

注：对象是不可以的。

```javascript
const iterableObj = {
    arr: ["你好", "哈哈", "啊啊"],
    // Symbol.iterator 是一个函数
    [Symbol.iterator]: function () {
        let index = 0;
        return {
            // 不使用箭头函数的话 this 指向存在问题
            next: () => {
                /**
                 * if (index < 1) { 时
                 * for (const item of iterableObj) {
                 *     console.log(item);
                 * } 他只会遍历一次，因为 for...of 的本质也是使用 next 进行遍历的
                 */
                if (index < this.arr.length) {
                    return {done: false, value: this.arr[index++]};
                } else {
                    return {done: true, value: undefined};
                }
            }
        };
    }
};

/*
// 下面的代码，一般情况下是不会使用的
const iterator1 = iterableObj[Symbol.iterator]();
console.log(iterator1.next());
console.log(iterator1.next());
console.log(iterator1.next());
console.log(iterator1.next());
console.log(iterator1.next());
console.log(iterator1.next());

const iterator2 = iterableObj[Symbol.iterator]();
console.log(iterator2.next());
console.log(iterator2.next());
console.log(iterator2.next());
console.log(iterator2.next());
 */

// for...of 可以遍历的东西，必须是一个可迭代的对象
for (const item of iterableObj) {
    console.log(item);
}
```

## 2、使用

```javascript
/**
 * 数组本身就是一个可迭代对象
 * 也就是说数组中本身就有 Symbol.iterator
 * Map/Set 也是可迭代对象
 * 函数中 arguments 也是一个可迭代对象
 * 字符串
 * 只有可迭代对象，才可以使用 for...of...
 * 注：对象是不可以的
 */
const arr = ["你好", "哈哈", "啊啊"];

const iterator = arr[Symbol.iterator]();
console.log(iterator.next());
console.log(iterator.next());
console.log(iterator.next());
console.log(iterator.next());
```

## 3、应用场景

```javascript
const iterableObj = {
    arr: ["你好", "哈哈", "啊啊"],
    // Symbol.iterator 是一个函数
    [Symbol.iterator]: function () {
        let index = 0;
        return {
            // 不使用箭头函数的话 this 指向存在问题
            next: () => {
                if (index < this.arr.length) {
                    return {done: false, value: this.arr[index++]};
                } else {
                    return {done: true, value: undefined};
                }
            }
        };
    }
};

const [item1, item2] = iterableObj;

const set = new Set(iterableObj)

const arr = Array.from(iterableObj);
console.log(arr);

Promise.all(iterableObj).then(res => {
    console.log(res);
})
```

## 4、给类添加可迭代对象

```javascript
class Person {
    /**
     * 这里可以不止有 names 一个参数，你可以根据情况自己添加
     */
    constructor(names) {
        this.names = names;
    }

    entry(newName) {
        this.names.push(newName);
    }

    /**
     * 下面这样就可以获取 class 中指定的迭代器了
     */
    [Symbol.iterator]() {
        let index = 0;
        return {
            next: () => {
                if (index < this.names.length) {
                    return {done: false, value: this.names[index++]};
                } else {
                    return {done: true, value: undefined};
                }
            },
            // 这个不会经常使用
            return: () => {
                console.log("迭代器提前截至了");
                return {done: true, value: undefined};
            }
        };
    }
}

const p1 = new Person(["小明", "小花", "哈哈哈"]);

for (const item of p1) {
    console.log(item);
}

for (const item of p1) {
    if (item === "小花") break;
    console.log(item);
}
```

# 三、生成器

## 1、什么是生成器？

生成器是 ES6 中新增的一种函数控制、使用的方案，它可以让我们更加灵活的控制函数什么时候继续执行、暂停执行等。

## 2、生成器函数和普通的函数的区别

注：生成器函数也是一个函数

首先，生成器函数需要在 function 的后面加一个符号

```javascript
function* fun() { //  建议把 * 放到 function 后，好阅读，如果没有 function 时，把 * 写到函数名前面，例如：*fun
    
}
```

其次，生成器函数可以通过 yield 关键字来控制函数的执行流程

```javascript
function* fun() {
    console.log("函数开始");

    console.log(100);
    yield;

    console.log(200);
    yield;

    console.log(300);
    yield;

    console.log("函数结束");
}
```

最后，生成器函数的返回值是一个 Generator（生成器），生成器事实上是一种特殊的迭代器

```javascript
function* fun() {

}

// 调用生成器函数（fun）时，会给我们返回一个生成器对象
const generator = fun();
console.log(generator);
```

![image-20240203181200223](./images/image-20240203181200223.png)

## 3、执行流程

```javascript
/**
 * yield 是暂停执行
 * return 是停止执行，后面在执行 next，也不会给下面走了
 */
function* fun() {
    console.log("函数开始");

    console.log(100);
    // 这里不能写 return，这里写了后，后面的全部不执行了
    // return 100;
    // 需要返回值的话，给 yield 后面写，不写时，返回的是：{ value: undefined, done: false }
    yield 100;

    console.log(200);
    yield 200;

    console.log(300);
    yield 300;

    console.log("函数结束");
    return "函数结束";
}

// 调用生成器函数（fun）时，会给我们返回一个生成器对象
const generator = fun();
console.log(generator.next());
generator.next();
generator.next();
console.log(generator.next());
```

![image-20240203182658640](./images/image-20240203182658640.png)

## 4、生成器的其他方法

### 1）next 中传入参数

```javascript
function* fun() {
    console.log("函数开始");

    console.log(100);
    // 这里不能写 return，这里写了后，后面的全部不执行了
    // return 100;
    // 需要返回值的话，给 yield 后面写，不写时，返回的是：{ value: undefined, done: false }
    const parame1 = yield 100;

    console.log(200, parame1);
    const parame2 = yield 200;

    console.log(300, "parame2", parame2);
    const parame3 = yield 300;

    console.log("函数结束", parame3);
    return "函数结束";
}

// 调用生成器函数（fun）时，会给我们返回一个生成器对象
const generator = fun();
generator.next(); // 第一段代码，比较特赦，无法传递参数
generator.next(10); // 在这传入参数，可以在上面，进行参数的接受，如：const parame1 = yield 100; parame1 就是这块传入的 10
generator.next(20);
generator.next(30);
```

### 2）调用函数时掺入参数

```javascript
function* fun(value) {
    console.log(value, "函数开始");

    console.log(100);
    // 这里不能写 return，这里写了后，后面的全部不执行了
    // return 100;
    // 需要返回值的话，给 yield 后面写，不写时，返回的是：{ value: undefined, done: false }
    const parame1 = yield 100;

    console.log(200, parame1);
    const parame2 = yield 200;

    console.log(300, "parame2", parame2);
    const parame3 = yield 300;

    console.log("函数结束", parame3);
    return "函数结束";
}

// 调用生成器函数（fun）时，会给我们返回一个生成器对象
const generator = fun("你好"); // 传入默认参数
generator.next(); // 第一段代码，比较特赦，无法传递参数
generator.next(10); // 在这传入参数，可以在上面，进行参数的接受，如：const parame1 = yield 100; parame1 就是这块传入的 10
generator.next(20);
generator.next(30);
```

### 3）生成器终止执行

```javascript
function* fun(value) {
    console.log(value, "函数开始");

    console.log(100);
    const parame1 = yield 100;

    console.log(200, parame1);
    const parame2 = yield 200;

    console.log(300, "parame2", parame2);
    const parame3 = yield 300;

    console.log("函数结束", parame3);
    return "函数结束";
}


const generator = fun("你好");
generator.next();
// 第二段代码 终止执行，后面的就不会在执行了
console.log(generator.return(15));
generator.next();
```

 ![image-20240204114329764](./images/image-20240204114329764.png)

### 4）生成器 - 抛出异常

```javascript
function* fun(value) {
    const num1 = 1;
    console.log("第一段代码");
    // yield;
    try {
        yield;
    } catch (err) {
        console.log("捕获到异常", err);
        // 这里也可以写 yield "返回的参数"
    }

    const num2 = 1;
    console.log("第二段代码");
    yield;

    console.log("函数结束");
}


const generator = fun("你好");
generator.next();
generator.throw("异常信息"); // 捕获到异常时，这块就不会报错了
```

![image-20240204132309845](./images/image-20240204132309845.png)

# 四、生成器替代迭代器

注：生成器其实就是一种特殊的迭代器（开发过程中，能用 `生成器` 就尽量用）。

```javascript
/**
 * 生成器替代迭代器
 */
function* createArrayIterator(arr) {
    /*
    // 下面的可以替换成 yield* 后面跟可迭代对象
    for (const item of arr) {
        yield item
    }
    */
    yield* arr
}

const arr = [1, 2, 3];
const arrIterator = createArrayIterator(arr);

console.log(arrIterator.next());
console.log(arrIterator.next());
console.log(arrIterator.next());
console.log(arrIterator.next());
```
