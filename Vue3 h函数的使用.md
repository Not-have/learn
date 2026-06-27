# 一、认识

文档：<https://v3.cn.vuejs.org/guide/render-function.html#dom-%E6%A0%91>

​  h() 到底会返回什么呢？其实不是一个*实际*的 DOM 元素。它更准确的名字可能是 createNodeDescription，因为它所包含的信息会告诉 Vue 页面上需要渲染什么样的节点，包括及其子节点的描述信息。我们把这样的节点描述为“虚拟节点 (virtual node)”，也常简写它为  VNode 。“虚拟 DOM”是我们对由 Vue 组件树建立起来的整个 VNode 树的称呼。

# 二、使用

文档：<https://v3.cn.vuejs.org/guide/render-function.html#h-%E5%8F%82%E6%95%B0>

## 1、h() 参数

h() 函数是一个用于创建 VNode 的实用程序。也许可以更准确地将其命名为 createVNode()，但由于频繁使用和简洁，它被称为 h() 。它接受三个参数：

```javascript
// @returns {VNode}
h(
    // {String | Object | Function} tag
    // 一个 HTML 标签名、一个组件、一个异步组件、或
    // 一个函数式组件。
    //
    // 必需的。
    'div',

    // {Object} props
    // 与 attribute、prop 和事件相对应的对象。
    // 这会在模板中用到。
    //
    // 可选的(在开发时。建议传，实在没有传的时候，传入 null)
    {},

    // {String | Array | Object} children
    // 子 VNodes, 使用 `h()` 构建,
    // 或使用字符串获取 "文本 VNode" 或者
    // 有插槽的对象。
    //
    // 可选的。
    [
        'Some text comes first.',
        h('h1', 'A headline'),
        h(MyComponent, {
            someProp: 'foobar'
        })
    ]
)
```

## 2、简单的使用

 ![image-20211225202723505](./images/202203280035618.png)

## 3、实现一个计数器案例

```javascript
<script>
/* 设置样式需要在这引入，如果使用style标签，则不能写scoped，不利于设置局部样式，所以不建议 */
import "./style.css"
import { h, ref } from "vue";

export default {
    // data 的写法
    // data() {
    //     return {
    //         counter: 0
    //     }
    // },
    setup() {
        const counter = ref(0)
        return { counter }
    },
    /**
     * 使用setup的时候，下面也可以用this，引入render有绑定this，所以下面取值，要用this
     */
    render() {
        return h("div", null, [
            h("h1", null, `当前计数：${this.counter}`),
            h("button", { onClick: () => this.counter++, class: "button" }, "加 1"),
            h("button", { onClick: () => this.counter--, class: "button" }, "减 1")
        ])
    }
}
</script>
```

修改成纯setup的写法：

```javascript
<script>
/* 设置样式需要在这引入，如果使用style标签，则不能写scoped，不利于设置局部样式，所以不建议 */
import "./style.css"
import { h, ref } from "vue";

export default {
    // data 的写法
    // data() {
    //     return {
    //         counter: 0
    //     }
    // },
    setup() {
        const counter = ref(0)
        return () => {
            return h("div", null, [
                h("h1", null, `当前计数：${counter.value}`),
                h("button", { onClick: () => counter.value++, class: "button" }, "加 1"),
                h("button", { onClick: () => counter.value--, class: "button" }, "减 1")
            ])
        }
    }
}
</script>
```

## 4、函数组件和插槽的使用

1）父组件

```javascript
<script>
import { h, ref } from "vue";
import Test from "./components/Test.vue"
export default {
    setup() {
        return {}
    },
    render() {
        return h(Test, null, {
            // default 对应的是一个函数，default是默认插槽
            default: props => h("span", null, "父传入到组件中的内容：" + props.name)
        })
    }
}
</script>
```

2）子组件

```javascript
<script>
import { h } from "vue";

export default {
    /**
     * 接收夫传入的内容
     */
    render() {
        return h("div", null, [
            h("div", null, "我是子组件"),
            /**
             * 在这判断别人是否传入
             * 也可以写 null，啥也不展示
             * 也可以在传入一个参数，使用的是 函数传参
             */
            this.$slots.default ? this.$slots.default({ name: "哈哈哈" }) : h("div", null, "我是子组件的默认值")
        ])
    }
}
</script>
```

<font color=red>注：在项目中，如果你像上面一样写代码，就太苦逼了，所以这个时候就要用 JSX。</font>

# 三、jsx的使用

## 1、jsx的认识

jsx我们通常会通过Babel来进行转换（React编写的jsx就是通过babel转换的）；

对于Vue来说，我们只需要在Babel中配置对应的插件即可；

文档：<https://v3.cn.vuejs.org/guide/render-function.html#jsx>

## 2、下载Babel插件支持vue（现在貌似脚手架直接支持）

```bash
npm install @vue/babel-plugin-jsx -D
```

## 3、配置babel

1）在根目录下创建 .babel.config.js

2）在.babel.config.js 里面加入，如下代码

```javascript
module.exports = {
    presets: [
        "@/vue/cli-plugin-babel/preset"
    ],
    plugins: [
        "@vue/babel-plugin-jsx"
    ]
}
```

## 4、简单的使用

```javascript
<script>
import { ref } from 'vue'
export default {
    setup() {
        let counter = ref(0)
        return { counter }
    },
    render() {
        return (
            <div>
                <div>JSX的使用</div>
                <h2>当前数字：{this.counter}</h2>
            </div>


        )
    }
}
</script>
```

## 5、计数器案例

```javascript
<script>
import { ref } from '@vue/reactivity'

export default {
    setup() {
        let counter = ref(0)
        function add() {
            counter.value++
        }
        function decrement() {
            counter.value--
        }
        return { counter, add, decrement }
    },
    render() {
        return (
            <div>
                <div>JSX的使用</div>
                <h2>当前数字：{this.counter}</h2>
                <button onClick={this.add}>加 1</button>
                <button onClick={this.decrement} >减 1</button>
            </div >
        )
    }
}
</script>
```

## 6、组件和插槽的使用

1）父组件

```javascript
<script>
import { ref } from '@vue/reactivity'
import Test from "./components/Test.vue"
export default {
    setup() {
        let counter = ref(0)
        function add() {
            counter.value++
        }
        function decrement() {
            counter.value--
        }
        return { counter, add, decrement }
    },
    render() {
        return (
            <div>
                <div>JSX的使用</div>
                {/* 如果这块使用setup里面的变量、方法 要加this */}
                <h2>当前数字：{this.counter}</h2>
                <button onClick={this.add}>加 1</button>
                <button onClick={this.decrement} >减 1</button>
                <hr />
                <Test>
                    {/* 这个里面写入传入的内容,也就是传入一个插槽 */}
                    {{ default: props => <p>我是父传入子的</p> }}
                </Test>
            </div >
        )
    }
}
</script>
```

2）子组件

```javascript
<script>
export default {
    /**
     * 接收夫传入的内容
     */
    render() {
        return (
            <div>
                <p>我是组件</p>
                {/* 这块也有可能没穿，你要显示一个默认值，这个时候，你就要用三元运算符 */}
                {this.$slots.default()}
            </div>
        )
    }
}
</script>
```

<i> <font color=red>注：如果你要 h 函数来写组件，请仔细查看文档，以上讲解，只是入门级。</font></i>
