## 一、开发Vue或React项目中路径别名智能提示方案

1、在根目录下创建jsconfig.json

```javascript
{
    "compilerOptions": {
        "experimentalDecorators": true,
        "baseUrl": "./",
        "paths": {
            "@/*": ["src/*"]
        }
    },
    "exclude": [
        "node_modules",
        "dist"
    ]
}
```

然后重启vsc；

## 二、配置开发、测试、生产 不同的环境

1、开发（ 在根目录下创建 *.env.development*  ）

```javascript
# 开发
ENV = 'development'

# 地址
VUE_APP_BASE_API = '地址'
```

2、测试（ 在根目录下创建  *.env.test*  ）

```javascript
NODE_ENV = production

# 测试
ENV = 'staging'

# base api
VUE_APP_BASE_API = '地址'
```

3、生产（在根目录下创建 *.env.production* ）

```javascript
# 生产
ENV = 'production'

# 地址
VUE_APP_BASE_API = '地址'
```

4、在 <font color=red>package.json</font>配置指令

注：主要是修改scripts里面的东西，linet建议删掉。

```json
"scripts": {
    "serve": "vue-cli-service serve --mode development",
    "build:test": "vue-cli-service build --mode test",
    "build:prod": "vue-cli-service build --mode production",
    "lint": "vue-cli-service lint"
},
```

5、在代码中使用

```javascript
console.log(process.env.VUE_APP_SECRET)
// 获取环境
console.log(process.env.NODE_ENV)
```

## 三、vue.config.js配置

[webpack](https://webpack.docschina.org/concepts/)：<https://webpack.docschina.org/concepts>

以下需要下载的npm插件：

```
npm install --save webpack-require-http  
npm install terser-webpack-plugin --save-dev
npm install --save compression-webpack-plugin
```

文档：[CompressionWebpackPlugin](https://www.webpackjs.com/plugins/compression-webpack-plugin/)：<https://www.webpackjs.com/plugins/compression-webpack-plugin/>

```javascript
const TerserPlugin = require('terser-webpack-plugin')
const CompressionWebpackPlugin = require('compression-webpack-plugin');
const path = require('path');
const { config } = require('process');
function resolve(dir) {
    return path.join(__dirname, dir)
}
module.exports = {
    // 基本路径
    publicPath: './',
    outputDir: 'dist',
    assetsDir: 'static',//将静态文件 放置在一个文件夹里
    runtimeCompiler: false,
    productionSourceMap: false,
    lintOnSave: false,

    chainWebpack: config => {
        //移除prefetch（添加 确保路由的懒加载）
        config.plugins.delete('prefetch')
        //图片 压缩
        config.module
            .rule('images')
            .use('url-loader')
            .loader('url-loader')
            .tap(options => Object.assign(options, { limit: 10 }))
    },

    configureWebpack: {
        //关闭 webpack 的性能提示
        performance: {
            hints: false
        },
        resolve: {
            alias: {
                '@': resolve('src')
            }
        },
        //使用cdn
        externals: [
            require('webpack-require-http'),
            /**
            当然需要在public——> index.html里面引入在线路径
             * 在vue.config.js里面引入了，就不要在外面引用、使用
              import Vue from "vue";
              import VueRouter from "vue-router";
              Vue.use(VueRouter);
             */
            {
                Vue: "vue",
                VueRouter: "vue-router"
            }
        ],
        //启用gzip压缩
        plugins: [
            new CompressionWebpackPlugin({
                filename: '[path][name].gz[query]',
                algorithm: 'gzip',
                test: /\.(js|css|json|ttf)(\?.*)?$/i,
                threshold: 1024,
                minRatio: 0.8,
                deleteOriginalAssets: false
            }),
   //取消console.log打印
            new TerserPlugin({
                terserOptions: {
                    ecma: undefined,
                    compress: {
                        drop_console: true,
                        drop_debugger: false,
                        pure_funcs: ['console.log'],
                    }
                }
            })
        ],
    },
    css: {
        extract: true,
        sourceMap: false,
        loaderOptions: {},
        modules: false
    },
    parallel: require('os').cpus().length > 1,
    pwa: {},
    devServer: {
        disableHostCheck: true,
        open: false,//启动完成 是否打开页面
        host: '0.0.0.0',
        port: 8080, //端口号:自己修改（可以快速修改地址，防止有时候，产生冲突）
        https: false,
        hotOnly: false,
    },
    // 第三方插件配置（scss和它一样）
    pluginOptions: {
        'style-resources-loader': {
            preProcessor: 'less',
            //在assets（静态资源文件夹）下创建全局样式文件；index.less 它就是less全局变量
            patterns: [path.resolve(__dirname, './src/assets/style/index.less')] // less所在文件路径
        }
    }
}
```

## 四、在vue中配置*eslint*

1、在根目录下创建 **.eslintrc.js**

```javascript
/*
 * @Author: 李勇
 * @Date: 2021-01-18 08:49:21
 * @LastEditors: 李勇
 * @LastEditTime: 2021-01-18 20:25:13
 * @FilePath: /we-chat/.eslintrc.js
 * @Description: 
 * 具体配置参看下面的博客：
   https://blog.csdn.net/mocoe/article/details/86759404
   https://blog.csdn.net/weixin_38606332/article/details/80864381
   
 */
module.exports = {
    root: true,
    env: {
        node: true,
        es6: true
    },
    extends: ['plugin:vue/recommended', 'eslint:recommended'],
    parserOptions: {
        parser: "babel-eslint"
    },
    rules: {
        "indent": [2, 4],
        'vue/html-self-closing': [
            'error',
            {
                html: {
                    void: 'never',
                    normal: 'never',
                    component: 'never',
                },
                svg: 'always',
                math: 'always',
            },
        ],
        "no-unused-vars": [2, {
            // 允许声明未使用变量
            "vars": "local",
            // 参数不检查
            "args": "none"
        }]
    }
}
```

2、删除*package.json*中的一部分代码

<font face="黑体" color=red>注：</font> 如果不删除也可以在*package.json*里面进行配置，配置方法同 *.eslintrc.js*里面：

3、在*vue.config.js*里面不要禁用*eslint*检查

```javascript
const path = require('path')
function resolve(dir) {
    return path.join(__dirname, dir)
}
module.exports = {
    // 基本路径
    publicPath: './', //部署应用包时的基本 URL
    outputDir: 'dist', // 输出文件目录
    assetsDir: '',//放置生成的静态资源 (js、css、img、fonts) 的 (相对于 outputDir 的) 目录
    runtimeCompiler: false, //是否使用包含运行时编译器的 Vue 构建版本。设置为true可以使用template
    productionSourceMap: false,//生产环境是否生成 sourceMap 文件
    lintOnSave: true,//为false的时候  关闭eslint 校验代码
    chainWebpack(config) {
        config.resolve.alias
        //     .set('style', resolve('public/style'))
        config.output.filename('js/[name].[hash:16].js');//hash值设置
        config.output.chunkFilename('js/[id].[hash:16].js');
        // config.output.filename('css/[name].[hash:16].css');//hash值设置
    },
    configureWebpack: {
        //关闭 webpack 的性能提示
        performance: {
            hints:false
        },
        resolve: {
            //配置别名
            alias: {
                '@': resolve('src')
            }
        },
        externals: {
            AMap: "window.AMap"
        }
    },
    // css相关配置
    css: {
        // 是否使用css分离插件 ExtractTextPlugin
        extract: true,
        // 开启 CSS source maps?
        sourceMap: false,
        // css预设器配置项
        loaderOptions: {},
        // 启用 CSS modules for all css / pre-processor files.
        modules: false
    },
    parallel: require('os').cpus().length > 1,//是否为 Babel 或 TypeScript 使用 thread-loader
    // PWA 插件相关配置
    // see https://github.com/vuejs/vue-cli/tree/dev/packages/%40vue/cli-plugin-pwa
    pwa: {},
    // webpack-dev-server 相关配置
    devServer: {
        disableHostCheck: true,
        open: process.platform === 'darwin',
        host: '0.0.0.0',
        port: 8080,  //端口号:自己修改（可以快速修改地址，防止有时候，产生冲突）
        https: false,
        hotOnly: false,
        /**
         * 设置代理 
         * 这里对他们两个地址 进行了 跨域的处理
         */
        /*
        proxy: {
            '/api1': {// /api 顶替了'localhost:3000'这个地址
                target: 'http://XXX.XX.XXX.XX', //对应自己的接口（后台给你的地址；前面有个地址，改变的是后面的路由的地址, 一般都是服务器（即网址，一般不会改变的）/xxx，）
                changeOrigin: true,
                ws: true,
                pathRewrite: {
                    '^/api1': ''
                }
            },
            //配置第二个可跨域的设置
            '/api2': {// /api 顶替了'localhost:3000'这个地址
                target: 'http://XXX.XX.XXX.XX', //对应自己的接口（后台给你的地址；前面有个地址，改变的是后面的路由的地址, 一般都是服务器（即网址，一般不会改变的）/xxx，）
                changeOrigin: true,
                ws: true,
                pathRewrite: {
                    '^/api2': ''
                }
            }    
        },
        */
    },
    // 第三方插件配置
    pluginOptions: {
        //使用less或者scss的时候
        'style-resources-loader': {
            preProcessor: 'less',
            //在assets（静态资源文件夹）下创建全局样式文件；index.less 它就是less全局变量
            patterns: [path.resolve(__dirname, './src/assets/style/index.less')] // less所在文件路径
        }
    }
}
```

## 五、在vue项目中使用艺术字

1、在assets下创建font文件夹，放入字体包

如[jindianyahei](https://gitee.com/Green_chicken/picture.git)

2、在App.vue的style中引入

```css
/* 声明字体 */
@font-face {
    font-family:jindianyahei;/*自己起的名字*/
    src: url(./assets/font/zihun35hao-jindianyahei.ttf);
}
```

3、使用

```javascript
<template>
    <div>
        你好
    </div>
</template>
<script>
import "@/utils/amfe-flexible.js";
import TopBar from "@/components/TopBar.vue";
export default {
    components: {
        TopBar
    }
}
</script>
<style scoped>
div{
    font-size:30px;
    font-family:jindianyahei ;/*保持与上面取的名字一致*/
}
</style>
```
