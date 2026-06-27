# 一、Vue3的认识

​        Vue (读音 /vjuː/，类似于 **view**) 是一套用于构建用户界面的**渐进式框架**。与其它大型框架不同的是，Vue 被设计为可以自底向上逐层应用。Vue 的核心库只关注视图层，不仅易于上手，还便于与第三方库或既有项目整合。另一方面，当与[现代化的工具链](https://v3.cn.vuejs.org/guide/single-file-component.html)以及各种[支持类库](https://github.com/vuejs/awesome-vue#components--libraries)结合使用时，Vue 也完全能够为复杂的单页应用提供驱动。

​        新特性：更好的性能、更小的包体积、更好的TypeScript集成、更优秀的API设计。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script src="https://unpkg.com/vue@next"></script>
</head>
<body>
    <div id="app"></div>
    <script>
        const el = {
            template:"<h1>你好</h1>"
        }
        const app = Vue.createApp(el)
        app.mount("#app");
    </script>
</body>
</html>
```

<hr />

<em> 注：</em>vue在html里面的两种使用方式

① 使用script标签，类型为 x-template

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script src="https://unpkg.com/vue@next"></script>
</head>
<body>
    <div id="app"></div>
    <script type="x-template" id="box">
        <div>
            {{num}}
        </div>
    </script>
    <script>
        Vue.createApp({
            template: "#box",
            data() {
                return {
                    num: 999
                }
            },
        }).mount("#app")
    </script>
</body>
</html>
```

②常使用template标签，因为他不会被渲染，因为他是HTML的标签，具体请参考：<https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/template>

​        template元素是一种用于保存客户端内容机制，该内容在加载页面时不会呈现，但随后可以(原文为 may be)在运行时使用JavaScript实例化。

​        将模板视为一个可存储在文档中以便后续使用的内容片段。虽然解析器在加载页面时确实会处理**`**元素的内容，但这样做只是为了确保这些内容有效；但元素内容不会被渲染。

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta http-equiv="X-UA-Compatible" content="IE=edge">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Document</title>
        <script src="https://unpkg.com/vue@next"></script>
    </head>
    <body>
        <div id="app"></div>
        <template id="box">
            {{ num }}
        </template>
        <script>
            Vue.createApp({
                template: "#box",
                data() {
                    return {
                        num: 99
                    }
                },
            }).mount("#app")
        </script>
    </body>
</html>
```

## 1、vue源码下载

#### ①建议使用github克隆

```text
git clone git@github.com:vuejs/vue-next.git
```

#### ②如果你是下载的压缩包，则需要以下的操作

注：以下指令的含义，自行百度。

a、yarn install

b、git init

c、git add .

d、git ommit -m '提交的信息'

## 2、运行

在运行钱要修改package.json文件，在运行指令后面加<em>  --sourcemap </em>,这样你在调试vue.global.js的时候，你才能看到源码。

```text
yarn dev
```

运行之后，他会把package里面的文件打包，并且生成一个vue文件——>dist——>vue.global.js（这个是需要使用的文件，vue.global.js.map是映射文件）。

## 3、调试

①在examples（例子）下创建demo文件夹，在demo文件夹下写案例，如下面这个demo.html；

```html
<!-- 建议vsc中建议使用Open In Default Browser打开这个HTML -->
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <div id="app"></div>
    <script src="../../dist/vue.global.js"></script>
    <script>
        debugger;
        Vue.createApp({
            template:"<h1>你好！</h1>"
        }).mount("#app")
    </script>
</body>
</html>
```

*<font color=red>注：key的作用：</font>*

​        key 特殊 attribute 主要用做 Vue 的虚拟 DOM 算法的提示，以在比对新旧节点组时辨识 VNodes。如果不使用 key，Vue 会使用一种算法来最小化元素的移动并且尽可能尝试就地修改/复用相同类型元素。而使用 key 时，它会基于 key 的顺序变化重新排列元素，并且 key 不再存在的元素将始终被移除/销毁。

# 二、v-bind的使用

`注`：
①以下的key都是可以不加引号的

②value不加引号就是变量了

③下面也可以调用一个对象，但是更建议使用计算属性

## 1、绑定class

```javascript
<template>
    <div>
        <!-- 1、普通语法 -->
        <div :class="one"> 一 </div>
        <!-- 2、对象语法 {'类名': 布尔值, '类名': 布尔值} 注：这个布尔值，可以是变量，类名不可以 -->
        <div :class="{'className2': true}"> 二 </div>
        <!-- 3、可以将对象放到一个单独的属性中 -->
        <div :class="obj1"> 三 </div>
        <!-- 4、传入数组 -->
        <!-- 4-1、数组中是元素 -->
        <div :class="['className4-1', two]"> 四1 </div>
        <!-- 4-2、数组中也可以是三元运算符或者绑定变量 -->
        <div :class="['className4-2', bool ? 'className4-2-a':'']"> 四2 </div>
        <!-- 4-3、数组中也可以是对象语法 -->
        <div :class="[obj1, {'className4-3': bool}]"> 四3 </div>
    </div>
</template>

<script>
import { reactive, ref } from 'vue'
export default {
    name: 'XXXXX',
    setup() {
        const one = ref("className1")
        const obj1 = reactive({ className3: true })
        const two = ref("className4-1-a")
        const bool = ref(true)
        return { one, obj1, two, bool }
    }
}
</script>

<style scoped>
.className1 {
    color: red;
}
.className2 {
    color: royalblue;
}
.className3 {
    color: pink;
}
.className4-1 {
    color: seagreen;
}
.className4-1-a {
    font-size: 20px;
}
.className4-2 {
    color: red;
}
.className4-2-a {
    font-size: 22px;
}
.className4-3 {
    font-size: 30px;
}
</style>
```

## 2、绑定style

注：类似与 font-size 的要使用驼峰（fontSize）或者直接写成字符串（'font-size'）

```javascript
<template>
    <div :style="{height: '200px', width: '200px', 'background-color': 'red'}"></div>
    <div :style="obj1"></div>
    <!-- 数组里面放多个对象，最后也是可以进项合并的 -->
    <div :style="[{fontSize: size + 'px',color:'red'}, obj1]">哈哈哈哈</div>
</template>

<script>
import { reactive, ref } from '@vue/reactivity'
export default {
    name: 'XXXXX',
    setup() {
        const obj1 = reactive({ height: '200px', width: '200px', backgroundColor: 'pink' })
        let size = ref(40)
        return { obj1, size }
    }
}
</script>
```

## 3、动态绑定属性

```javascript
<template>
    <div :[key]="value"></div>
</template>

<script>
import { reactive, ref } from '@vue/reactivity'
export default {
    name: 'XXXXX',
    setup() {
        const key = ref("class");
        const value = ref("box");
        return { key, value }
    }
}
</script>

<style scoped>
.box {
    width: 100px;
    height: 100px;
    background-color: pink;
}
</style>
```

## 4、绑定一个对象

```html
<template>
    <div v-bind="obj">
        <!-- 
            给标签或者组件（这样就可以直接传值）上直接加对象，这个时候，就不能使用简写了（使用简写阅读性巨差），要实现的效果如下
            <div dir="dir" lang="zh"></div>
         -->
    </div>
</template>

<script>
import { reactive, ref } from '@vue/reactivity'
export default {
    name: 'XXXXX',
    setup() {
        const obj = reactive({
            dir: "dir", // 规定元素中内容的阅读顺序
            lang: "zh" // 设定了该元素中所要使用的语言
        })
        return { obj }
    }
}
</script>

<style scoped>
div {
    width: 100px;
    height: 100px;
    background-color: pink;
}
</style>
```

# 三、watch的使用

<font color=red>使用 deep 选项时，旧值将与新值相同，因为它们的引用指向同一个对象/数组。Vue 不会保留变更之前值的副本。</font>

## 1、基础使用

```html
<template>
    <div>
        <p>Watch监听器</p>
        <button @click="fun">点击</button>
        {{ info }}
    </div>
</template>

<script>
export default {
    name: 'Watch',
    data() {
        return {
            info: { name: "小明", age: 22 }
        }
    },
    watch: {
        info(newValue, oldValue) {
            console.log("新值" , newValue, "旧值" , oldValue);
        }
    },
    methods: {
        fun() {
            this.info = { name: "小花" };
        }
    }
}
</script>
```

## 2、watch的配置

```html
<template>
    <div>
        <p>Watch监听器</p>
        <button @click="fun">点击</button>
        {{ info }}
    </div>
</template>
<script>
export default {
    name: 'Watch',
    data() {
        return {
            info: { name: "小明", age: 22 }
        }
    },
    watch: {
        // 只改变对象里面的某个属性，这个时候  watch不触发 但是 视图改变了
        info(newValue, oldValue) {
            console.log("新值" , newValue, "旧值" , oldValue);
        }
    },
    methods: {
        fun() {
            // 只改变对象里面的某个属性
            this.info.name = "小花";
        }
    }
}
</script>
```

*所以在这种使用就要使用深度监听器*

```html
<template>
    <div>
        <p>Watch监听器</p>
        <button @click="fun">点击</button>
        {{ info }}
    </div>
</template>

<script>

export default {
    name: 'Watch',
    data() {
        return {
            info: { name: "小明", age: 22 }
        }
    },
    watch: {
        // 深度监听 、 立即执行
        info: {
            handler: function (newValue, oldValue) {
                console.log("新值", newValue, "旧值", oldValue);
            },
            deep: true , // deep为true的时候，就是深度监听
            immediate: true // 在新加载页面的时候，监听没执行，如果需要在页面渲染的时候，一定执行一次，就要用立即执行
        }
    },
    methods: {
        fun() {
            this.info.name = "小花";
        }
    }
}
</script>
```

## 3、当监听元素改变，触发methods里面的方法

```html
<template>
    <div>
        <p>Watch监听器</p>
        <button @click="fun">点击</button>
        {{ info }}
    </div>
</template>

<script>

export default {
    name: 'Watch',
    data() {
        return {
            info: 0
        }
    },
    watch: {
        info: "change",
    },
    methods: {
        fun() {
            this.info++;
        },
        change(newValue, oldValue) {
            console.log("新值", newValue, "旧值", oldValue);
        }
    }
}
</script>
```

## 4、监听元素改变，触发多个方法

注：这样会逐一被调用。

```html
<template>
    <div>
        <p>Watch监听器</p>
        <button @click="fun">点击</button>
        {{ info }}
    </div>
</template>

<script>
export default {
    name: 'Watch',
    data() {
        return {
            info: 0
        }
    },
    watch: {
        // 深度监听 、 立即执行
        info: [
            "change",
            {
                handler: function (newValue, oldValue) {
                    console.log("新值", newValue, "旧值", oldValue);
                },
                deep: true, // deep为true的时候，就是深度监听
                immediate: true   // 在新加载页面的时候，监听没执行，如果需要在页面渲染的时候，一定执行一次，就要用立即执行
            }
        ]
    },
    methods: {
        fun() {
            this.info ++;
        },
        change(newValue, oldValue) {
            console.log("新值", newValue, "旧值", oldValue);
        }
    }
}
</script>
```

## 5、在页面初始化的时候，告诉watch你要监听谁

*注：this.$watch()里面传三个参数（第三个按需要传入），第一个监听的对象，第二个回调函数，第三个参数是配置选项（配置选项以对象的方式传入）， 同时他还有一个返回值，当你调用这个返回值的时候，你可以取消 监听。*

```html
<template>
    <div>
        <p>Watch监听器</p>
        <button @click="fun">点击</button>
        <button @click="cancleWatch">取消监听</button>
        {{ info }}
    </div>
</template>
<script>
export default {
    name: 'Watch',
    data() {
        return {
            info: { name: "小明", age: 22 },
            cancel: null
        }
    },
    methods: {
        fun() {
            this.info.age++;
        },
        cancleWatch() {
            // 取消监听
            this.cancel();
        }
    },
    created() {
        // 里面传三个参数，第一个监听的对象，第二个回调函数, 第三个参数是配置选项（配置选项以对象的方式传入），同时他还有一个返回值，当你调用这个返回值的时候，你可以取消 监听
        this.cancel = this.$watch("info", (newValue, oldValue) => { // 这里可以写箭头函数，你也可以不写箭头
            console.log("新值", newValue, "旧值", oldValue);
        }, {
            deep: true,
            immediate: true
        })
    }
}
</script>
```

# 四、组件通讯

## 1、父组件给子组件传值

官网参考：<https://v3.cn.vuejs.org/guide/component-props.html>

### 1）子组件以数组的方式接受传值

注：这样传值没办法定义类型、默认值等，一般不使用。

① 父组件

```javascript
<template>
    <div>
        <son-one title="哈哈哈" content="你好!我是哈哈哈"></son-one>
        <son-one :title="title" :content="content"></son-one>
        <!-- 简便的方式 -->
        <son-one v-bind="obj"></son-one>

    </div>
</template>
<script>
import SonOne from "./SonOne.vue"
export default {
    components: {
        SonOne
    },
    data() {
        return {
            title: "呵呵",
            content: "你好!我是呵呵",
            /**
             * 如果是对象，可以使用这个简便的方法
             */
            obj: {
                title: "拉拉",
                content: "你好!我是拉拉"
            }
        }
    },
}
</script>
```

② 子组件

```javascript
<template>
    <div>
        <p>{{title}}</p>
        <p>{{content}}</p>
    </div>
</template>
<script>

export default {
    props: ["title", "content"],
    components: {

    },
    data() {
        return {

        }
    },
}
</script>
```

 ![image-20211020001808816](https://i.loli.net/2021/10/20/BZFjm6cG2yVHstu.png)

### 2）对象的用法

 ![image-20211020002003424](./images/202203260137368.png)

 ![image-20211020002347543](./images/202203260146138.png)

① 父组件

```javascript
<template>
    <div>
        <son-one title="哈哈哈" content="你好!我是哈哈哈"></son-one>
        <son-one :title="title" ></son-one>
        <!-- 简便的方式 -->
        <son-one v-bind="obj"></son-one>
    </div>
</template>
<script>
import SonOne from "./SonOne.vue"
export default {
    components: {
        SonOne
    },
    data() {
        return {
            title: "呵呵",
            content: "你好!我是呵呵",
            /**
             * 如果是对象，可以使用这个简便的方法
             */
            obj: {
                title: "拉拉",
                content: "你好!我是拉拉"
            }
        }
    },
}
</script>
```

② 子组件

```javascript
<template>
    <div>
        <p>{{title}}</p>
        <p>{{content}}</p>
    </div>
</template>
<script>

export default {
    props: {
        title: String,
        /**
         * 指定更多的东西
         */
        content: {
            /* 指定类型 */
            type:String,
            /**
             * 必传的属性
             * 默认值： false
             */
            required: true,
            /* 定义 一个在不穿的时候的默认值，当然 必传的属性 和 默认值二选一 */
            default:"默认值"
        }
    }
}
</script>
```

### 3）类型都有哪些

String

Number

Boolean

Array

Object

Date

Function

Symbo

① 父组件

```javascript
<template>
    <div>
        <!-- 在html中大小写 不敏感，在html中，都会吧大写转换成小写
        但是 vue-loader 替你做了操作，所以传值的时候，可以写成大写
        但是遇见大写的建议使用 - 结合起来 
        -->
        <son-one str="哈哈哈" :num="1" multiple-types="你好！" custom-verification="哈哈" :fun="fun"></son-one>
    </div>
</template>
<script>
import SonOne from "./SonOne.vue"
export default {
    components: {
        SonOne
    },
    data() {
        return {
            title: "呵呵",
            content: "你好!我是呵呵",
            /**
             * 如果是对象，可以使用这个简便的方法
             */
            obj: {
                title: "拉拉",
                content: "你好!我是拉拉"
            }
        }
    },
    methods: {
        fun(){
            console.log("传入的函数");
        }
    },
}
</script>
```

②子组件

```javascript
<template>
    <div>
        <p>{{ str }}</p>
        <p>{{ num }}</p>
        <p>{{ multipleTypes }}</p>
        <p>{{ obj }}：{{ obj.str }}</p>
        <p>{{ customVerification }}</p>
    </div>
</template>
<script>

export default {
    props: {
        /* 字符串 */
        str: String,
        /* 数字带有默认值 */
        num: {
            /* 指定类型 */
            type: Number,
            /* 定义 一个在不穿的时候的默认值，当然 必传的属性 和 默认值二选一 */
            default: 1
        },
        /* 多个类型判断，就是可以传入多种类型 */
        multipleTypes: [Number, String],
        /**
         * 带有默认值的对象(引用类型)
         * 这个时候你必须要return，负责就会产生指向的问题，一个组件的值改变，其他的引用组件的值 也会改变
         */
        obj: {
            type: Object,
            // 这样 他每个都是一个独立的对象
            default() {
                return { str: "哈哈哈", num: 1 }
            }
        },
        /**
         * 自定义验证函数
         * 可以验证你传入的值，是否符合要求
         * 下面这样写：代表你传入的值 只能是这三个字符串中的一个
         */
        customVerification: {
            validator(val) {
                return ["你好", "哈哈", "呵呵"].includes(val);
            }
        },
        /**
         * 具有默认值的函数
         */
        fun: {
            type: Function,
            default() {
                return "Default function"
            }
        }
    },
    created() {
        this.fun();
    },
    data() {
        return {

        }
    },
}
</script>
```

 ![image-20211020005207084](./images/202203260147630.png)

## 2、非 Prop 的 Attribute

<https://v3.cn.vuejs.org/guide/component-attrs.html>

注：也就是传递给组件某个属性，如：style、id、class。

### 1）如果我有根节点的话（单个根节点）

注：也就是子组件中有根元素

![image-20211024135612644](https://i.loli.net/2021/10/24/QPHn5x9EVFTd1t4.png)

直接在使用的时候，在标签里面加入样式。

① 你也可以禁用子组件继承父组件的样式，写法如下：

 ![image-20211024135937497](./images/202203260147207.png)

② 你也可以指定位置继承样式，写法如下：

![无标题](https://gitee.com/Green_chicken/picture/raw/master/Pt1NTBmrWjYQhVZ.png)

③ 一次传入多个属性的写法：

a、父组件

```javascript
<template>
    <div>
        <!-- 在html中大小写 不敏感，在html中，都会吧大写转换成小写
        但是 vue-loader 替你做了操作，所以传值的时候，可以写成大写
        但是遇见大写的建议使用 - 结合起来 
        -->
        <son-one str="哈哈哈" :num="1" multiple-types="你好！" custom-verification="哈哈" :fun="fun" class="box" id="only"></son-one>
    </div>
</template>
<script>
import SonOne from "./SonOne.vue"
export default {
    components: {
        SonOne
    },
    data() {
        return {
            title: "呵呵",
            content: "你好!我是呵呵",
            /**
             * 如果是对象，可以使用这个简便的方法
             */
            obj: {
                title: "拉拉",
                content: "你好!我是拉拉"
            }
        }
    },
    methods: {
        fun(){
            console.log("传入的函数");
        }
    },
}
</script>
```

b、子组件

```javascript
<template>
    <div>
        <p v-bind="$attrs">{{ str }}</p>
        <p>{{ num }}</p>
        <p>{{ multipleTypes }}</p>
        <p>{{ obj }}：{{ obj.str }}</p>
        <p>{{ customVerification }}</p>
    </div>
</template>
<script>

export default {
    // 关闭继承过来 
    inheritAttrs: false,
    props: {
        /* 字符串 */
        str: String,
        /* 数字带有默认值 */
        num: {
            /* 指定类型 */
            type: Number,
            /* 定义 一个在不穿的时候的默认值，当然 必传的属性 和 默认值二选一 */
            default: 1
        },
        /* 多个类型判断，就是可以传入多种类型 */
        multipleTypes: [Number, String],
        /**
         * 带有默认值的对象(引用类型)
         * 这个时候你必须要return，负责就会产生指向的问题，一个组件的值改变，其他的引用组件的值 也会改变
         */
        obj: {
            type: Object,
            // 这样 他每个都是一个独立的对象
            default() {
                return { str: "哈哈哈", num: 1 }
            }
        },
        /**
         * 自定义验证函数
         * 可以验证你传入的值，是否符合要求
         * 下面这样写：代表你传入的值 只能是这三个字符串中的一个
         */
        customVerification: {
            validator(val) {
                return ["你好", "哈哈", "呵呵"].includes(val);
            }
        },
        /**
         * 具有默认值的函数
         */
        fun: {
            type: Function,
            default() {
                return "Default function"
            }
        }
    },
    created() {
        this.fun();
    },
    data() {
        return {

        }
    },
}
</script>
<style scoped>
.box{
    color: red;
}
#only{
    font-size: 30px;
}
</style>
```

### 2）多个根节点

注：当子元素有多个根节点的时候，父元素传入一个class的时候，如果你不指定，他会报以下的错误：

![image-20211024141733760](https://i.loli.net/2021/10/24/agsuDTLbk9KpzyJ.png)

![image-20211024141920529](./images/202203260148777.png)

## 3、子组件给父组件传值

传值的时候，需要先在子组件定义给父组件传值的名字，写法有 数组、对象 两种方式。

### 1）数组的写法

① 子组件

```javascript
<template>
    <div>
        <button @click="add">+1</button>
        <button @click="reduce">-1</button>
        <br />
        自定义加几：<input type="text" v-model.number="num">
        <button @click="customize">+{{num}}</button>
    </div>
</template>
<script>

export default {
    name: 'CounterOperation',
    components: {

    },
    /**
     * 1、告诉你要在子触发夫的事件：
     * 写法：1、一般写数组； 2、也可以写对象
     */
    emits:["add", "sub", "customize"],
    data() {
        return {
            num: 0
        }
    },
    methods: {
        add(){
            // console.log("+1");
            /**
             * 2、要触发的事件
             */
            this.$emit("add");
        },
        reduce(){
            // console.log("-1");
            this.$emit("sub");
        },
        customize(){
            /**
             * 参数传递的写法
             * 这块是可以传多个参数
             */
            this.$emit("customize", this.num);
        }
    },
}
</script>
```

② 父组件

```javascript
<template>
    <div>
        计算器：{{ counter }}
        <counter-operation @add="addOne" @sub="subOne" @customize="customize" />
    </div>
</template>
<script>
import CounterOperation from "./CounterOperation.vue";
export default {
    name: 'index',
    components: {
        CounterOperation
    },
    data() {
        return {
            counter:0
        }
    },
    methods: {
        addOne(){
            this.counter++
        },
        subOne(){
            this.counter--
        },
        customize(num){
            // console.log(num);
            this.counter += num;
        }
    },
}
</script>
```

### 2）对象的写法，对象的写法，可以验证传值

① 子组件

```javascript
<template>
    <div>
        <button @click="add">+1</button>
        <button @click="reduce">-1</button>
        <br />
        自定义加几：<input type="text" v-model.number="num">
        <button @click="customize">+{{num}}</button>
    </div>
</template>
<script>

export default {
    name: 'CounterOperation',
    components: {

    },
    /**
     * 1、告诉你要在子触发夫的事件：
     * 写法：1、一般写数组； 2、也可以写对象
     */
    // 对象写法 可以对参数进项验证
    emits: {
        // null 空的代表 不需要参数类型的验证
        add: null,
        sub: null,
        /**
         * 有几个参数，这里就写几个
         * 在这进行参数的验证，验证成功 return true
         * 注：验证失败依然会穿过去
         * 可以要求参数大于几，他才可以穿传过去，在不匹配的时候，他只会 警告，不会报错
         */ 
        customize: payload => {
            console.log(payload);
            return true
        }
    },
    data() {
        return {
            num: 0
        }
    },
    methods: {
        add(){
            // console.log("+1");
            /**
             * 2、要触发的事件
             */
            this.$emit("add");
        },
        reduce(){
            // console.log("-1");
            this.$emit("sub");
        },
        customize(){
            /**
             * 参数传递的写法
             * 这块是可以传多个参数
             */
            this.$emit("customize", this.num);
        }
    },
}
</script>
```

② 父组件

```javascript
<template>
    <div>
        计算器：{{ counter }}
        <counter-operation @add="addOne" @sub="subOne" @customize="customize" />
    </div>
</template>
<script>
import CounterOperation from "./CounterOperation.vue";
export default {
    name: 'index',
    components: {
        CounterOperation
    },
    data() {
        return {
            counter:0
        }
    },
    methods: {
        addOne(){
            this.counter++
        },
        subOne(){
            this.counter--
        },
        customize(num){
            this.counter += num;
        }
    },
}
</script>
```

## 4、tabs切换案例

1）父组件

```javascript
<template>
    <div>
        <tabs :title="title" :contents="contents" @throw="throw" />
        <h1>{{contents[currentIndex]}}</h1>
    </div>
</template>
<script>
import tabs from "./tabs.vue"
export default {
    name: 'index',
    components:{
        tabs
    },
    data() {
        return{
            title:["衣服", "鞋子", "包"],
            contents: ["衣服页面", "鞋子页面", "包页面"],
            currentIndex: 0
        }
    },
    methods: {
        throw(index){
            this.currentIndex = index;
        }
    },
}
</script>
```

2）子组件

```javascript
<template>
    <div>
        <div v-for="(item,index) in title" :key="index" class="item" :class="{active: currentIndex === index}" @click="fun(index)">
            <span>{{item}}</span>
        </div>
    </div>
</template>
<script>

export default {
    name: 'tabs',
    props: {
        title: Array
    },
    components: {

    },
    data() {
        return {
            currentIndex: 0
        }
    },
    methods: {
        fun(index){
            this.currentIndex = index;
            this.$emit("throw", this.currentIndex)
        }
    },
}
</script>

<style scoped>
div {
    display: flex;
}
.item {
    flex: 1;
    text-align: center;
}
.item > span {
    width: 100%;
}
.active {
    color: red;
}
.active > span {
    padding: 0 0px 5px;
    border-bottom: 3px solid red;
}
</style>
```

## 5、非父子组件的通讯

注：也就是孙子组件去使用爷爷组件的数据，这个时候，就要使用 <font color=red>Provide和Inject</font> 。

1）使用

①爷爷组件

```javascript
<template>
    <div>
        爷爷组件
        <hr />
        <father />
    </div>
</template>
<script>
import father from "./father.vue";
import { computed } from "vue"
export default {
    name: 'grandfather',
    components: {
        father
    },
    data() {
        return {
            num: 9
        }
    },
    /**
     * 定义数据
     * 但是 只是给子孙组件里面使用，当前 组件不能使用 
     * 注：1、需要写成函数的方式，不能写成对象的方法，负责 有this指向的问题
     *    2、赋值的时候，他不会去把改变时候的值给赋值过去的，所以就要使用计算属性，但是 使用方式有可能与之前的不同
     */
    /*
    provide: {
        obj: {
            name: "里斯",
            age: 26
        }
    }
    */
    provide() {
        return {
            /**
             * 但是这样写，会返回一个对象，所以要使用 .value
             */
            num: computed(() => {
                return this.num
            }),
            obj: {
                name: "里斯",
                age: 26
            }
        }
    }
}
</script>
```

② 父组件

```javascript
<template>
    <div>
        夫组件
        <hr />
        <son />
    </div>
</template>
<script>
import son from "./son.vue"
export default {
    name: 'grandfather',
    components:{
        son
    },
}
</script>
```

③ 子组件

```javascript
<template>
    <div>
        孙子组件
        <p>{{obj.name}}</p>
        <p>{{obj.age}}</p>
        <p>{{num.value}}</p>
    </div>
</template>
<script>

export default {
    name: 'grandfather',
    /**
     * 使用数据
     */
    inject:["obj", "num"],
}
</script>
```

## 6、全局事件总线的使用

注：Vue3从实例中移除了 $on、$off 和 $once 方法，所以我们如果希望继续使用全局事件总线，要通过第三方的库。

1）mitt下载：

```bash
npm install mitt
```

2）使用第三方库的时，尽量封装成一个组件

3）下面实现一个在跨父子组件的通信

![无标题](https://gitee.com/Green_chicken/picture/raw/master/3FMV1uckiGhaOgl.png)

①封装一个桥梁

```javascript
/**
 * 开发中这个放到utils里面
 */

// 1、引入
import mitt from "mitt";
/**
 * 2、使用
 * 这款i可以写多个
 */
const emitter = mitt();
export default emitter;
```

![image-20211030170906608](./images/202203260148971.png)

# 五、slot的使用

## 1、插槽的默认内容

① 引用组件

```javascript
<template>
    <div>
        <p>solt的使用</p>
        <translate>
            哈哈哈
        </translate>
        <translate />
    </div>
</template>
<script>
import Translate from "./Translate.vue";
export default {
    name: 'index',
    components:{
        Translate
    }
}
</script>
```

② solt的组件

```javascript
<template>
    <div>
        <!-- 默认内容是在不传值的时候，使用的 -->
        <slot>
            默认的内容
        </slot>
    </div>
</template>
<script>

export default {
    name: 'Translate',
}
</script>
```

 ![image-20211031144613603](./images/202203260149956.png)

## 2、具名插槽

<font color=red>注：插槽有一个默认的名字：name = "default"。</font>

```javascript
<!-- 插槽的默认名字 -->
<solt name="default"></solt>
```

① 具名的插槽

```javascript
<template>
    <div class="box">
        <!-- 默认内容是在不传值的时候，使用的 -->
        <div class="left">
            <slot name="left">
                默认的内容
            </slot>
        </div>
        <div class="right">
            <slot name="right">
                默认的内容
            </slot>
        </div>

    </div>
</template>
<script>

export default {
    name: 'Translate',
}
</script>
<style scoped>
.box {
    display: flex;
    width: 100%;
}
.left,
.right {
    width: 50%;
    height: 200px;
}
.left{
    background-color: red;
}
.right{
    background-color: royalblue;
}
</style>
```

② 使用具名插槽

```javascript
<template>
    <div>
        <p>solt的使用</p>
        <translate>
            <template v-slot:left>
                左边的
            </template>
            <template v-slot:right>
                右边的
            </template>
        </translate>
    </div>
</template>
<script>
import Translate from "./Translate.vue";
export default {
    name: 'index',
    components:{
        Translate
    }
}
</script>
```

![image-20211031150422170](./images/202203260149741.png)

## 3、动态插槽名

注：封装的高级组件，插槽的名字（name）就不是写死的。

① 使用

```javascript
<template>
    <div>
        <p>solt的使用</p>
        
        <translate :name="name">
            <template v-slot:right>
                具名的插槽
            </template>
            <!-- 传入的写法 -->
            <template v-slot:[name]>
                动态插槽，传入不同的类名
            </template>
        </translate>
    </div>
</template>
<script>
import Translate from "./Translate.vue";
export default {
    name: 'index',
    components: {
        Translate
    },
    data() {
        return {
            name: "left"
        }
    },
}
</script>
```

② 插槽组件

```javascript
<template>
    <div class="box">
        <p>
            <slot name="right"></slot>
        </p>
        <!-- 动态插槽 -->
        <p>
            <slot :name="name"></slot>
        </p>
    </div>
</template>
<script>

export default {
    name: 'Translate',
    props: {
        name: {
            type: String,
            default() {
                /**
                 * 插槽的默认name
                 */
                return "default"
            }
        }

    }
}
</script>
```

![image-20211031152402708](./images/202203260150030.png)

## 4、插槽的缩写

 ![image-20211031152612040](./images/202203260150236.png)

## 5、作用域插槽

注：也就是 在父组件中使用子组件的变量。

![image-20211031234417575](./images/202203260241710.png)

1）不指定插槽名字：

① 父组件

```javascript
<template>
    <div>
        <p>solt的使用</p>
        
        <translate :tabs="tab">
            <!-- 2、使用，在父组件中获取 -->
            <template v-slot="slotScope">
                <div>
                    {{ slotScope.item }} —— {{ slotScope.index }}
                </div>                
            </template>
        </translate>
    </div>
</template>
<script>
import Translate from "./Translate.vue";
export default {
    name: 'index',
    components: {
        Translate
    },
    data() {
        return {
            tab:[
                {name:"标签一"},
                {name:"标签二"},
                {name:"标签三"},
                {name:"标签四"}
            ]
        }
    },
}
</script>
```

② 子组件

```javascript
<template>
    <div class="box">
        <template v-for="(item,index) in tabs" :key="index">
            <!-- 1、定义插槽的时候，放入属性，然后使用 -->
            <slot :item="item" :index="index"></slot>
        </template>
    </div>
</template>
<script>

export default {
    name: 'Translate',
    props: {
        tabs: {
            type: Array,
            default() {
                return []
            }
        }
    }
}
</script>
```

2）指定插槽名字

① 父组件

```javascript
<template>
    <div>
        <p>solt的使用</p>
        
        <translate :tabs="tab">
            <!-- 2、使用，在父组件中获取 -->
            <template v-slot:one="slotScope">
                <div>
                    {{ slotScope.item }} —— {{ slotScope.index }}
                </div>                
            </template>
        </translate>
    </div>
</template>
<script>
import Translate from "./Translate.vue";
export default {
    name: 'index',
    components: {
        Translate
    },
    data() {
        return {
            tab:[
                {name:"标签一"},
                {name:"标签二"},
                {name:"标签三"},
                {name:"标签四"}
            ]
        }
    },
}
</script>
```

② 子组件

```javascript
<template>
    <div class="box">
        <template v-for="(item,index) in tabs" :key="index">
            <!-- 1、定义插槽的时候，放入属性，然后使用 -->
            <slot name="one" :item="item" :index="index"></slot>
        </template>
    </div>
</template>
<script>

export default {
    name: 'Translate',
    props: {
        tabs: {
            type: Array,
            default() {
                return []
            }
        }
    }
}
</script>
```

# 六、动态组件

## 1、基本使用

实现一个tab的案例：

![image-20211102233043902](./images/202203260242560.png)

 ![chrome-capture](https://gitee.com/Green_chicken/picture/raw/master/chrome-capture.gif)

## 2、传值和传递事件

①父组件

```javascript
<template>
    <div>
        <p>动态组件的使用</p>
        <button v-for="item in tabs" :key="item.lable" @click="fun(item)" :class="{ active: com === item.lable }">
            {{ item.value }}
        </button>
        <component :is="com" :num="99" @throw="throw"></component>
    </div>
</template>
<script>
import TabTwo from "./TabTwo.vue"
import TabOne from "./TabOne.vue"
export default {
    name: 'index',
    components: {
        TabOne,
        TabTwo
    },
    data() {
        return {
            tabs: [{ value: "组件一", lable: "TabOne" }, { value: "组件一", lable: "TabTwo" }],
            com: "TabOne"
        }
    },
    methods: {
        fun(obj) {
            this.com = obj.lable
        },
        throw(val) {
            console.log(val);
        }
    }
}
</script>

<style scoped>
.active {
    border: 1px solid rebeccapurple;
}
button {
    background-color: transparent;
    border: 0;
    margin: 0 10px;
}
</style>
```

② 组件一（传值组件）

```javascript
<template>
    <div>
        <p>动态组件一</p>
        {{num}}
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
    }
}
</script>
```

③  组件二（子给夫抛出事件）

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

# 七、webpack分包

## 1、默认打包过程

1）默认情况下，在构建整个组件树的过程中，因为组件和组件之间是通过模块化直接依赖的，那么webpack在打包时就会将组 件模块打包到一起（比如一个app.js文件中）；

2）这个时候随着项目的不断庞大，app.js文件的内容过大，会造成首屏的渲染速度变慢。

## 2、打包时，代码的分包

1）对于一些不需要立即使用的组件，我们可以单独对它们进行拆分，拆分成一些小的代码块chunk.js；

2）对于一些不需要立即使用的组件，我们可以单独对它们进行拆分，拆分成一些小的代码块chunk.js。

## 3、wenpack在vue中的分包

注：

<font color=red >通过import函数导入的模块，后续webpack对其进行打包的时候会进行分包操作，分包之后 就不会打进app.js了。</font>

```bash
/**
 * 
 * 分包的使用
 */
import("@/utils/subcontract").then(res => {
    console.log(res.operation(10, 10));

})
```

 ![image-20211106173750209](./images/202203260243795.png)

# 八、vue中实现异步组件（defineAsyncComponent）

在项目中，如果我们 需要异步加载某些组件的时候，就可以使用defineAsyncComponent。

文档：<https://v3.cn.vuejs.org/api/global-api.html#definecomponent>

## 1、defineAsyncComponent接受两种参数

### 1）工厂函数，该工厂函数需要返回一个Promise对象

![image-20211106174925721](./images/202203260243459.png)

```javascript
<template>
    <div>
        <Home />
        <About />
    </div>
</template>
<script>
import { defineAsyncComponent } from "vue";
import Home from "./views/Home.vue";
const About = defineAsyncComponent(() => import("./views/About.vue"))
export default {
    components:{
        Home,
        About
    }
}
</script>
```

### 2）接受一个对象类型，对异步函数进行配置

```javascript
<template>
    <div>
        <Home />
        <About />
    </div>
</template>
<script>
import { defineAsyncComponent } from "vue";
import Home from "./views/Home.vue";
import Loading from "@/components/Loading.vue";
// const About = defineAsyncComponent(() => import("./views/About.vue"))
const About = defineAsyncComponent({
    loader: () => import("./views/About.vue"),
    /**
     * 他是一个异步的组件：
     * loadingComponent 配置一个展位组件，也就是loading
     * errorComponent 当组件加载失败的时候，你展示的组件
     * delay 延迟组件展示,在现实loadingComponent 组件前的延迟
     * onError 是监听错误，也就是  异步组件加载失败的监听
     */
    loadingComponent: Loading,
    delay: 200,
    /**
     * onError 参数含义
     * err 错误信息
     * retry 函数，调用retry 尝试重新加载
     * fail  一个函数，指示加载程序结束退出
     * attempts 记录尝试次数
     */
    onError: function(err, retry, fail, attempts){
        console.log(err, retry, attempts);
    }
})
export default {
    components:{
        Home,
        About
    }
}
</script>
```

## 2、异步组件和Suspense

1）Suspense是一个内置的全局组件，该组件有两个插槽：

① default：如果default可以显示，那么显示default的内容；

② fallback：如果default无法显示，那么会显示fallback插槽的内容。

2）使用方式如下：

![image-20211107211831619](./images/202203260243382.png)

# 九、ref的使用

1、获取元素

1）在Vue开发中我们是不推荐进行DOM操作的；

2）给元素或者组件绑定一个ref的attribute属性。

2、组件实例有一个$refs属性

它一个对象Object，持有注册过 ref attribute 的所有 DOM 元素和组件实例。

注：在组件上 定义ref，可以获取组件里面的方法、变量、DOM等。

① 父组件

```javascript
<template>
    <div>
        <p ref="try">父组件</p>
        <zu-jian ref="zujian" />
    </div>
</template>
<script>
import ZuJian from './ZuJian.vue';

export default {
    components: { ZuJian },
    name: 'index',
    mounted() {
        console.log(this.$refs.try);
        console.log(this.$refs.zujian);
        console.log(this.$refs.zujian.num);
        console.log(this.$refs.zujian.fun(1));
    },
}
</script>
```

② 子组件

```javascript
<template>
    <div>
        组件
    </div>
</template>
<script>

export default {
    name: 'ZuJian',
    components:{
        
    },
    data() {
        return{
            num: 10
        }
    },
    methods: {
        fun(val){
            console.log(val);
            return val
        }
    },
}
</script>
```

# 十、$parent和$root

## 1、通过$parent来访问父元素

① 子组件

```javascript
<template>
    <div>
        组件
        <button @click="fun1">获取父组件</button>
    </div>
</template>
<script>

export default {
    name: 'ZuJian',
    components:{
        
    },
    data() {
        return{
            num: 10
        }
    },
    methods: {
        fun(val){
            console.log(val);
            return val
        },
        fun1(){
            console.log(this.$parent.obj);
        }
    },
}
</script>
```

② 父组件

```javascript
<template>
    <div>
        <p ref="try">父组件</p>
        <zu-jian ref="zujian" />
    </div>
</template>
<script>
import ZuJian from './ZuJian.vue';

export default {
    components: { ZuJian },
    name: 'index',
    data() {
        return {
            obj: {
                name: "小明"
            }
        }
    },
    mounted() {
        console.log(this.$refs.try);
        console.log(this.$refs.zujian);
        console.log(this.$refs.zujian.num);
        console.log(this.$refs.zujian.fun(1));
    },
}
</script>
```

## 2、以通过$root获取跟组件，也就是 App.vue

 ![image-20211107221101517](./images/202203260244164.png)

# 十一、组件的v-model

## 1、API的使用

① 父组件

```javascript
<template>
    <div>
        <p>在元素上使用v-model</p>
        <input type="text" v-model="text"><br />
        <!-- 原理 -->
        <input type="text" :value="text" @input="text = $event.target.value">
        <hr />
        <p> 组件上使用v-model </p>
        {{textComponent}}
        <test-input v-model="textComponent" />
        <!-- 原理 -->
        <!-- <test-input :modelValue="textComponent" @updata:model-value="textComponent = $event" /> -->
    </div>
</template>
<script>
import TestInput from './TestInput.vue'

export default {
    name: 'index',
    components: {
        TestInput

    },
    data() {
        return {
            text: "",
            textComponent: "你好！"
        }
    },
}
</script>
```

② 子组件

```javascript
<template>
    <div>
        <button @click="fun">按钮</button>
        {{modelValue}}
    </div>
</template>
<script>

export default {
    name: 'TestInput',
    props: {
        modelValue: String 
    },
    emits: ["update:modelValue"],
    methods: {
        fun() {
            this.$emit("update:modelValue", "你好123！")
        }
    },
}
</script>
```

## 2、原理的实现

 计算属性的实现：

① 父组件

```javascript
<template>
    <div>
        <p> 组件上使用v-model </p>
        {{textComponent}}
        <!-- 原理 -->
        <test-input :modelValue="textComponent" @update:model-value="textComponent = $event" />
    </div>
</template>
<script>
import TestInput from './TestInput.vue'

export default {
    name: 'index',
    components: {
        TestInput

    },
    data() {
        return {
            textComponent: "你好！"
        }
    },
}
</script>
```

② 子组件

```javascript
<template>
    <div>
        <!-- <input type="text" :value="text" @input="fun"> -->
        <!-- 简单的写法 -->
        <input type="text" :value="text" @input="fun">
    </div>
</template>
<script>

export default {
    name: 'TestInput',
    props: {
        modelValue: String 
    },
    emits: ["update:modelValue"],
    computed: {
        text:{
            set(e){
                this.$emit("update:modelValue", e.target.value)
            },
            get(){
                // 读取props传进来的
                return this.modelValue
            }
        }
    },
    methods: {
        fun(e) {
            this.$emit("update:modelValue", e.target.value)
        }
    },
}
</script>
```

## 3、绑定多个

① 父组件

```javascript
<template>
    <div>
        <p> 组件上使用v-model </p>
        {{textComponent}}
        <br />
        {{title}}
        <!-- 绑定多个,v-model:名字 -->
        <test-input v-model="textComponent" v-model:title="title" />
    </div>
</template>
<script>
import TestInput from './TestInput.vue'

export default {
    name: 'index',
    components: {
        TestInput

    },
    data() {
        return {
            textComponent: "你好！",
            title: "呵呵呵"
        }
    },
}
</script>
```

② 子组件

```javascript
<template>
    <div>
        <!-- 传多个值，不同的展示 -->
        <input type="text" v-model="text">
        <input type="text" v-model="title1">
    </div>
</template>
<script>
export default {
    name: 'TestInput',
    props: {
        modelValue: String,
        title: String
    },
    // 事件
    emits: ["update:modelValue", "update:title"],
    computed: {
        text:{
            set(e){
                this.$emit("update:modelValue", e)
            },
            get(){
                // 读取props传进来的
                return this.modelValue
            }
        },
        // 计算属性 和 props 名字不能重复
        title1:{
            set(e){
                this.$emit("update:title", e)
            },
            get(){
                // 读取props传进来的
                return this.title
            }
        },

    }
}
</script>
```
