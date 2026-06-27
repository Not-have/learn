# 一、transition的认识

## 1、transition是内置组件

内置组件可以直接在模板中使用，而不需注册。

<keep-alive>、<transition>、<transition-group> 和 <teleport> 组件都可以被打包工具 tree-shake。所以它们只会在被使用的时候被引入。如果你需要直接访问它们，也可以将它们显性导入。

官网文档：<https://v3.cn.vuejs.org/api/built-in-components.html#transition>

## 2、transition原理

1）自动嗅探目标元素是否应用了CSS过渡或者动画，如果有 那么在恰当的时机添加/删除 CSS类名；

2）如果 transition 组件提供了JavaScript钩子函数，这些钩子函数将在恰当的时机被调用；

3）如果没有找到JavaScript钩子并且也没有检测到CSS过渡/动画，DOM插入、删除操作将会立即执行。

![image-20211115223944544](./images/202203280037393.png)

# 二、transition 的name属性

## 1、过渡动画

注：① <font color=red>v 代表name里面定义的类名</font>；

        ② enter代表的都是进入时的状态；
    
        ③ leave是离开。

1）v-enter-from：定义进入过渡的开始状态。在元素被插入DOM之前生效，在元素被插入DOM之后的下一帧移除。

2）v-enter-active：定义进入过渡生效时的状态。在整个进入过渡的阶段中应用，在元素被插入之前生效，在过渡/动 画完成之后移除。这个类可以被用来定义进入过渡的过程时间，延迟和曲线函数。

注：一般在v-enter-active里面写transition属性。

3）v-enter-to：定义进入过渡的结束状态。在元素被插入之后下一帧生效 (与此同时 v-enter-from 被移除)，在过渡/ 动画完成之后移除。

4）v-leave-from：定义离开过渡的开始状态。在离开过渡被触发时立刻生效，下一帧被移除。

5）v-leave-active：定义离开过渡生效时的状态。在整个离开过渡的阶段中应用，在离开过渡被触发时立刻生效，在 过渡/动画完成之后移除。这个类可以被用来定义离开过渡的过程时间，延迟和曲线函数。

6）v-leave-active：定义离开过渡生效时的状态。在整个离开过渡的阶段中应用，在离开过渡被触发时立刻生效，在 过渡/动画完成之后移除。这个类可以被用来定义离开过渡的过程时间，延迟和曲线函数。

<img src="https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/08aac77ffdc1408494966948dee802b6~tplv-k3u1fbpfcp-zoom-1.image" />

vue动画[过渡文档](https://v3.cn.vuejs.org/guide/transitions-enterleave.html#%E5%8D%95%E5%85%83%E7%B4%A0-%E7%BB%84%E4%BB%B6%E7%9A%84%E8%BF%87%E6%B8%A1)：<https://v3.cn.vuejs.org/guide/transitions-overview.html>

## 2、CSS 过渡

```javascript
<template>
    <div class="content">
        <button @click="isShow = !isShow">显示 / 隐藏</button>
        <hr />
        <transition name="animation">
            <h2 v-if="isShow" class="box">
                transition 的使用一
            </h2>
        </transition>
        <br>
    </div>
</template>
<script>

export default {
    name: 'One',
    components: {

    },
    data() {
        return {
            isShow: true
        }
    },
}
</script>

<style scoped>
.content{
    text-align: center;
}
.box{
    display: inline-block;
}
.animation-enter-active {
    animation: identifier 2s ease;
}
.animation-leave-active {
    animation: identifier 2s ease reverse;
}
@keyframes identifier {
    0% {
        transform: scale(0);
    }
    50% {
        transform: scale(1.5);
    }
    100% {
        transform: scale(1);
    }
}
</style>
```

## 3、同时使用过渡和动画

Vue 为了知道过渡的完成，必须设置相应的事件监听器。它可以是 transitionend  或 animationend，这取决于给元素应用的 CSS 规则。如果你使用其中任何一种，Vue 能自动识别类型并设置监听。但是，在一些场景中，你需要给同一个元素同时设置两种过渡动效，比如 animation 很快的被触发并完成了，而 transition 效果还没结束。在这种情况中，你就需要使用 type attribute 并设置 animation 或 transition 来明确声明你需要 Vue 监听的类型。

实现一个放大加渐变的效果：

```javascript
<template>
    <div class="content">
        <button @click="isShow = !isShow">显示 / 隐藏</button>
        <hr />
        <transition name="animation">
            <h2 v-if="isShow" class="box">
                transition 的使用一
            </h2>
        </transition>
    </div>
</template>
<script>

export default {
    data() {
        return {
            isShow: true
        }
    },
}
</script>
<style scoped>
.content {
    text-align: center;
}
.box {
    display: inline-block;
}
.animation-enter-from,
.animation-leave-to {
    opacity: 0;
}
.animation-enter-active,
.animation-leave-active {
    transition: opacity 2s ease;
}

.animation-enter-active {
    animation: identifier 2s ease;
}
.animation-leave-active {
    animation: identifier 2s ease reverse;
}
@keyframes identifier {
    0% {
        transform: scale(0);
    }
    50% {
        transform: scale(1.5);
    }
    100% {
        transform: scale(1);
    }
}
</style>
```

![image-20211116000137504](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/56cebdc70f2c416f90fcd44d7a36eb14~tplv-k3u1fbpfcp-zoom-1.image)

## 4、显示的指定动画时间

![无标题](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/e32a2edfec934ea89b6d1012be180a7d~tplv-k3u1fbpfcp-zoom-1.image)

## 5、过渡的模式mode

<font color=red>注：实现两个元素之间来回切换的时候，使用过渡的模式。</font>

1）in-out：新元素先进行过渡，完成之后当前元素过渡离开

 ![chrome-capture](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/35b473bc54d34ee8bed8919473726cdd~tplv-k3u1fbpfcp-zoom-1.image)

2）out-in：当前元素先进行过渡，完成之后新元素过渡进入

 ![chrome-capture](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/829b2900bb5947c2b00d7243f3f9c24c~tplv-k3u1fbpfcp-zoom-1.image)

3）案例

```javascript
<template>
    <div class="content">
        <button @click="isShow = !isShow">显示 / 隐藏</button>
        <hr />
        <!-- 修改模式 -->
        <transition name="animation" mode="out-in">
            <h2 v-if="isShow" class="box">条件一</h2>
            <h2 v-else class="box">条件二</h2>
        </transition>
    </div>
</template>
<script>

export default {
    name: 'One',
    components: {

    },
    data() {
        return {
            isShow: true
        }
    },
}
</script>
<style scoped>
.content {
    text-align: center;
}
.box {
    display: inline-block;
}
.animation-enter-from,
.animation-leave-to {
    opacity: 0;
}
/* 时间先设置成一样的 */
.animation-enter-active,
.animation-leave-active {
    transition: opacity 2s ease;
}

.animation-enter-active {
    animation: identifier 2s ease;
}
.animation-leave-active {
    animation: identifier 2s ease reverse;
}
@keyframes identifier {
    0% {
        transform: scale(0);
    }
    50% {
        transform: scale(1.5);
    }
    100% {
        transform: scale(1);
    }
}
</style>
```

## 6、appear初次渲染

 ![image-20211121154751618](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/7ac8e7f751e24dad9d8db93e331664e7~tplv-k3u1fbpfcp-zoom-1.image)

# 三、自定义过渡class

enter-from-class

enter-active-class

enter-to-class

leave-from-class

leave-active-class

leave-to-class  

他们的优先级高于普通的类名，这对于 Vue 的过渡系统和其他第三方 CSS 动画库，如 Animate.css. 结合使用十 分有用。

<https://v3.cn.vuejs.org/guide/transitions-enterleave.html#%E8%87%AA%E5%AE%9A%E4%B9%89%E8%BF%87%E6%B8%A1-class-%E7%B1%BB%E5%90%8D>

# 四、JavaScript钩子

<https://v3.cn.vuejs.org/guide/transitions-enterleave.html#javascript-%E9%92%A9%E5%AD%90>

```vue
<template>
    <p>gsap的使用</p>
    <button @click="isShow = !isShow">显示 / 隐藏</button>
    <hr />
    <transition appear 
        @before-enter="beforeEnter" 
  @enter="enter" 
  @after-enter="afterEnter" 
  @enter-cancelled="enterCancelled" 
  @before-leave="beforeLeave" 
  @leave="leave" 
  @after-leave="afterLeave"
        @leave-cancelled="leaveCancelled">
        <!-- 
            里面添加删除元素的时候，才会添加当动画
            这个里面可以写动态组件 和 路由
         -->
        <h2 v-if="isShow" class="box">
            gsap的使用
        </h2>
    </transition>
</template>

<script>

export default {
    name: 'gsap',
    data() {
        return {
            isShow: true
        }
    },
    methods: {
        /**
         * 初始化的操作
         * 相当与from
         */
        beforeEnter(el) {
            console.log("beforeEnter");
        },
        /**
         * 相当于active
         * 在这个里面执行一些具体的动画
         * 
         */
        enter(el, done) {
            console.log("enter")
        },
        /**
         * 结束
         * 在这做一些收尾的动画效果
         */
        afterEnter(el) {
            console.log("afterEnter")
        },
        enterCancelled(el) {
            console.log("enterCancelled")
        },
        /**
         * 离开时 ，做一些初始化
         */ 
        beforeLeave(el) {
            console.log("beforeLeave")
        },
        /**
         * 具体离开时 执行的动画
         */
        leave(el, done) {
            console.log("leave")
        },
        /**
         * 离开之后
         */
        afterLeave(el) {
            console.log("afterLeave")
        },
        leaveCancelled(el) {
            console.log("leaveCancelled")
        }
    },
}
</script>
```

这些钩子函数可以结合 CSS transitions/animations 使用，也可以单独使用。

当只用 JavaScript 过渡的时候，在 enter和 leave 钩中必须使用 done 进行回调。否则，它们将被同步调用，过渡会立即完成

添加 :css="false"，也会让 Vue 会跳过 CSS 的检测，除了性能略高之外，这可以避免过渡过程中 CSS 规则的影响。

# 五、animate.css的使用

地址：<https://animate.style>

Animate.css是一个随时可用的跨浏览器动画库，可用于您的 Web 项目。非常适合强调、主页、滑块和注意力引导提示。

## 1、安装

```bash
npm install animate.css --save
# or
yarn add animate.css
```

## 2、导入

注：在main.js中导入。

```jade
import 'animate.css';
```

## 3、使用

### 1）通过name属性的使用

```javascript
<template>
    <button @click="isShow = !isShow">显示 / 隐藏</button>
    <hr />
    <!-- 
            设置指定时间(他俩会覆盖下面的时间)
            数字的形式
         -->
    <transition name="box" appear>
        <!-- 
            里面添加删除元素的时候，才会添加当动画
            这个里面可以写动态组件 和 路由
         -->
        <h2 v-if="isShow" class="box">
            transition 的使用一
        </h2>
    </transition>
</template>

<script>
export default {
    name: 'animate-css',
    data() {
        return {
            isShow: true
        }
    },
}
</script>
<style scoped>
.box-enter-active{
    animation: bounceInRight .5s ease-in;
}
.box-leave-active{
    animation: bounceInRight .5s ease-in reverse;
}
</style>
```

![image-20211121161602159](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/f69f0cfddc9a4c53ae4658fa41c85b6b~tplv-k3u1fbpfcp-zoom-1.image)

### 2）自定义类名的使用

![image-20211121165219185](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/05ad94eef5f44bb5bf8dd963b29e3759~tplv-k3u1fbpfcp-zoom-1.image)

```javascript
<template>
    <button @click="isShow = !isShow">显示 / 隐藏</button>
    <hr />
    <!-- 
            设置指定时间(他俩会覆盖下面的时间)
            数字的形式
         -->
    <transition  appear enter-active-class="animate__animated animate__bounceInRight" leave-active-class="animate__animated animate__backOutUp">
        <!-- 
            里面添加删除元素的时候，才会添加当动画
            这个里面可以写动态组件 和 路由
         -->
        <h2 v-if="isShow" class="box">
            transition 的使用一
        </h2>
    </transition>
</template>

<script>
export default {
    name: 'animate-css',
    data() {
        return {
            isShow: true
        }
    },
}
</script>
```

# 六、gsap

地址：<https://greensock.com/>

通过JavaScript为CSS属性、SVG、Canvas等设置动画，并且是浏览器兼容的。

## 1、安装

```bash
npm install gsap
```

## 2、使用

```javascript
<template>
    <p>gsap的使用</p>
    <button @click="isShow = !isShow">显示 / 隐藏</button>
    <hr />
    <!-- 
        :css="false" 忽略检测css的动画
     -->
    <transition appear @enter="enter" @leave="leave" :css="false">
        <!-- 
            里面添加删除元素的时候，才会添加当动画
            这个里面可以写动态组件 和 路由
         -->
        <h2 v-if="isShow" class="box">
            gsap的使用
        </h2>
    </transition>
</template>

<script>
/* 引入 */
import gsap from "gsap";
export default {
    name: 'gsap',
    data() {
        return {
            isShow: true
        }
    },
    methods: {
        /**
         * 相当于active
         * 在这个里面执行一些具体的动画
         * el 当前元素
         */
        enter(el, done) {
            console.log("enter")
            gsap.from(el, {
                scale: 0,
                x: 200,
                // 当动画完成的时候，去执行 done 这个回调
                onComplete: done
            })
        },
        /**
         * 具体离开时 执行的动画
         */
        leave(el, done) {
            console.log("leave")
            gsap.to(el, {
                scale: 0,
                x: 200,
                // 当动画完成的时候，去执行 done 这个回调
                onComplete: done
            })
        }
    },
}
</script>
```

## 3、实现一个递增的计算器

```javascript
<template>
    <p>gsap实现数字变化</p>
    <input type="number" v-model="num" step="100">
    <h2>{{ showNum }}</h2>
    <h2>{{ show.toFixed(0) }}</h2>
</template>

<script>
import gsap from "gsap";
export default {
    name: 'gsap',
    data() {
        return {
            num: 0,
            show: 0
        }
    },
    computed: {
        showNum() {
            return this.show.toFixed(0)
        }
    },
    watch: {
        num(newValue) {
            gsap.to(this, { duration: 1, show: newValue })

        }
    }
}
</script>
```

 ![chrome-capture](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/efc85ad41b41494499145122446f88ab~tplv-k3u1fbpfcp-zoom-1.image)

# 七、列表过渡

文档：<https://v3.cn.vuejs.org/guide/transitions-list.html>

## 1、移动动画

 ![chrome-capture](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/4c2948269d2d4969990e9de5117d2179~tplv-k3u1fbpfcp-zoom-1.image)

```javascript
<template>
    <button @click="addNum">添加</button>
    <button @click="removeNum">删除</button>
    <button @click="shuffleNum">数字洗牌</button>

    <transition-group tag="p" name="box">
        <span v-for="(index,item) in numbers" :key="index">
            {{item}}
        </span>
    </transition-group>
</template>

<script>
/**
 * npm i lodash
 */
import _ from 'lodash';
export default {
    data() {
        return {
            numbers: [0, 1, 2, 3, 4, 5, 6, 7, 8, 9],
            numCounter: 10
        }
    },
    methods: {
        addNum() {
            // this.numbers.push(this.numCounter++)
            this.numbers.splice(this.randomIndex(), 0, this.numCounter++)
        },
        removeNum() {
            this.numbers.splice(this.randomIndex(), 1)
        },
        shuffleNum() {
            this.numbers = _.shuffle(this.numbers);
        },
        randomIndex() {
            return Math.floor(Math.random() * this.numbers.length)
        }
    },
}
</script>

<style scoped>
span {
    margin-right: 10px;
    display: inline-block;
}

.box-enter-from,
.box-leave-to {
    opacity: 0;
    transform: translateY(30px);
}
/* 一出时的动画 */
.box-enter-active,
.box-leave-active {
    transition: all 1s ease;
}
/* 对移除元素一个绝对定位，这个时候 他会脱离文档流，确保下个元素 正常进入  */
.box-leave-active {
    position: absolute;
}
/* 位移的动画 */
.box-move {
    transition: transform 1s ease;
}
</style>
```

## 2、搜索框列表展示的动画

### 1）css实现

 ![chrome-capture](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/176edf382c65455b8310af5899a59a1a~tplv-k3u1fbpfcp-zoom-1.image)

```javascript
<template>
    <div>
        <input type="text" v-model="text">
        <transition-group name="box">
            <p v-for="(item, index) in showText" :key="index">{{ item }}</p>
        </transition-group>
    </div>
</template>
<script>
export default {
    components: {

    },
    data() {
        return {
            arr: ["哈哈1", "呃呃1", "呵呵2", "拉拉2", "嘿嘿3", "啊啊3", "呵呵4"],
            text: ""
        }
    },
    computed: {
        showText() {
            /**
             * item 为true保留当前元素
             *      为false不保留当前元素
             */
            return this.arr.filter(item => item.indexOf(this.text) !== -1)
        }
    }
}
</script>
<style scoped>
div{
    padding: 20px;
}
input {
    border: 1px solid gray;
}

.box-enter-from,
.box-leave-to {
    opacity: 0;
    transform: translateY(30px);
}
/* 一出时的动画 */
.box-enter-active,
.box-leave-active {
    transition: all 1s ease;
}
</style>
```

### 2）js的实现

 ![chrome-capture](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/581a2168324142f7aef8d253809a8f5b~tplv-k3u1fbpfcp-zoom-1.image)

```javascript
<template>
    <div>
        <input type="text" v-model="text">
        <transition-group name="box" :css="false" @before-enter="beforeEnter" @enter="enter" @leave="leave">
            <!-- 
                给p元素对象，绑定一个属性，属性名字叫 data-index,这个属性，可以用事件源 获取到 
                获取的时候 el.dataset.index
            -->
            <p v-for="(item, index) in showText" :key="index" :data-index="index">{{ item }}</p>
        </transition-group>
    </div>
</template>
<script>
import gsap from "gsap"
export default {
    data() {
        return {
            arr: ["哈哈1", "呃呃1", "呵呵2", "拉拉2", "嘿嘿3", "啊啊3", "呵呵4"],
            text: ""
        }
    },
    computed: {
        showText() {
            /**
             * item 为true保留当前元素
             *      为false不保留当前元素
             */
            return this.arr.filter(item => item.indexOf(this.text) !== -1)
        }
    },
    methods: {
        beforeEnter(el) {
            el.style.opacity = 0;
            el.style.height = 0;

        },
        enter(el, done) {
            gsap.to(el, {
                opacity: 1,
                height: "20px",
                /**
                 * 这是一个延迟
                 * 不设置这个，他就会一起消失
                 * 
                 */
                delay: el.dataset.index * 0.1,
                onComplete: done
            })
        },
        leave(el, done) {
            gsap.to(el, {
                opacity: 0,
                height: "0px",
                delay: el.dataset.index * 0.1,
                onComplete: done
            })
        }
    },
}
</script>

<style scoped>
div {
    padding: 20px;
}
input {
    border: 1px solid gray;
}

.box-enter-from,
.box-leave-to {
    opacity: 0;
    transform: translateY(30px);
}
.box-enter-active,
.box-leave-active {
    transition: all 1s ease;
}
</style>
```
