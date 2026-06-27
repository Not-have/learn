文档：<https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise>

​           <https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Using_promises>

# 一、认识

Promise  对象用于表示一个异步操作的最终完成 (或失败)及其结果值。

## 1、使用回调函数 完成Promise

```javascript
function requestData(code, successCallback, failtureCallback){
    setTimeout(() =>{
        if(code === "你好"){
            console.log("成功")
            successCallback("成功的回调函数")
        }else {
            console.log("失败")
            failtureCallback("失败的回调函数")
        }
    },2000)
}

/**
 * 这个是使用的，那么上面的函数，必须给这通知一个结果
 * 基础的写法：传入成功和失败的回调函数
 */
requestData("你好", (res) => {
    console.log(res)
}, err => {
    console.error(err)
})
```

## 2、Promise的使用

一个 Promise 必然处于以下几种状态之一：

待定（pending）: 初始状态，既没有被兑现，也没有被拒绝。

已兑现（fulfilled）: 意味着操作成功完成。

已拒绝（rejected）: 意味着操作失败。

注：① 他是ES6新增的；

​        ② Promise是一个类（是类就要 new 类名）；

​        ③ <font color=red>Promise状态一旦确定下来，那么就是不可更改的</font>；

```javascript
function fun(){
    return new Promise((resolve, reject) => {
    
    })
}
const fn = fun()
// .then 方法传入成功的回调，会在Promise执行resolve函数时，被回调
fn.then(() => {

})
// .catch 方法传入失败的回调，会在Promise执行reject函数时，被回调
fn.catch(() => {

})
/**
 * Promise中传入的是一个 回调函数
 * 这个回调函数，传入之后，会立即执行
 * 传入的这个函数叫 executor
 * 这个回调函数，有两个参数：resolve, reject
 * 成功：resolve
 * 失败：reject
 */
const promise = new Promise((resolve, reject) => {
    console.log("Promise中传入的函数，立即执行")
})
```

基础使用：

```javascript
function fun() {
    /**
     * resolve、reject中可以传入参数
     */
    return new Promise((resolve, reject) =>{
        fetch("https://mock.mengxuegu.com/mock/61922927f126df7bfd5b79ef/promise/promise3#!method=get").then(req => {
            return req.json()
        }).then(res => {
            resolve(res)
        }).catch(err => {
            reject(err)
        })
    })
}
const fn = fun()
fn.then(res => {
    console.log("成功", res)
}).catch(err => {
    console.log("失败", err)
})
// 上面的也可以传入两个函数(比如下面的写法)
fn.then(res => {
    console.log(res)
}, err =>{
    console.log(err)
})
```

## 3、resolve, reject的参数

1）普通的值或者对象

```javascript
new Promise((resolve, reject) => {
    // resolve("resolve")
    resolve({name:"哈哈哈"})
}).then(res => {
    console.log(res) // {name:"哈哈哈"}
}, err => {
    console.error(err)
})
```

2）再传入一个Promise对象

如果当入的是一个Promise，那么当前Promise的状态，由传入的Promise决定；相当与当前的状态做了移交

```javascript
const newPromise = new Promise((resolve, reject) => {
    resolve("传入的Promise")
})

new Promise((resolve, reject) => {
    resolve(newPromise)
}).then(res => {
    console.log(res) // 传入的Promise
}, err => {
    console.error(err)
})
```

3）传入一个对象（并且这个对象实现了thenable）传入一个对象，并且这个对象有实现 then 方法，那么也会执行该 then 方法，并且该 then 方法决定后续状态

```javascript
new Promise((resolve, reject) => {
    const obj = {
        then: function (resolve, reject){
            // resolve("我是对象中的then方法")
            reject("我是对象中的then方法中 失败的回调")
        }
    }
    /**
     * 当你传入的是一个对象，并且这个对象里面有then方法，它到时候会自己执行
     * 这个对象中的then方法，会自动传递进来两个参数，分别为：resolve, reject
     * 这个时候，你就可以在对象的then方法中，去决定这个Promise的状态
     */
    resolve(obj)
}).then(res => {
    console.log(res)
}, err => {
    console.error(err)
})
```
