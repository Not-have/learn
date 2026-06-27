# 一、认识vue-router

文档：<https://next.router.vuejs.org/zh/>

Vue Router 是 [Vue.js](http://v3.vuejs.org/) 的官方路由。它与 Vue.js 核心深度集成，让用 Vue.js 构建单页应用变得轻而易举。

用 Vue + Vue Router 创建单页应用非常简单：通过 Vue.js，我们已经用组件组成了我们的应用。当加入 Vue Router 时，我们需要做的就是将我们的组件映射到路由上，让 Vue Router 知道在哪里渲染它们。

# 二、使用

## 1、下载

```javascript
npm install vue-router@4
```

## 2、创建路由文件

注：在src——>创建router文件夹——>index.js文件

```javascript
import { createRouter, createWebHistory, createWebHashHistory } from 'vue-router';
// 1、引入组件
import Home from "../views/Home.vue"
import Router4 from "../views/Router4的使用/Index.vue"
/**
 * 2、配置路由映射关系
 */
const routes = [
    {
        path: '/home',
        component: Home
    },
    {
        path: "/router",
        component: Router4
    }
]
// 3、创建一个路由的对象
const router = createRouter({
    // 指定模式
    /**
     * createWebHashHistory 带 # 号
     * createWebHistory 不带 # 号
     */
    history: createWebHashHistory(),
    // 下面这个 可以写成ES6的简写 routers
    routes:routes
})

export default router
```

## 3、路由懒加载

```javascript
    {
        path: '/路由地址',
        name: '路由记录独一无二的名称',
        component: () => import("文件路径/文件名.vue")
    },
```

 ![image-20220102214822798](./images/202203260245621.png)

## 4、引入main.js

```javascript
import { createApp } from 'vue'
import App from './App.vue'
import router from './router/index'

const app = createApp(App);

app.use(router);
app.mount('#app');
```

## 5、设置路由出口文件(一般在App.vue里)

```javascript
<template>
 <router-view />
</template>
```

## 6、路由属性

name属性：路由记录独一无二的名称

meta属性：自定义的数据

## 7、在setup中获取路由

1）useRoute返回当前理由里面显示的对象

2）useRouter 返回的是全局的路由对象

# 三、router-link页面跳转

## 1、router-link跳转的写法

```javascript
<template>
    <div id="app">
        <router-link to="/home">Home</router-link>
        <router-link to="/router">Router</router-link>
        <!-- 出口 -->
        <router-view />
    </div>
</template>
```

## 2、router-link属性

1）to属性：

是一个字符串，或者是一个对象  

2）replace属性：

设置 replace 属性的话，当点击时，会调用 router.replace()，而不是 router.push()， router.push()有返回，另一个没

3）active-class属性：

设置激活a元素后应用的class，默认是router-link-active

4）exact-active-class属性：

链接精准激活时，应用于渲染的  的 class，默认是router-link-exact-active

## 3、设置默认路由

```javascript
import { createRouter, createWebHistory, createWebHashHistory } from 'vue-router';
// 1、引入组件
import Home from "../views/Home.vue"
import Router4 from "../views/Router4的使用/Index.vue"
/**
 * 2、配置路由映射关系
 */
const routes = [
 {
        path: '/',
        component: Home
    },
    {
        path: '/home',
        component: Home
    },
    {
        path: "/router",
        component: Router4
    }
]
// 3、创建一个路由的对象
const router = createRouter({
    // 指定模式
    /**
     * createWebHashHistory 带 # 号
     * createWebHistory 不带 # 号
     */
    history: createWebHashHistory(),
    // 下面这个 可以写成ES6的简写 routers
    routes:routes
})

export default router
```

## 4、重定向

```javascript
import { createRouter, createWebHistory, createWebHashHistory } from 'vue-router';
// 1、引入组件
import Home from "../views/Home.vue"
import Router4 from "../views/Router4的使用/Index.vue"
/**
 * 2、配置路由映射关系
 */
const routes = [
 {
        path: '/',
        redirect:"/home"
    },
    {
        path: '/home',
        component: Home
    },
    {
        path: "/router",
        component: Router4
    }
]
// 3、创建一个路由的对象
const router = createRouter({
    // 指定模式
    /**
     * createWebHashHistory 带 # 号
     * createWebHistory 不带 # 号
     */
    history: createWebHashHistory(),
    // 下面这个 可以写成ES6的简写 routers
    routes:routes
})

export default router
```

# 四、带参数的动态路由匹配

## 1、路由里面的写法

 ![image-20220103213629261](./images/202203260246286.png)

## 2、路由跳转处的写法

 ![image-20220103213808147](./images/202203260246256.png)

## 3、在组件里面获取

 ![image-20220103214755757](./images/202203260251806.png)

## 4、一次获取多个值

![image-20220103215302553](./images/202203260251516.png)

# 五、实现一个404找不到路径

注：提供一个404图片

 <img src="https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/6c55371c5a4e44158a3ddbf60cedeeb1~tplv-k3u1fbpfcp-watermark.image?" style="zoom: 33%;" />

## 1、匹配找不到的路径

```javascript
    {
        // 这个使用正则，在找不到的时候，会走 *
        path: "/:pathMatch(.*)",
        name: "404",
        component: () => import("../views/404.vue")
    }
    /*
    // 第二种写法
    {
        // 这个使用正则，在找不到的时候，会走 *
        path: "/:catchAll(.*)",
        name: "404",
        component: () => import("../views/404.vue")
    }
    */
```

## 2、获取URL中任意匹配的内容

```javascript
<template>
    <div class="not-found">
        <img src="@/assets/404.gif" alt="">
        <!-- 获取输入的内容 -->    
        <p>{{$route.params.pathMatch}}</p>
    </div>
</template>
<style scoped>
.not-found {
    width: 100%;
    height: 100%;
    overflow: hidden;
}
.not-found img {
    width: 100%;
    height: 100%;
}
</style>
```

## 4、加星和不加的区别

![image-20220103221034303](./images/202203260252157.png)

# 六、二级路由

![image-20220103222818068](./images/202203260252893.png)

# 七、JS方式路由跳转

## 1、直接跳转

```javascript
<template>
    <div>
        <p>router的使用</p> 
        <router-link to="/router/one">一</router-link>
        <router-link to="/router/two">二</router-link>
        <button @click="fun">跳转</button>
        <!-- 展示二级路由 -->
        <router-view></router-view>
    </div>
</template>
<script>
import { useRouter } from "vue-router"
export default {
    setup(){
        const router = useRouter();
        function fun(){
            // 这个是路径
            router.push("/router/two")
        }
        return { fun }
    }
}
</script>
```

## 2、对象的写法

```javascript
<template>
    <div>
        <p>router的使用</p> 
        <router-link to="/router/one">一</router-link>
        <router-link to="/router/two">二</router-link>
        <button @click="fun">跳转</button>
        <!-- 展示二级路由 -->
        <router-view></router-view>
    </div>
</template>
<script>
import { useRouter } from "vue-router"
export default {
    setup(){
        const router = useRouter();
        function fun(){
            router.push({
                // 这个是路径
                path:"/router/two"
            })
        }
        return { fun }
    }
}
</script>
<style scoped>
/* 默认 router-link 选中时的class名 */
.router-link-active{
    color: red;
    margin: 0 20px;
}
</style>
```

## 3、query传参

1）传参界面

```javascript
<template>
    <div>
        <p>router的使用</p> 
        <router-link to="/router/one">一</router-link>
        <router-link to="/router/two">二</router-link>
        <button @click="fun">跳转</button>
        <!-- 展示二级路由 -->
        <router-view></router-view>
    </div>
</template>
<script>
import { useRouter } from "vue-router"
export default {
    setup(){
        const router = useRouter();
        function fun(){
            router.push({
                path:"/router/two",
                query:{
                    age:22
                }
            })
        }
        return { fun }
    }
}
</script>
<style scoped>
/* 默认 router-link 选中时的class名 */
.router-link-active{
    color: red;
    margin: 0 20px;
}
</style>
```

2）接收界面

```javascript
<template>
    <P>router 的二级路由2 </P>
</template>

<script>
import { useRoute } from "vue-router"
export default {
    name: 'Two',
    components:{
        
    },
    setup() {
        const route = useRoute();
        console.log(route.query.age);
    }
}
</script>
```

# 八、router-view的v-slot

<https://next.router.vuejs.org/zh/api/#router-link-%E7%9A%84-v-slot>

# 九、router-view过渡动画的实现

```javascript
<template>
    <div>
     <!-- 下面使用了对象的结构 -->
        <router-view v-slot="{ Component }">
            <transition name="box">
                <!-- keep-alive 缓存 -->
                <keep-alive>
                    <component :is="Component"></component>
                </keep-alive>
            </transition>
        </router-view>
    </div>
</template>
<style scoped>
a {
    margin: 0 20px;
}
/* 默认 router-link 选中时的class名 */
.router-link-active {
    color: red;
    font-size: 16px;
}
.box-enter-from,
.box-leave-to {
    opacity: 0;
}
.box-enter-active,
.box-leave-active {
    transition: opacity 1s ease-in;
}
</style>
```

# 十、动态添加路由

## 1、给一级路由添加

```javascript
import { createRouter, createWebHistory, createWebHashHistory } from 'vue-router';

const routes = [
    {
        path: "/",
        redirect: "/home"
    },
    {
        path: '/home',
        name: 'Home',
        component: () => import("../views/Home.vue")
    },
    {
        // 这个使用正则，在找不到的时候，会走 *
        path: "/:pathMatch(.*)",
        name: "404",
        component: () => import("../views/404.vue")
    }
]

const router = createRouter({
    history: createWebHistory(process.env.BASE_URL),
    routes
})

let addList = {
    path: "/login",
    name: "login",
    component: () => import("../views/login/index.vue")
}
// 添加了一个路由
router.addRoute(addList)

export default router
```

## 2、给二级路由添加

```javascript
/*
给二级路由添加
*/
router.addRoute("一级路由的名称(也就是 name属性)", {这里放二级路由对象})
```

```javascript
import { createRouter, createWebHistory, createWebHashHistory } from 'vue-router';

const routes = [
    {
        path: "/",
        redirect: "/home"
    },
    {
        path: '/home',
        name: 'Home',
        component: () => import("../views/Home.vue")
    },
    {
        path: "/router",
        name: "router",
        // 理由引入
        component: () => import("../views/Router4的使用/Index.vue"),
        children: [
            // 设置二级路由的默认展示界面
            {
                path: "",
                //redirect 是重定向
                redirect: "/router/one"
            },
            {
                // 二级路由直接写路径，不用写父级路径 和 /
                path: "one",
                component: () => import("../views/Router4的使用/son/One.vue")
            }
        ]
    },
    {
        // 这个使用正则，在找不到的时候，会走 *
        path: "/:pathMatch(.*)",
        name: "404",
        component: () => import("../views/404.vue")
    }
]

const router = createRouter({
    history: createWebHistory(process.env.BASE_URL),
    routes
})

let addList = {
    path: "/login",
    name: "login",
    component: () => import("../views/login/index.vue")
}
// 添加了一个路由
router.addRoute(addList)
// 添加一个二级路由
router.addRoute("router", {
    path: "two",
    component: () => import("../views/Router4的使用/son/Two.vue")
})

export default router
```

# 十一、动态删除路由

## 1、添加一个相同的路由

 ![image-20220106231310600](./images/202203260253782.png)

## 2、通过removeRoute方法，传入路由的名称

```javascript
router.removeRoute("对应要删除的 name 名称")
```

## 3、通过addRoute方法的返回值回调

```javascript
import { createRouter, createWebHistory, createWebHashHistory } from 'vue-router';

const routes = [
    {
        // 这个使用正则，在找不到的时候，会走 *
        path: "/:pathMatch(.*)",
        name: "404",
        component: () => import("../views/404.vue")
    }
]

const router = createRouter({
    history: createWebHistory(process.env.BASE_URL),
    routes
})
let addList = {
 path: '/home',
 name: 'Home',
 component: () => import("../views/Home.vue")
}
const removeRoute = router.addRoute(addList)
removeRoute()

export default router
```

## 4、检测路由是否存在

1）router.hasRoute()：检查路由是否存在

2）router.getRoutes()：获取一个包含所有路由记录的数组（获取当前所有路由）

# 十二、路由导航守卫

<font color=red>注：用来路由跳转 和 取消路由跳转。</font>

## 1、参数说明

```javascript
/**
 * 导航守卫
 * to：即将进入的路由Route对象
 * from：即将离开的路由Route对象
 */
router.beforeEach((to, from) => {
    console.log(to)
    console.log(from)
})
```

<font color=red>注：vue-router4 中，已经不建议使用 next。</font>

## 2、返回值说明

1）return false（取消当前导航）

```javascript
/**
 * 导航守卫
 * to：即将进入的路由Route对象
 * from：即将离开的路由Route对象
 */
router.beforeEach((to, from) => {
    console.log(to)
    console.log(from)
    /**
     * 1、取消当前跳转
     */
    return false
})
```

<font color=red>注：不返回或者undefined：进行默认导航。</font>

2）返回一个路由地址

① 返回一个字符串

```javascript
router.beforeEach((to, from) => {
    if (to.path === "/home") {
        return "/plugins"
    }
})
```

② 返回一个对象

```javascript
router.beforeEach((to, from) => {
    if (to.path === "/home") {
        return {
            path: "/plugins", query: {
                name: "你好"
            }
        }
    }
})
```

3）next

在Vue3中我们是通过返回值来控制的，不再推荐使用next函数，这是因为开发中很容易调用多次next；

# 十三、完整的导航解析流程

1、导航被触发。

2、在失活的组件里调用  beforeRouteLeave 守卫

3、调用全局的 beforeEach 守卫

4、在重用的组件里调用 beforeRouteUpdate 守卫（组件内）

5、在路由配置里调用 beforeEnter（路由对象中）

6、解析异步路由组件

7、在被激活的组件里调用 beforeRouteEnter

8、调用全局的 beforeResolve 守卫（全局解析守卫，这个时候导航已经确定了）

9、导航被确认

10、调用全局的 afterEach 钩子

11、触发 DOM 更新

12、调用 beforeRouteEnter（这里不能拿到this，也就是不能拿到当前的组件实例）守卫中传给 next 的回调函数（next里面有个参数，它可以获取到组件实例的），创建好的组件实例会作为回调函数的参数传入

<hr />

文档：<https://next.router.vuejs.org/zh/guide/advanced/navigation-guards.html>
