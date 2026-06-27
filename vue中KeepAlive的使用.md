# 一、缓存

## 1、在动态组件中的使用

保存组件的状态，而不是销毁，那么你就需要把 component 包裹起来，然后你在切换，他就会先放入缓存，而不是销毁。

① 父组件

```javascript
<template>
    <div>
        <p>动态组件一</p>
        {{num}}
        <br />
        {{ number }}
        <p>
            <button @click="number++">+1</button>
            &nbsp;&nbsp;&nbsp;&nbsp;
            <button @click="number--">-1</button>
        </p>
    </div>
</template>
<script>

export default {
    name: 'index',
    props: {
        num: {
            type: Number,
            default() {
                return 0
            }
        }
    },
    data() {
        return {
            number : 0
        }
    },
}
</script>
```

② 组件一

```java
<template>
    <div>
        <p>动态组件一</p>
        {{num}}
        <br />
        {{ number }}
        <p>
            <button @click="number++">+1</button>
            &nbsp;&nbsp;&nbsp;&nbsp;
            <button @click="number--">-1</button>
        </p>
    </div>
</template>
<script>

export default {
    name: 'index',
    props: {
        num: {
            type: Number,
            default() {
                return 0
            }
        }
    },
    data() {
        return {
            number : 0
        }
    },
}
</script>
```

③ 组件二

```javascript
<template>
    <div>
        <p>动态组件二</p>
        <button @click="throwAnEvent">抛出事件</button>
    </div>
</template>
<script>

export default {
    name: 'index',
    /**
     * 抛出事件
     */
    emits: ["throw"],
    methods: {
        throwAnEvent() {
            this.$emit("throw", 99)
        }
    },
}
</script>
```

# 二、keepalive属性

## 1、include 只有名称匹配的组件会被缓存

<font color=red>keepalive中的include是根据组件中的name进行匹配的，所以在创建组件的时候，尽量加上name。</font>

### 1） 字符串书写的时候，中间使用逗号分隔，分割后 最好不要加空格

 ![image-20211103000728064](./images/202203260259610.png)

### 2) 正则的写法

 ![image-20211103001020430](./images/202203260259898.png)

### 3）数组的写法

 ![image-20211103001107074](./images/202203260300943.png)

## 2、exclude 任何名称匹配的组件都不 会被缓存

### 1） 字符串书写的时候，中间使用逗号分隔，分割后 最好不要加空格

 ![image-20211103001422935](./images/202203260300098.png)

### 2) 正则的写法

 ![image-20211103001355427](./images/202203260300775.png)

### 3）数组的写法

  ![image-20211103001327015](./images/202203260301296.png)

## 3、max

里面传入的值：数字、字符串

最多可以缓存多少组件实例，一旦达 到这个数字，那么缓存组件中最近没有被访问的实例会被销毁。

# 三、设置了keep-live之后，vue中的生命周期

```javascript
    activated(){
        console.log("设置了keep-live之后，每次进入执行的钩子");
    },
    deactivated(){
        console.log("设置了keep-live之后，每次离开执行的钩子");
    }
```
