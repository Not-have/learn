# 1、Object.defineProperty

1）Object.defineProperty 的设计初中不是为了监听对象中属性变化的，而是为了定义`访问属性描述符`。

2）Object.defineProperty 的缺点：

① 一次监听太多的时候，不是很友好；

② 新增、删除的时候，他是无能为力的；

③ 会修改原对象中的属性。

```javascript
const obj = {
    name: "里斯",
    age: 16
};

/**
 * 监听某个属性
 */
/*
Object.defineProperty(obj, "name", {
    set(v) {
        console.log(v);
        console.log("监听到 set");
    },
    get() {
        console.log("监听到 get");
    }
});
*/

/**
 * 监听所有的属性
 */
Object.keys(obj).forEach(key => {
    console.log(key);
    let value = obj[key];
    Object.defineProperty(obj, key, {
        set(v) {
            console.log(`监听到属性 ${key}，被 set 为 ${v}`);
            value = v;
        },
        get() {
            console.log(`监听到属性 ${key} get`);
            return value;
        }
    });
});


obj.name = "哈哈哈";

console.log(obj.name);
```

# 2、Proxy

在ES6中，新增了一个`Proxy 类`，这个类从名字就可以看出来，是用于帮助我们创建一个代理的，也就是说，如果我们希望监听一个对象的相关操作，那么我们可以先创建一个代理对象 ( Proxy 对象)，之后对该对象的所有操作，都通过代理对象来完成，代理对象可以监听我们想要对原对象进行哪些操作。

## 1）对象的监听

[![代码太长，所以放的图片，如果图片加载失败，请到 github 查看](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/3e08a8f141f44148ad6f66c7fdc1ab3e~tplv-k3u1fbpfcp-jj-mark:0:0:0:0:q75.image#?w=1970&h=6152&s=1538428&e=png&b=2b3d50)](https://github.com/Not-have/practise/blob/main/js/Proxy.js)

## 2）函数调用的监听

注：函数也是一个对象。

```javascript
function fun() {
    console.log("哈哈哈");
}

const objProxy = new Proxy(fun, {
    /**
     * 拦截对函数的调用的捕获器
     * @param target target new Proxy 所代理的 obj（监听的对象）
     * @param thisArg 调用函数时绑定的 this 值
     * @param argArray 调用函数时传递的参数列表
     */
    apply(target, thisArg, argArray) {
        console.log("对 fun 函数进行了 apply 的调用。");
        // 调用原始函数，并在其结果前后添加一些内容
        target.apply(thisArg, argArray)
    },
    /**
     * 监听 class 时的捕获器
     * @param target target new Proxy 所代理的 obj（监听的对象）
     * @param argArray new fun() 时传入的参数，例：new fun(1, 2, ...rest)
     * @param newTarget 被调用的构造函数。在这里，newTarget 就是 objProxy 本身，因为 objProxy 是一个函数代理
     */
    construct(target, argArray, newTarget) {
        console.log("对 fun 函数进行了 construct 的调用。");
        return new target(...argArray)
    }
});

objProxy.apply();
new objProxy();
```

## 3）new Proxy 中 receiver 参数的作用

```javascript
const obj = {
    _name: "里斯",
    age: 16,
    get name() {
        return this._name;
    },
    set name(newValue) {
        this._name = newValue;
    }
};

const objProxy = new Proxy(obj, {
    /**
     * receiver 就是 objProxy 这个代理对象
     * receiver 传入 Reflect.get 之后，他就作为 obj 里的 this 了（this 这个时候就改变了）
     *
     */
    get(target, key, receiver) {
        console.log(receiver);
        /**
         * 传入 receiver 后，他被访问了两次
         */
        return Reflect.get(target, key, receiver);
    },
    set(target, key, newValue, receiver) {
        Reflect.set(target, key, newValue, receiver);
    }
});

objProxy.name = "哈哈哈";
console.log(objProxy.name);
```

# 3、Reflect

## 1）作用

① 它主要提供了很多操作 JavaScript 对象的方法，有点像 Object 中操作对象的方法，例如下面的 Object API：

Reflect.getPrototypeOf(target) 类似于 Object.getPrototypeOf0 ；

Reflect.defineProperty(target, propertyKey, attributes) 类似于 Object.defineProperty0;

② 对对象本身的操作，会使用到 Reflect

## 2）文档

[Reflect 文档](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Reflect)

## 3）常见方法

[Reflect.apply()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Reflect/apply)

[Reflect.construct()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Reflect/construct)

[Reflect.defineProperty()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Reflect/defineProperty)

[Reflect.deleteProperty()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Reflect/deleteProperty)

[Reflect.get()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Reflect/get)

[Reflect.getOwnPropertyDescriptor()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Reflect/getOwnPropertyDescriptor)

[Reflect.getPrototypeOf()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Reflect/getPrototypeOf)

[Reflect.has()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Reflect/has)

[Reflect.isExtensible()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Reflect/isExtensible)

[Reflect.ownKeys()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Reflect/ownKeys)

[Reflect.preventExtensions()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Reflect/preventExtensions)

[Reflect.set()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Reflect/set)

[Reflect.setPrototypeOf()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Reflect/setPrototypeOf)

## 4）Reflect 的使用

```javascript
/**
 * 不对原来的对象，进行任何操作，这个时候，就使用了 new Proxy
 * 但是在不使用 Reflect 的情况下，你在 new Proxy 修改，他会依然改变了 obj
 */
const obj = {
    name: "里斯",
    age: 16
};

const objProxy = new Proxy(obj, {
    get(target, key, receiver) {
        return Reflect.get(target, key);
    },
    set(target, key, newValue, receiver) {
        /**
         * target[key] = newValue 和 Reflect.set(target, key, newValue) 的区别
         * target[key] = newValue 设置是否成功时不知道的
         * Reflect.set(target, key, newValue) 设置成功失败时会返回 Boolean
         */
        const result = Reflect.set(target, key, newValue);
        // 可以在这进行成功 / 失败的某些操作

    }
});

objProxy.name = "哈哈哈";

console.log(objProxy);
console.log(obj);
```

```javascript
/**
 * 实现一个 new Fun1，调用和执行的时候，依然是 Fun1，但是希望最后创建出来的这个对象的类型是 Fun2
 */
function Fun1(name, age) {
    this.name = name;
    this.age = age;
}

function Fun2() {

}

// const fn = new fun1("里斯", 16);
// console.log(fn);

const fn2 = Reflect.construct(Fun1, ["里斯", 16], Fun2);
console.log(fn2);
```

# 4、响应式

## 1）什么是响应式

n 有一个初始化的值，有一段代码使用了这个值，那么在 n 有一个新的值时，这段代码可以自动重新执行。

## 2）初步实现

```javascript
class Depend {
    constructor() {
        this.reactiveFns = new Set();
    }

    addDepend(receiverFn) {
        if (activeReactiveFn) {
            this.reactiveFns.add(receiverFn);
        }
    }

    /**
     * 通知已经改变的依赖
     */
    notify() {
        this.reactiveFns.forEach(fn => {
            fn();
        });
    }
}

/**
 * watchFn
 * 找到 depend 对象
 * 让传入的函数，默认执行一次
 *
 */
let activeReactiveFn = null;

function watchFn(fn) {
    activeReactiveFn = fn;
    fn();
    activeReactiveFn = null;
}

/**
 * 封装一个获取 depend 的函数
 * 只要是一个数据结构的封装，主要看  Map、WeakMap 的使用
 */
const targetMap = new WeakMap();

function getDepend(target, key) {
    // 根据target对象获取map的过程
    let map = targetMap.get(target);
    if (!map) {
        map = new Map();
        targetMap.set(target, map);
    }

    // 根据key获取depend对象
    let depend = map.get(key);
    if (!depend) {
        depend = new Depend();
        map.set(key, depend);
    }
    return depend;
}

function rective(obj) {
    return new Proxy(obj, {
        get(target, key, receiver) {
            // 根据 target 和 key 可以获取 depend
            const depend = getDepend(target, key);
            // 给depend对象中添加响应函数
            depend.addDepend(activeReactiveFn);

            return Reflect.get(target, key, receiver);
        },
        set(target, key, newValue, receiver) {
            Reflect.set(target, key, newValue, receiver);
            const depend = getDepend(target, key);
            depend.notify();
        }
    });
}

const obj = {
    name: "里斯",
    age: 18,
    address: "西安"
};

const objProxy = rective(obj);

watchFn(function () {
    console.log("name 改变", objProxy.name);
});

watchFn(function () {
    console.log("age 改变", objProxy.age);
});

objProxy.name = "哈哈哈";
```
