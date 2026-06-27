文档：<https://next.vuex.vuejs.org/zh/>

提供一个Vue devtools谷歌插件安装包：<https://wwe.lanzouw.com/inHnkyjawwh>

# 一、Vuex认识

Vuex 是一个专为 Vue.js 应用程序开发的 状态管理模式 + 库。它采用集中式存储管理应用的所有组件的状态，并以相应的规则保证状态以一种可预测的方式发生变化。

 <img src="https://next.vuex.vuejs.org/vuex.png" style="zoom:50%;" />

# 二、安装和初步使用

## 1、安装

```bash
# npm
npm install vuex@next --save
# yarn
yarn add vuex@next --save
```

## 2、创建一个 store（仓库）

1）在根目录 ——> src ——> c（文件夹）

2）在store下创建index.js文件（这个文件提供一个初始 state 对象和一个mutation）

```javascript
import { createStore } from 'vuex'
/**
 * 创建仓库和导出
 */
export default createStore({
    state: {
        num: 111
    }
})
```

## 3、对这个仓库进行一个导入（在 main.js 中导入）

```javascript
import { createApp } from 'vue'
import App from './App.vue'
import store from './store/index.js'

const app = createApp(App)
app.use(store)
app.mount('#app')
```

## 4、在页面或组件中使用

```html
<template>
    vuex
    <p>{{$store.state.num}}</p>
</template>
```

# 三、Vuex中核心概念的使用

## 1、State

store本质上是一个容器，它包含着你的应用中大部分的状态。

注：对State里面的数据修改，必须要通过Mutation来进行一个提交。

1）在state中储存数据

```javascript
import { createStore } from 'vuex'
/**
 * 创建仓库和导出
 */
export default createStore({
    state: {
        num: 111
    }
})
```

2）在页面或者组件中使用

```javascript
<template>
    vuex
    <!-- 1、直接取用 -->
    <p>{{ $store.state.num }}</p>
    <p>{{ num }}</p>
</template>
<script>
import { computed } from "vue"
import { useStore, mapState } from "vuex"
export default {
    setup() {
        const store = useStore()
        /**
        * 2、使用计算属性的方式
        */
        const num = computed(() => store.state.num)
        return { num }
    }
}
</script>
```

## 2、mapState

1）vuex状态管理中的数据(src——>store——>index.js)

```javascript
import { createStore } from 'vuex'
/**
 * 创建仓库和导出
 */
export default createStore({
    state: {
        num: 111,
        name: "小明",
        age: 22
    }
})
```

2）在页面或者组件中使用

① 数组的方式，并且没有在当前页面定义computed属性

```vue
<template>
    vuex
    <!-- 1、直接取用 -->
    <p>{{$store.state.num}}</p>
    <p>{{name}}</p>
    <p>{{age}}</p>
</template>
<script>
import { mapState } from "vuex"
export default {
    /**
     * 2、使用计算属性的方式
     * computed 要求传入一个对象
     * mapState 返回的是一个对象
     * 所以  可以按照下面这样来写
     */
    computed: mapState(["name", "age"]),

}
</script>
```

② 在页面还定义了其他的计算属性时，使用扩展运算符

```vue
<template>
    vuex
    <!-- 1、直接取用 -->
    <p>{{$store.state.num}}</p>
    <p>{{num}}</p>
    <p>{{name}}</p>
    <p>{{age}}</p>
</template>
<script>
import { mapState } from "vuex"
export default {
    /**
     * 2、使用计算属性的方式
     * computed 要求传入一个对象
     * mapState 返回的是一个对象
     * 所以  可以按照下面这样来写
     */
    computed: {
        num() {
            return this.$store.state.num
        },
        // 把映射出来的对象，全部放到了computed里
        ...mapState(['name', "age"])
    }
}
</script>
```

③ 对象的写法（使用对象，可以自定义key）

```vue
<template>
    vuex
    <!-- 1、直接取用 -->
    <p>{{$store.state.num}}</p>
    <p>{{num}}</p>
    <p>{{name}}</p>
    <p>{{ages}}</p>
</template>
<script>
import { mapState } from "vuex"
export default {
    /**
     * 2、使用计算属性的方式
     * computed 要求传入一个对象
     * mapState 返回的是一个对象
     * 所以  可以按照下面这样来写
     */
    computed: {
        num() {
            return this.$store.state.num
        },
        ...mapState({
            name: state => state.name,
            // 自定义名字，在有变量冲突的时候，可以使用
            ages: state => state.age,
        })
    }
}
</script>
```

## 3、在setup中mapState的使用

store——>index.js里面的状态

```javascript
import { createStore } from 'vuex'
/**
 * 创建仓库和导出
 */
export default createStore({
    state: {
        num: 111,
        name: "小明",
        age: 22
    }
})
```

1）基本使用

```vue
<template>
    vuex
    <!-- 1、直接取用 -->
    <p>{{ $store.state.num }}</p>
    <p>{{ num }}</p>
    <p>{{ name }}</p>
    <p>{{ age }}</p>
</template>
<script>
import { computed } from "vue"
import { useStore, mapState } from "vuex"
export default {
    setup() {
        const store = useStore()
        const num = computed(() => store.state.num)
        // mapState 在 setup 中的使用
        const arrFn = mapState(['name', "age"])
        const storeState = {}
        // 拿到arr中所有的key
        Object.keys(arrFn).forEach(fnKey => {
            // 这里使用了 bind改变this指向
            const fn = arrFn[fnKey].bind({ $store: store })
            storeState[fnKey] = computed(fn)
        })
        return { num, ...storeState }
    }
}
</script>
```

2）<font color=red>将上面的使用封装，方便在以后使用</font>（推荐）

① src——> hook——> useStore.js（创建这个  文件）

```javascript
/**
 * 在setup中使用mapState的封装
 * @param {*} mapArr 数组 | 对象
 */
import { computed } from "vue"
import { useStore, mapState } from "vuex"
export default function useState(mapArr) {
    // 获取store
    const store = useStore()
    // mapState 在 setup 中的使用(在这获取对应的函数),mapState是自己支持对象和数组的
    const arrFn = mapState(mapArr)
    const storeState = {}
    // 拿到arrFn中所有的key
    Object.keys(arrFn).forEach(fnKey => {
        // 这里使用了 bind改变this指向
        const fn = arrFn[fnKey].bind({ $store: store })
        storeState[fnKey] = computed(fn)
    })
    return storeState
}
```

② 在页面使用

```vue
<template>
    vuex
    <p>{{ num }}</p>
    <p>{{ name }}</p>
    <p>{{ ages }}</p>
</template>
<script>
import useState from "@/hook/useStore.js"
export default {
    setup() {
        // 把想要获取的传入hook
        const storeState = useState(["num", "name"])
        const obj = useState({
            ages: state => state.age
        })
        return { ...storeState, ...obj }
    }
}
</script>
```

## 4、getters

Vuex 允许我们在 store 中定义“getter”（可以认为是 store 的计算属性）。

1）基本使用

① 数据源

```javascript
import { createStore } from 'vuex'
/**
 * 创建仓库和导出
 */
export default createStore({
    state: {
        name: "小明",
        age: 24,
        product: [
            { name: "面包", price: 10, count: 11 },
            { name: "火腿", price: 12, count: 2 },
            { name: "方便面", price: 13, count: 3 },
            { name: "牛奶", price: 14, count: 5 }
        ]
    },
    getters: {
        /**
         * state 获取state中的数据
         * getters 使用getters中其他的方法
         */
        totalPrice(state, getters) {
            let totalPrice = 0
            for (let i = 0; i < state.product.length; i++) {
                totalPrice += state.product[i].price
            }
            return totalPrice + getters.unit
        },
        unit() {
            return "元"
        }
    }
})
```

② 在页面或者组件中使用

```javascript
<template>
    <p> getters</p>
    <p>{{ $store.getters.totalPrice }}</p>
</template>
```

## 5、getters的返回函数（getters接受参数）

1）数据源

```javascript
import { createStore } from 'vuex'
/**
 * 创建仓库和导出
 */
export default createStore({
    state: {
        name: "小明",
        age: 24,
        product: [
            { name: "面包", price: 10, count: 11 },
            { name: "火腿", price: 12, count: 2 },
            { name: "方便面", price: 13, count: 3 },
            { name: "牛奶", price: 14, count: 5 }
        ]
    },
    getters: {
        /**
         * 这样就可以在外面传入函数
         */
        moreThanThe(state) {
            return function (n) {
                let totalPrice = 0
                for (let i = 0; i < state.product.length; i++) {
                    if (state.product[i].count > n) continue
                    totalPrice += state.product[i].price
                }
                return totalPrice
            }
        }
    }
})
```

2）使用

```vue
<template>
    <p>{{ $store.getters.moreThanThe(4) }}</p>
</template>
```

## 6、mapGetters辅助函数

1）数据源

```javascript
import { createStore } from 'vuex'
/**
 * 创建仓库和导出
 */
export default createStore({
    state: {
        name: "小明",
        age: 24,
        product: [
            { name: "面包", price: 10, count: 11 },
            { name: "火腿", price: 12, count: 2 },
            { name: "方便面", price: 13, count: 3 },
            { name: "牛奶", price: 14, count: 5 }
        ]
    },
    getters: {
        /**
         * state 获取state中的数据
         * getters 使用getters中其他的方法
         */
        totalPrice(state, getters) {
            let totalPrice = 0
            for (let i = 0; i < state.product.length; i++) {
                totalPrice += state.product[i].price
            }
            return totalPrice + getters.unit
        },
        unit() {
            return "元"
        },
        /**
         * 这样就可以在外面传入函数
         */
        moreThanThe(state) {
            return function (n: number) {
                let totalPrice = 0
                for (let i = 0; i < state.product.length; i++) {
                    if (state.product[i].count > n) continue
                    totalPrice += state.product[i].price
                }
                return totalPrice
            }
        }
    }
})
```

2）使用

① 数组的使用

```vue
<template>
    <p> getters</p>
    <p>{{ unit }}</p>
    <p>{{ totalPrice }}</p>
    <p>{{ nameInfo }}</p>
</template>

<script>
import { mapGetters } from "vuex"
export default {
    name: 'getters',
    computed: {
        unit() {
            return this.$store.getters.unit
        },
        // 数组的写法
        ...mapGetters(["totalPrice", "nameInfo"])
    }
}
</script>
```

② 对象的写法

```vue
<template>
    <p> getters</p>
    <p>{{ unit }}</p>
    <p>{{ totalPrice }}</p>
    <p>{{ nameInfo }}</p>
</template>

<script>
import { mapGetters } from "vuex"
export default {
    name: 'getters',
    computed: {
        unit() {
            return this.$store.getters.unit
        },
        // 以下  都可以写多个，演示时 写一个，我只是为了方便
        ...mapGetters(["totalPrice"]),
        ...mapGetters({
            nameInfo: "nameInfo"
        })
    }
}
</script>
```

## 7、在setup中使用getters及mapGetters辅助函数

1）数据源

```javascript
import { createStore } from 'vuex'
/**
 * 创建仓库和导出
 */
export default createStore({
    state: {
        name: "小明",
        age: 24,
        product: [
            { name: "面包", price: 10, count: 11 },
            { name: "火腿", price: 12, count: 2 },
            { name: "方便面", price: 13, count: 3 },
            { name: "牛奶", price: 14, count: 5 }
        ]
    },
    getters: {
        /**
         * state 获取state中的数据
         * getters 使用getters中其他的方法
         */
        totalPrice(state, getters) {
            let totalPrice = 0
            for (let i = 0; i < state.product.length; i++) {
                totalPrice += state.product[i].price
            }
            return totalPrice + getters.unit
        },
        unit() {
            return "元"
        },
        nameInfo(state) {
            return state.name
        }
    }
})
```

2）在src——> hook——> useGetters.js 封装这个文件

```javascript
/**
 * 在setup中使用mapState的封装
 * @param {*} mapArr 数组 | 对象
 */
import { computed } from "vue"
import { useStore, mapGetters } from "vuex"
export default function useGetters(mapArr) {
    // 获取store
    const store = useStore()
    // mapState 在 setup 中的使用(在这获取对应的函数),mapState是自己支持对象和数组的
    const arrFn = mapGetters(mapArr)
    const storeState = {}
    // 拿到arr中所有的key
    Object.keys(arrFn).forEach(fnKey => {
        // 这里使用了 bind改变this指向
        const fn = arrFn[fnKey].bind({ $store: store })
        storeState[fnKey] = computed(fn)
    })
    return storeState
}
```

3）使用

```vue
<template>
    <p> getters</p>
    <p>{{ unit }}</p>
    <p>{{ totalPrice }}</p>
    <p>{{ nameInfo }}</p>
</template>

<script>
import useGetters from "@/hook/useGetters.js"
export default {
    name: 'getters',
    setup() {
        const storeGettersFns = useGetters(["totalPrice", "unit", "nameInfo"])
        return { ...storeGettersFns }
    }
}
</script>
```

## 8、对setup中使用辅助函数进行封装

1）封装（在src——> hook——> vuexMap.js）

```javascript
/**
 * 在setup中使用mapState的封装
 * @param {*} mapArr mapState | mapGetters
 * @param {*} mapArr 数组 | 对象
 * 注：mapState、mapState 是使用的使用传入的
 */
import { computed } from "vue"
import { useStore } from "vuex"
export default function vuexMap(mapFun, mapArr) {
    // 获取store
    const store = useStore()
    // mapState 在 setup 中的使用(在这获取对应的函数),mapState是自己支持对象和数组的
    const arrFn = mapFun(mapArr)
    const storeState = {}
    // 拿到arr中所有的key
    Object.keys(arrFn).forEach(fnKey => {
        // 这里使用了 bind改变this指向
        const fn = arrFn[fnKey].bind({ $store: store })
        storeState[fnKey] = computed(fn)
    })
    return storeState
}
```

2）使用

```vue
<template>
    <p> getters</p>
    <p>{{ unit }}</p>
    <p>{{ totalPrice }}</p>
    <p>{{ nameInfo }}</p>
</template>

<script>
import vuexMap from "@/hook/vuexMap.js"
import { mapGetters } from 'vuex'
export default {
    name: 'getters',
    setup() {
        // 下面以mapGetters为例，mapState和他一样
        const storeGettersFns = vuexMap(mapGetters, ["totalPrice", "unit", "nameInfo"])
        return { ...storeGettersFns }
    }
}
</script>
```

## 9、Mutation

更改 Vuex 的 store 中的状态的唯一方法是提交 mutation。

1）使用

① store ——>index.js去修改state中的数据

```javascript
import { createStore } from 'vuex'
/**
 * 创建仓库和导出
 */
export default createStore({
    state: {
        counter: 0
    },
    mutations: {
        /**
         * mutations修改数据源
         * @param state 数据原
         * @param payload 外部调用时 传入的参数
         */
        add(state, payload) {
            state.counter += payload
        },
        reduce(state) {
            state.counter--
        }
    }
})
```

② 在页面中使用

a、常用的风格

```vue
<template>
    <p>mutation</p>
    <h1>{{$store.state.counter}}</h1>
    <button @click="add">+1</button>
    <button @click="reduce">-1</button>
</template>

<script>
import { useStore } from "vuex"
export default {
    name: 'mutation',
    setup() {
        const store = useStore()
        function add() {
            store.commit("add", 1)
        }
        function reduce() {
            store.commit("reduce")
        }
        return { add, reduce }
    }
}
</script>
```

b、另一种提交风格

![image-20220109220528508](./images/202203260304138.png)

2）将Mutation中方法封装成常量

注：Mutation中的名称 和 页面中使用的名称一样时，为了方便使用，我们可以把它定义成一个常量。

① 在store——> mutation-type.js

```javascript
/**
 * 床两一般都是大写
 */
export const ADD = "add"
```

② 在store——> index.js

```javascript
import { createStore } from 'vuex'
import { ADD } from "./mutation-type.js"
/**
 * 创建仓库和导出
 */
export default createStore({
    state: {
        counter: 0
    },
    mutations: {
        /**
         * mutations修改数据源
         * @param state 数据原
         * @param payload 外部调用时 传入的参数
         */
        [ADD](state, payload) {
            state.counter += payload.num
        },
        reduce(state) {
            state.counter--
        }
    }
})
```

③ 使用

```vue
<template>
    <p>mutation</p>
    <h1>{{$store.state.counter}}</h1>
    <button @click="add">+1</button>
    <button @click="reduce">-1</button>
</template>

<script>
import { useStore } from "vuex"
import { ADD } from "@/store/mutation-type"
export default {
    name: 'mutation',
    setup() {
        const store = useStore()
        function add() {
            store.commit({
                type: ADD,
                num: 1
            })
        }
        function reduce() {
            store.commit("reduce")
        }
        return { add, reduce }
    }
}
</script>
```

## 10、mapMutations

1）状态管理

```javascript
import { createStore } from 'vuex'
/**
 * 创建仓库和导出
 */
export default createStore({
    state: {
        counter: 0
    },
    mutations: {
        /**
         * mutations修改数据源
         * @param state 数据原
         * @param payload 外部调用时 传入的参数
         */
        add(state, payload) {
            state.counter += payload
        },
        reduce(state) {
            state.counter--
        }
    }
})
```

2）使用

```vue
<template>
    <p>mutation</p>
    <h1>{{$store.state.counter}}</h1>
    <button @click="add(1)">+1</button>
    <button @click="reduce">-1</button>
</template>

<script>
import { mapMutations } from "vuex"
export default {
    name: 'mutation',
    methods: {
        // 这里也可以写成对象
        ...mapMutations(["add", "reduce"])
    }
}
</script>

```

## 11、mapMutations在setup中的使用

1）vuex中的书写

```javascript
import { createStore } from 'vuex'
/**
 * 创建仓库和导出
 */
export default createStore({
    state: {
        counter: 0
    },
    mutations: {
        /**
         * mutations修改数据源
         * @param state 数据原
         * @param payload 外部调用时 传入的参数
         */
        add(state, payload) {
            state.counter += payload
        },
        reduce(state) {
            state.counter--
        }
    }
})
```

2）使用

```vue
<template>
    <p>mutation</p>
    <h1>{{$store.state.counter}}</h1>
    <button @click="add(1)">+1</button>
    <button @click="reduce">-1</button>
</template>

<script>
import { mapMutations } from "vuex"
export default {
    name: 'mutation',
    setup() {
        const storeMutations = mapMutations(["add", "reduce"])
        return { ...storeMutations }
    }
}
</script>
```

## 12、Action

Action 类似于 mutation，不同在于：

- Action 提交的是 mutation，而不是直接变更状态。
- Action 可以包含任意异步操作。

1）状态管理

```javascript
import { createStore } from 'vuex'
/**
 * 创建仓库和导出
 */
export default createStore({
    state: {
        counter: 0
    },
    mutations: {
        add(state, payload) {
            state.counter += payload
        }
    },
    // actions 里面也是放函数的
    actions: {
        // addActions({ commit, dispatch, state, rootGetters, rootState, getters }, payload) { // 这个式结构的写法
        addActions(context, payload) {
            // 查看 context 的属性有哪些
            console.log(context);

            context.commit("add", payload)
        }
    }
})
```

2）使用

```vue
<template>
    <p>action</p>
    <h1>{{$store.state.counter}}</h1>
    <button @click="add">+1</button>
</template>
<script>
export default {
    name: 'action',
    methods: {
        add() {
            // 这里也可以写成对象类型
            this.$store.dispatch("addActions", 1)
        }
    }
}
</script>
```

## 13、在setup中使用mapActions

1）数据源

```javascript
import { createStore } from 'vuex'
/**
 * 创建仓库和导出
 */
export default createStore({
    state: {
        counter: 0
    },
    mutations: {
        add(state, payload) {
            state.counter += payload
        }
    },
    // actions 里面也是放函数的
    actions: {
        // addActions({ commit, dispatch, state, rootGetters, rootState, getters }, payload) { // 这个式结构的写法
        addActions(context, payload) {
            // 查看 context 的属性有哪些
            // console.log(context);
            context.commit("add", payload)
        }
    }
})
```

2）使用

```vue
<template>
    <p>action</p>
    <h1>{{$store.state.counter}}</h1>
    <!-- 在这传入参数 -->
    <button @click="add(1)">+1</button>
</template>

<script>
import { useStore, mapActions } from "vuex"
export default {
    name: 'action',
    setup() {
        const store = useStore()
        // 这里也可以使用数组
        const actionsFn = mapActions({ add: "addActions" })
        return { ...actionsFn }
    }
}
</script>
```

## 14、Module

文档：<https://next.vuex.vuejs.org/zh/guide/modules.html#%E5%B8%A6%E5%91%BD%E5%90%8D%E7%A9%BA%E9%97%B4%E7%9A%84%E7%BB%91%E5%AE%9A%E5%87%BD%E6%95%B0>

由于使用单一状态树，应用的所有状态会集中到一个比较大的对象。当应用变得非常复杂时，store 对象就有可能变得相当臃肿。

为了解决以上问题，Vuex 允许我们将 store 分割成 模块（module）。每个模块拥有自己的 state、mutation、action、getter、甚至是嵌套子模块——从上至下进行同样方式的分割：

1）在src——> store ——> 创建module文件夹（用来存储模块）

注：① 每个模块他都是一个对象；
        ② 每个模块里都要state、getters、action、mutation属性；

2）在module下创建你需要的文件（例：one.js）

```javascript
const one = {
    state() {
        return {
            oneName: "模块一"
        }
    },
    getters: {

    },
    mutations: {

    },
    actions: {

    }
}
export default one
```

3）在src——>index.js中引入

```javascript
const one = {
    /**
     * 命名空间（这样设置就代表了 模块化的使用）
     */
    namespaced: true,
    state() {
        return {
            oneName: "模块一"
        }
    },
    getters: {
        /**
         * 
         * @param {*} state 当前的state
         * @param {*} getters 当前的getters
         * @param {*} rootState 根目录下的state
         * @param {*} rootGetters 根目录下的getters
         */
        fun1(state, getters, rootState, rootGetters) {

        }
    },
    mutations: {
        increment(state) {
            state.oneName = "哈哈哈"
        }
    },
    actions: {
        /**
         * 做了结构
         */
        fun2({ commit, dispatch, state, rootCommit, rootDispatch, rootState }) {
            commit('someMutation', null, { root: true }) // 这个代表修改根目录下的 commit的某一个事件（someMutation），这个事件写在 mutations 里
            dispatch('someOtherAction', null, { root: true }) // -> 个代表修改根目录下的 dispatch的某一个事件（someOtherAction），这个事件写在 actions 里
        }
    }
}
export default one
```

4）使用

```vue
<template>
    <p>模块1</p>

    <p>{{name}}</p>
    <button @click="fun">点</button>
</template>

<script>
import { computed } from '@vue/reactivity'
import { useStore } from "vuex"
export default {
    name: 'module1',
    setup() {
        const store = useStore()
        function fun() {
            // 这块写 / ，是因为在模块里面设置了命名空间
            store.commit("customize/increment")
        }
        const name = computed(() => store.state.customize.oneName)
        return { name, fun }
    }
}
</script>
```

## 15、module的辅助函数

1）vuex的数据源

① 模块中的数据（src——>store——>module——>one.js）

```javascript
const one = {
    namespaced: true,
    state() {
        return {
            oneCounter: 100
        }
    },
    getters: {
        doubleOneCounter(state, getters, rootState, rootGetters) {
            return state.oneCounter * 2
        }
    },
    mutations: {
        increment(state) {
            state.oneCounter++
        }
    },
    actions: {
        // 修改跟目录下的数据
        incrementAction({ commit, dispatch, state, rootState, getters, rootGetters }) {
            commit("increment")
            commit("increment", null, { root: true })
        }
    }
}
export default one

```

② 根目录下的数据（src——>store——>index.js）

```javascript
import { createStore } from "vuex"
import one from "./modules/one.js"
export default createStore({
    state: {
        name: "根模块",
        rootCounter: 0
    },
    /**
     * 这个里面也可以有： mutations、actions、getters
     */
    mutations: {
        increment(state) {
            state.rootCounter++
            console.log(state.rootCounter);

        }
    },
    modules: {
        one
    }
})
```

2）在setup中的使用

```javascript
<template>
    <div>
        <h2>{{ oneCounter }}</h2>
        <h2>{{ doubleOneCounter }}</h2>
        <button @click="increment">one + 1</button>
        <button @click="incrementAction">one + 1</button>
    </div>
</template>
<script>
import { createNamespacedHelpers } from "vuex"
// 这个组件见：8、对setup中使用辅助函数进行封装
import vuexMap from "@/hook/vuexMap.js"
/**
 * 获取当前模块的 mapState, mapGetters, mapMutations, mapActions
 * 可以把createNamespacedHelpers也封装进vuexMap，但是 我感觉这样太麻烦
 */
const { mapState, mapGetters, mapMutations, mapActions } = createNamespacedHelpers("one")
export default {
    setup() {
        const state = vuexMap(mapState, ["oneCounter"])
        const getters = vuexMap(mapGetters, ["doubleOneCounter"])
        const mutations = mapMutations(["increment"])
        const actions = mapActions(["incrementAction"])

        return { ...state, ...getters, ...mutations, ...actions }
    }
}
</script>
```

3）在选项API中的使用

① 方法一

```vue
<template>
    <div>
        <h2>{{ counter }}</h2>
        <h2>{{ doubleCounter }}</h2>
        <button @click="increment">one + 1</button>
        <button @click="incrementAction">one + 1</button>
    </div>
</template>
<script>
import { mapState, mapGetters, mapMutations, mapActions } from "vuex"
export default {
    computed: {
        ...mapState({
            counter: state => state.one.oneCounter
        }),
        ...mapGetters({
            doubleCounter: "one/doubleOneCounter"
        })
    },
    methods: {
        ...mapMutations({
            increment: "one/increment"
        }),
        ...mapActions({
            incrementAction: "one/incrementAction"
        }),
    }
}
</script>
```

② 方法二

```vue
<template>
    <div>
        <h2>{{ oneCounter }}</h2>
        <h2>{{ doubleOneCounter }}</h2>
        <button @click="increment">one + 1</button>
        <button @click="incrementAction">one + 1</button>
    </div>
</template>
<script>
import { mapState, mapGetters, mapMutations, mapActions } from "vuex"

export default {
    computed: {
        /**
         * 第一个参数是映射模块
         * 第二个是展示的数据
         */
        ...mapState("one", ["oneCounter"]),
        ...mapGetters("one", ["doubleOneCounter"])
    },
    methods: {
        ...mapMutations("one", ["increment"]),
        ...mapActions("one", ["incrementAction"]),
    }
}
</script>
```

③ 方法三

```vue
<template>
    <div>
        <h2>{{ oneCounter }}</h2>
        <h2>{{ doubleOneCounter }}</h2>
        <button @click="increment">one + 1</button>
        <button @click="incrementAction">one + 1</button>
    </div>
</template>
<script>
import { createNamespacedHelpers } from "vuex"
/**
 * createNamespacedHelpers 这个是vuex里面的，里面的参数是模块名
 * 告诉他，你要映射的模块名
 */
const { mapState, mapGetters, mapMutations, mapActions } = createNamespacedHelpers("one")
export default {
    computed: {
        ...mapState(["oneCounter"]),
        ...mapGetters(["doubleOneCounter"])
    },
    methods: {
        ...mapMutations(["increment"]),
        ...mapActions(["incrementAction"]),
    }
}
</script>
```

# 四、vuex中数据请求

1、在vuex中（store——>index.js）

```javascript
import { createStore } from 'vuex'
import axios from 'axios'
export default createStore({
    state: {
        obj: {}
    },
    mutations: {
        DATA(state, payload) {
            state.obj = payload
        }
    },
    actions: {
        request(context, payload) {
            return new Promise((resolve, reject) => {
                axios({
                    url: "https://mock.mengxuegu.com/mock/60434bccf340b05bceda3906/practise-nuxtjs/test",
                    method: "GET"
                }).then(res => {
                    // console.log(res.data)
                    resolve(res.data)
                }).catch(err => {
                    reject(err)
                    console.error(err)
                })
            })
        }
    }
})
```

2、使用

```vue
<template>
    <p>数据请求</p>
    <h1>{{data.title}}</h1>
    <p>{{data.content}}</p>
</template>

<script>
import { onMounted, reactive, toRefs } from 'vue'
import { useStore } from "vuex"
export default {
    name: 'request',
    setup() {
        let obj = reactive({
            data: {}
        })
        const store = useStore()
        onMounted(() => {
            store.dispatch("request").then(res => {
                // console.log(res)
                obj.data = res.data
                console.log(obj);
            }).catch(err => {
                console.error(err)
            })
        })

        return { ...toRefs(obj) }
    }
}
</script>
```
