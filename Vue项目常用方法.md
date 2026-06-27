## 1、在vue项目中判断按钮权限

1）在utils——>button-permissions

```typescript
//引入vuex
import store from "../store";
export function hasPermission(permission) {
    //获取vuex模块化里面的数据
    let allow = store.state.button.button;
    if (allow.length > 0) {
        //来判断一个数组是否包含一个指定的值，包含则返回 true，否则返回false
        return allow.includes(permission);
    }
}
```

2）在vuex获取按钮权限的数据

文件结构如下：![image-20210303141459628](https://gitee.com/Green_chicken/picture/raw/master/img/20210303141501.png)

```typescript
const state = {
    button:["approval"]
}
const mutations = {}
const actions = {}
export default {
    namespaced: true,
    state,
    mutations,
    actions
}
```

3）使用

①挂载在vue3.0的全局

```javascript
import { createApp } from 'vue'
import App from './App.vue'
import router from './router'
import store from './store'
// 引入elementUI
import ElementPlus from 'element-plus';
import 'element-plus/lib/theme-chalk/index.css';
//这个是引入中文的element-plus
import locale from 'element-plus/lib/locale/lang/zh-cn'
//全局修改element-plusUI的样式
import "./assets/style/elementUI.scss";

// 这块 不适用链式的，所以修改成下面的
/* 这个是链接式：createApp(App).use(store).use(router).mount('#app')*/
// 全局引入变量
import { hasPermission } from "@/utils/button-permissions";

const app = createApp(App);
//这个是vue3规定的语法
app.config.globalProperties.hasPermission = hasPermission;
app.use(hasPermission);

//这个是引入element-plus中文
app.use(ElementPlus, { locale });
app.use(store);
app.use(router);
app.mount('#app');
```

```javascript
<template>
    <!-- 根据里面的参数，去显示 -->
    <div v-if="hasPermission('qqq')">
        申请点号 
    </div>
</template>
```

<font color=red>②</font>局部使用（尤大推荐的）,不用在main.js里面引入

```javascript
<template>
    <div v-if="hasPermission('qqq')">
        申请点号
        
    </div>
</template>
<script lang="ts">
import { hasPermission } from "@/utils/button-permissions";
export default {
    setup(){
        return {hasPermission}
    }
}
</script>
<style lang="scss" scoped>

</style>
```

注：vue2挂载全局的方法

```javascript
import Vue from 'vue'
import App from './App'

Vue.config.productionTip = false
//引入的方法
const height = "你好！我是全局变量或者方法";
Vue.prototype.height = height;

new Vue({
  el: '#app',
  render: h => h(App)
})
```

## 2、vue2中重置data中的某个变量

注：该方法 一般用在 搜索处

```html
<template>
    <el-form ref="form" :model="form">
        <el-form-item label="" prop="">
            <el-select v-model="form.logLevel" placeholder="请选择活动区域" filterable clearable>
                <el-option label="区域一" value="shanghai"></el-option>
                <el-option label="区域二" value="beijing"></el-option>
            </el-select>
        </el-form-item>
        <el-form-item label="">
            <el-button type="primary" icon="el-icon-refresh" @click="queryClick('reset')"></el-button>
        </el-form-item>
    </el-form>
</template>
<script>
export default {
    data() {
        return {
            form: {
                logLevel: '',
            }
        }
    },
    methods: {
        /**
        * 重置选择框，也就是清空
        */
        queryClick(val) {
            Object.assign(this.form, this.$options.data().form);  
        }
    }
}
</script>
```
