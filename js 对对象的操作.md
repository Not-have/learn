[文档](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty)：<https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty>

## 1、Object.defineProperty(obj, prop, descriptor) 参数的含义

obj — 要进行操作的对象
prop — 对对象中的那个属性进行操作，当写入的属性不存在时，就会给一个参数（对象中—），去添加这个属性
descriptor — <font color=red>属性描述符</font>，对属性（第二个参数）进行操作（属性描述符：数据属性、存取属性）

示例：

```javascript
let obj = {
    name: '里斯',
    age: 22
}

// 属性描述符 是一个对象
Object.defineProperty(obj, 'sex', {
    value: 'max' // 默认值 undefined
})

console.log(obj)
```

 <img src="./images/image-20221209000143154.png" alt="image-20221209000143154" style="zoom: 33%;" />

## 2、属性描述符 的分类

1）数据属性（数据属性一般都存在value，value 的默认值是 undefined）

① configurable

false 代表该属性不可删除，也不可修改

默认值： false

```javascript
let obj = {
    name: '里斯',
    age: 22
}

// 属性描述符 是一个对象
Object.defineProperty(obj, 'sex', {
    value: 'max',
    /**
     * false 该属性不可删除
     * false 该属性不可修改
     */
    configurable: false
})
delete obj.sex

console.log(obj)
```

② enumerable 表示属性是否可以通过for-in或者Object.keys()返回该属性

是否可以枚举

默认值： false

```javascript
let obj = {
    name: '里斯',
    age: 22
}

// 属性描述符 是一个对象
Object.defineProperty(obj, 'sex', {
    value: 'max',
    enumerable: false
    
})

console.log(Object.keys(obj)) // ['name', 'age'] 为 false 时，不可得到新增的 sex ,但是你使用的时候 不影响
```

③ writable 表示是否可以修改属性的值

默认值： false

```javascript
let obj = {
    name: '里斯',
    age: 22
}
// 属性描述符 是一个对象
Object.defineProperty(obj, 'sex', {
    value: '男',
    writable: false
})

obj.sex = '女'

console.log(obj) // {name: '里斯', age: 22, sex: '男'}
```

2）存取属性

注：存取属性描述符中 get 、set 不能和 writable 、value 共存。

```javascript
let obj = {
    name: '里斯',
    age: 22,
    _sex: '男'
}
// 存取属性描述符，可以把你不想暴露出去的私有属性，换个方式 暴露出去
Object.defineProperty(obj, 'sex', {
    enumerable: true,
    configurable: true,
    get: function (){
        return this._sex
    },
    set: function (value){
        this._sex = value
    }
})

console.log(obj)

obj.sex = '女';
console.log(obj)
```

① 隐藏某一个私有属性，但希望外界使用和赋值的时候使用；

② 如果我们希望截获一个属性，它访问和赋值的过程，也可以使用 存取属性描述符。

## 3、定义多个属性

```javascript
let obj = {};

Object.defineProperties(obj, {
    neme: {
        value: '里斯',
        // 里面的配置 与 defineProperty 相同
    },
    age: {
        value: 22
    }
})
console.log(obj) // {neme: '里斯', age: 22}
```

## 4、简单的写法

```javascript
// 简单写法，但不可 精准控制
let obj = {
    name: '里斯',
    _age: 22,
    set age(value) {
        this._age = value
    },
    get age(){
        return this._age
    }
}

obj.age = 25;

console.log(obj.age)
```

![image-20221210215915976](./images/202212102159658.png)

## 5、获取属性描述符

```javascript
let obj = {}
Object.defineProperties(obj, {
    name: {
        value: '里斯',
        // 里面的配置 与 defineProperty 相同
    },
    age: {
        value: 22
    }
})
console.log(obj) // {name: '里斯', age: 22}

// 获取单个
console.log(Object.getOwnPropertyDescriptor(obj, 'name')) // {value: '里斯', writable: false, enumerable: false, configurable: false}

// 获取所有
console.log(Object.getOwnPropertyDescriptors(obj))
```

## 6、禁止给对象添加新的属性

```javascript
let obj = {
    name: '里斯'
}

// 禁止给对象添加新的属性
Object.preventExtensions(obj)

obj.age = 22
obj.sex = '女'

console.log(obj) // { name: '里斯' }
```

## 7、禁止配置 或 删除的属性

1）自己实现

```javascript
let obj = {
    name: '里斯',
    age: 25
}

// 禁止配置 或 删除的属性
for(let key in obj){
    Object.defineProperty(obj, key, {
        configurable: false,
        enumerable: true,
        writable: true,
        value: obj[key]
    })
}

delete  obj.age


console.log(obj)
```

2）seal 的使用

```javascript
let obj = {
    name: '里斯',
    age: 25
}

// 禁止配置 或 删除的属性
Object.seal(obj)

delete  obj.age

console.log(obj)
```

## 8、禁止删除对象中的属性

```javascript
let obj = {
    name: '里斯',
    age: 25
}

Object.freeze(obj)

obj.name = '哈哈'
console.log(obj)
```
