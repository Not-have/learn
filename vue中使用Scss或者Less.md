# 一、CSS预处理器的使用

## 1、下载插件

```text
npm install style-resources-loader --save
npm install vue-cli-plugin-style-resources-loader --save
```

## 2、在根目录下创建vue.config.js

```javascript
let path = require('path');

module.exports = {
    // 第三方插件配置（scss和它一样）
    pluginOptions: {
        'style-resources-loader': {
            // less的话  这里改成less
            preProcessor: 'scss',
            //在assets（静态资源文件夹）下创建全局样式文件；index.less 它就是less全局变量
            patterns: [path.resolve(__dirname, './src/styles/index.scss')]
        }
    }
}
```

## 3、使用

```vue
<template>
    <div class="content">
    </div>
</template>

<style lang="scss" scoped>
/*
    // 下面这种使用方式，不需要在全局引入
    @import "~@/style/variables.scss";
*/
.content{
    width: calc(100% - #{$sideBarWidth});
}
</style>
```

# 二、vue项目中使用icon图标库

## 1、去iconfont上下载

地址：<https://www.iconfont.cn/>

## 2、下载

![](./images/202203260257382.png)

*注：把下载的文件解压后放入assets——>icon（自己创建）文件夹里。*

## 3、使用

```html
<template>
    <div>
        <span class="iconfont icon-cake-1"></span>
    </div>
</template>
<script>
/* 这块也可以在main.js里面全局引入*/
import "@/assets/icon/iconfont.css";
</script>
<style scoped>
.icon-cake-1{
    color: black;
}
</style>
```

  ![image-20210830182404804](./images/202203260257650.png)

# 三、vue中使用svg图

## 1、下载

```bash
npm i --save svg-sprite-loader
```

## 2、在webpack中配置

```javascript
const path = require('path')
function resolve(dir) {
    return path.join(__dirname, dir)
}
module.exports = {
    chainWebpack(config) {
        config.module
            .rule('svg')
            // svg 图的存放文件
            .exclude.add(resolve('src/assets/svg'))
            .end()
        config.module
            .rule('icons')
            .test(/\.svg$/)
            // svg 图的存放文件
            .include.add(resolve('src/assets/svg'))
            .end()
            .use('svg-sprite-loader')
            .loader('svg-sprite-loader')
            .options({
                symbolId: 'icon-[name]'
            })
            .end()
    },
}
```

## 3、在根目录下——>src——>assets——>svg里面放入 svg图

## 4、在components里面创建svg文件夹

1）创建vue组件

```javascript
<template>
    <svg :class="svgClass" aria-hidden="true" v-on="$listeners">
        <use :xlink:href="iconName" />
    </svg>
</template>

<script>
export default {
    name: 'SvgIcon',
    props: {
        iconClass: {
            type: String,
            required: true
        },
        className: {
            type: String,
            default: ''
        }
    },
    computed: {
        iconName() {
            return `#icon-${this.iconClass}`
        },
        svgClass() {
            if (this.className) {
                return 'svg-icon ' + this.className
            } else {
                return 'svg-icon'
            }
        },
    }
}
</script>

<style scoped>
.svg-icon {
    width: 1em;
    height: 1em;
    vertical-align: -0.15em;
    fill: currentColor;
    overflow: hidden;
}
.svg-external-icon {
    background-color: currentColor;
    mask-size: cover !important;
    display: inline-block;
}
</style>
```

2）将组件注册到vue中

```javascript
import Vue from 'vue'
import SvgIcon from './svg.vue'

Vue.component('svg-icon', SvgIcon)
const req = require.context('../../assets/svg', false, /\.svg$/)
const requireAll = requireContext => requireContext.keys().map(requireContext)
requireAll(req)
```

## 5、将注册的组件 引入全局(main.js)

```javascript
import "@/components/svg/index.js";
```

## 6、使用

```javascript
<template>
    <svg-icon iconClass="svg图的文件名"  className="box"></svg-icon>
</template>

<style scoped>
.box{
    color: red;
}
</style>
```

注：如果修改 不改变颜色，请删除svg图中的style标签里 的每一项fill样式设置。
