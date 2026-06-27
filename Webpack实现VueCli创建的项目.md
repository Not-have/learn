以下webpack的配置详情，请查看以下地址：<https://juejin.cn/post/7010284695850057735>

# 一、安装

```bash
# 因为现在的默认版本是vue2，加@next安装的就是vue3
npm install vue@next
```

## 1、webpack的起始配置如下

注：这个里面的配置使用参考webpack5+的基础配置。

```javascript
const path = require("path");
const { CleanWebpackPlugin } = require("clean-webpack-plugin");
const HtmlWebpackPlugin = require("html-webpack-plugin");
const { DefinePlugin } = require("webpack");
const CopyWebpackPlugin = require("copy-webpack-plugin")
module.exports = {
    mode: "development",
    devtool: "source-map",
    entry: "./src/main.js",
    output: {
        path: path.resolve(__dirname, "./build"),
        filename: "js/main.js" // 指定js的打包目录
    },
    module: {
        rules: [
            {
                test: /\.css$/,
                use: [
                    "style-loader",
                    "css-loader",
                    "postcss-loader",
                ]
            },
            {
                test: /\.(jpe?g|png|gif|svg)$/,
                type: "asset",
                generator: {
                    filename: "img/[name]_[hash:6][ext]",
                },
                parser: {
                    dataUrlCondition: {
                        maxSize: 10 * 1024,
                    }
                }
            },
            {
                test: /\.(eot|ttf|woff2?)$/,
                type: "asset/resource",
                generator: {
                    filename: "font/[name]-[hash:8][ext]"
                }
            },
            {
                test: /\.js$/,
                loader: "babel-loader"
            }
        ]
    },
    plugins: [
        new CleanWebpackPlugin(),
        new HtmlWebpackPlugin({
            title: "指定html打包模板",
            template: "./public/index.html",
        }),
        new DefinePlugin({
            BASE_URL: " './' "
        }),
        new CopyWebpackPlugin({
            patterns: [
                {
                    from: "public",
                    to: "./",
                    globOptions: {
                        ignore: [
                            "**/index.html"
                        ]
                    }
                }
            ]
        })
    ]
}
```

注：下面这两个是从webpack中抽取出来的，也可以  选择不抽离。

1）babel.config.js里面的配置

```javascript
module.exports = {
    presets: [
        /**
         * 这个里面可以写多个
         * 给里面每个配置参数的写法如下：
           ["插件名",{}]
         */
        "@babel/preset-env"
    ]
}
```

2）postcss.config.js

```javascript
module.exports = {
    // 指定你使用的插件
    plugins: [
        // 自动给浏览器加上前缀
        require("postcss-preset-env")
    ]
}
```

## 2、依赖的下载文件

package.json

```json
{
    "name": "webpack-demo",
    "version": "1.0.0",
    "main": "index.js",
    "scripts": {
        "build": "webpack",
        "serve": "webpack serve"
    },
    "author": "",
    "license": "ISC",
    "devDependencies": {
        "@babel/cli": "^7.15.4",
        "@babel/core": "^7.15.5",
        "@babel/plugin-transform-arrow-functions": "^7.14.5",
        "@babel/plugin-transform-block-scoping": "^7.15.3",
        "@babel/preset-env": "^7.15.6",
        "@vue/compiler-sfc": "^3.2.11",
        "autoprefixer": "^10.3.4",
        "babel-loader": "^8.2.2",
        "clean-webpack-plugin": "^4.0.0",
        "copy-webpack-plugin": "^9.0.1",
        "css-loader": "^6.2.0",
        "file-loader": "^6.2.0",
        "html-webpack-plugin": "^5.3.2",
        "less": "^4.1.1",
        "less-loader": "^9.0.0",
        "postcss": "^8.3.6",
        "postcss-cli": "^8.3.1",
        "postcss-loader": "^6.1.1",
        "postcss-preset-env": "^6.7.0",
        "style-loader": "^3.2.1",
        "vue-loader": "^16.5.0",
        "webpack": "^5.52.0",
        "webpack-cli": "^4.8.0",
        "webpack-dev-server": "^4.2.1",
        "webpack-merge": "^5.8.0"
    },
    "dependencies": {
        "vue": "^3.2.11"
    },
    "keywords": [],
    "description": ""
}
```

# 二、使用

## 1、创建一个目录（这个是根目录）

1）在目录下创建public，里面有一个favicon.ico、和index.html

 [favicon.ico](https://github.com/Not-have/picture/blob/935f63535031686655642bde46739fecf5449757/favicon.ico)

```html
<!DOCTYPE html>
<html lang="">
  <head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width,initial-scale=1.0">
    <link rel="icon" href="<%= BASE_URL %>favicon.ico">
    <title><%= htmlWebpackPlugin.options.title %></title>
  </head>
  <body>
    <noscript>
      <strong>We're sorry but <%= htmlWebpackPlugin.options.title %> doesn't work properly without JavaScript enabled. Please enable it to continue.</strong>
    </noscript>
    <div id="app"></div>
    <!-- built files will be auto injected -->
  </body>
</html>
```

2）在目录下 创建一个入口文件main.js

```javascript
/*
 下面这个是正确的写法
*/
// 引入vue 这个是可以解析template的
import { createApp } from "vue/dist/vue.esm-bundler";
/**
* 如果使用SFC文件（.vue），这个时候的引入，可以是下面的这中形式，因为vue-loader会对他进行解析
* import { createApp } from "vue";
*/

const app = createApp({
    template: "<h2>你好！<h2> ",
    data() {
        return {
            
        }
    },
})
// 在html中要有一个对应的元素
app.mount("#app")
```

注：

 ![image-20210912152121251](./images/202203272337148.png)

上面这样写，页面视图并不会渲染，并且在控制台还会有一个警告：

 ![image-20210912152233469](./images/202203272338921.png)

因为源代码解析有两个版本，所以我们要选择一个可以解析的版本：

vue版本一：runtime + compiler  这个是可以解析。

vue版本二： runtime + only  这个是默认的，用他的时候，页面渲染不出来demo元素。

## 2、目录结构如下

 ![image-20210912153730638](./images/202203272338394.png)

# 三、编写 App.vue 文件

## 1、把main.js里面的html和js、css进行分离

1）在src下创建App.vue

```html
<template>
    <h2>你好！</h2>
</template>
<script>
// 这个里面可以引入组件
export default {
    data() {
        return {
            
        }
    },
}
</script>
<style scoped>
</style>
```

2）在main.js中引入

```javascript
// 引入vue 这个是可以解析template的
import { createApp } from "vue/dist/vue.esm-bundler";
import App from "./App.vue";
const app = createApp(App)
// 在html中要有一个对应的元素
app.mount("#app")
```

但是这个时候，你去`npm run build`他会编译失败，因为你有引入了一个新的文件格式，他不能进行识别，所以 你要下载 loader。

## 2、下载loader识别 .vue 文件

1）安装loader

```bash
npm install vue-loader@next -D
# vue-loader本身是依赖@vue/compiler-sfc这个的，这个包是对单文件进行解析的
npm install @vue/compiler-sfc -D
```

2）在webpack.config.js中进行配置

```javascript
const path = require("path");
const { CleanWebpackPlugin } = require("clean-webpack-plugin");
const HtmlWebpackPlugin = require("html-webpack-plugin");
const { DefinePlugin } = require("webpack");
const CopyWebpackPlugin = require("copy-webpack-plugin")
// 引入vue-loader中的plugin
const { VueLoaderPlugin } = require("vue-loader/dist/index")
module.exports = {
    mode: "development",
    devtool: "source-map",
    entry: "./src/main.js",
    output: {
        path: path.resolve(__dirname, "./dist"),
        filename: "js/main.js" // 指定js的打包目录
    },
    module: {
        rules: [
            {
                test: /\.css$/,
                use: [
                    "style-loader",
                    "css-loader",
                    "postcss-loader",
                ]
            },
            {
                test: /\.(jpe?g|png|gif|svg)$/,
                type: "asset",
                generator: {
                    filename: "img/[name]_[hash:6][ext]",
                },
                parser: {
                    dataUrlCondition: {
                        maxSize: 10 * 1024,
                    }
                }
            },
            {
                test: /\.(eot|ttf|woff2?)$/,
                type: "asset/resource",
                generator: {
                    filename: "font/[name]-[hash:8][ext]"
                }
            },
            {
                test: /\.js$/,
                loader: "babel-loader"
            },
            {
                test: /\.vue$/,
                loader: "vue-loader"
            }
        ]
    },
    plugins: [
        new CleanWebpackPlugin(),
        new HtmlWebpackPlugin({
            title: "指定html打包模板",
            template: "./public/index.html",
        }),
        new DefinePlugin({
            BASE_URL: " './' "
        }),
        new CopyWebpackPlugin({
            patterns: [
                {
                    from: "public",
                    to: "./",
                    globOptions: {
                        ignore: [
                            "**/index.html"
                        ]
                    }
                }
            ]
        }),
        new VueLoaderPlugin()
    ]
}
```

## 3、配置别名和匹配 .vue 文件

webpack.config.js

```javascript
const path = require("path");
const { CleanWebpackPlugin } = require("clean-webpack-plugin");
const HtmlWebpackPlugin = require("html-webpack-plugin");
const { DefinePlugin } = require("webpack");
const CopyWebpackPlugin = require("copy-webpack-plugin")
// 引入vue-loader中的plugin
const { VueLoaderPlugin } = require("vue-loader/dist/index")
module.exports = {
    target: "web",
    mode: "development",
    devtool: "source-map",
    entry: "./src/main.js",
    output: {
        path: path.resolve(__dirname, "./dist"),
        filename: "js/main.js" // 指定js的打包目录
    },
    devServer: {
        /**
         * 模块的热替换：
         * 模块热替换是指在 应用程序运行过程中，替换、添加、删除模块，而无需重新刷新整个页面
         * 比如：你在使用计算器的时候，你不想修改代码之后，计算器的结果也丢失的情况下 使用
         * 
         */
        hot: true,
        // host: "0.0.0.0",
        port: 7777,
        /**
         * 是否为静态文件开启gzip 
         * 默认值是 false
         */
        compress: true,
    },
    resolve: {
        /**
         * 如果是一个文件:
         * 他会根据extensions里面写的自动给路径后面匹配，去文件夹中查找相应的文件
         * extensions 里面的默认值有：[".js", ".json", ".mjs", ".wasm"]
         * 可以给下面加入 .vue .ts .jsx
         * 下面这样写，在引入vue文件的时候，就不用写后缀了
         */
        extensions: [".js", ".json", ".mjs", ".wasm", ".vue"],
        /**
         * 如果是一个文件夹
         * 会在文件夹中根据 resolve.mainFiles配置选项中指定的文件顺序查找
         * resolve.mainFiles的默认值是 ['index']
         * 再根据 resolve.extensions来解析扩展名
         * mainFiles 一般不配置
         */
        mainFiles: ["index"],
        /**
         * 是配置别名alias
         * "别名名字": path.resolve(__dirname, "相对与配置文件的相对路径")
         */
        alias: {
            "@": path.resolve(__dirname, "./src")
        }
    },
    module: {
        rules: [
            {
                test: /\.css$/,
                use: [
                    "style-loader",
                    "css-loader",
                    "postcss-loader",
                ]
            },
            {
                test: /\.(jpe?g|png|gif|svg)$/,
                type: "asset",
                generator: {
                    filename: "img/[name]_[hash:6][ext]",
                },
                parser: {
                    dataUrlCondition: {
                        maxSize: 10 * 1024,
                    }
                }
            },
            {
                test: /\.(eot|ttf|woff2?)$/,
                type: "asset/resource",
                generator: {
                    filename: "font/[name]-[hash:8][ext]"
                }
            },
            {
                test: /\.js$/,
                loader: "babel-loader"
            },
            {
                test: /\.vue$/,
                loader: "vue-loader"
            }
        ]
    },
    plugins: [
        new CleanWebpackPlugin(),
        new HtmlWebpackPlugin({
            title: "指定html打包模板",
            template: "./public/index.html",
        }),
        new DefinePlugin({
            BASE_URL: " './' ",
            /**
             * 下面这两个的配置，请查看github文档：
             * https://github.com/vuejs/vue-next/tree/master/packages/vue#bundler-build-feature-flags
             */
            __VUE_OPTIONS_API__: true,
            __VUE_PROD_DEVTOOLS__: false
        }),
        new CopyWebpackPlugin({
            patterns: [
                {
                    from: "public",
                    to: "./",
                    globOptions: {
                        ignore: [
                            "**/index.html"
                        ]
                    }
                }
            ]
        }),
        new VueLoaderPlugin()
    ]
}
```

# 四、webpack.config.js 环境区分

## 1、下载

```bash
# 合并webpack的配置文件
npm install webpack-merge -D
```

## 2、在跟目录下创建config

里面创建一下三个文件

webpack.common.config.js  公共的

webpack.dev.config.js  开发环境

webpack.prod.config.js  生产环境

## 3、在package.json中重新配置

以下的文件路径是指配置文件的相对路径。

```json
    "scripts": {
        "build": "webpack --config ./config/webpack.prod.config.js",
        "serve": "webpack serve --config ./config/webpack.dev.config.js"
    },
```

## 4、文件里面的内容

### 1）webpack.common.config.js

```javascript
const path = require("path");
const HtmlWebpackPlugin = require("html-webpack-plugin");
const { DefinePlugin } = require("webpack");

// 引入vue-loader中的plugin
const { VueLoaderPlugin } = require("vue-loader/dist/index")
module.exports = {
    target: "web",
    entry: "./src/main.js",
    output: {
        path: path.resolve(__dirname, "../dist"),
        filename: "js/main.js" // 指定js的打包目录
    },
    
    resolve: {
        extensions: [".js", ".json", ".mjs", ".wasm", ".vue"],
        // mainFiles: ["index"],
        alias: {
            "@": path.resolve(__dirname, "../src")
        }
    },
    module: {
        rules: [
            {
                test: /\.css$/,
                use: [
                    "style-loader",
                    "css-loader",
                    "postcss-loader",
                ]
            },
            {
                test: /\.(jpe?g|png|gif|svg)$/,
                type: "asset",
                generator: {
                    filename: "img/[name]_[hash:6][ext]",
                },
                parser: {
                    dataUrlCondition: {
                        maxSize: 10 * 1024,
                    }
                }
            },
            {
                test: /\.(eot|ttf|woff2?)$/,
                type: "asset/resource",
                generator: {
                    filename: "font/[name]-[hash:8][ext]"
                }
            },
            {
                test: /\.js$/,
                loader: "babel-loader"
            },
            {
                test: /\.vue$/,
                loader: "vue-loader"
            }
        ]
    },
    plugins: [
        new HtmlWebpackPlugin({
            title: "指定html打包模板",
            template: "./public/index.html",
        }),
        new DefinePlugin({
            BASE_URL: " './' ",
            /**
             * 下面这两个的配置，请查看github文档：
             * https://github.com/vuejs/vue-next/tree/master/packages/vue#bundler-build-feature-flags
             */
            __VUE_OPTIONS_API__: true,
            __VUE_PROD_DEVTOOLS__: false
        }),
        new VueLoaderPlugin()
    ]
}
```

### 2）webpack.dev.config.js

```javascript
const { merge } = require('webpack-merge');
const commonConfig = require('./webpack.common.config');
const path = require("path");
module.exports = merge(commonConfig, {
    mode: "development",
    devtool: "source-map",
    devServer: {
        /**
        * webpack-dev-server4+ 以后更新了配置参数，具体的见下方文档：
        * https://webpack.docschina.org/configuration/dev-server/
        */
        static: {
            directory: path.join(__dirname, '../public'),
        },
        // 在浏览器展示console.log编译进度
        client: {
            progress: true,
            // 报错时浏览器全屏覆盖
            overlay: true,
        },
        hot: true,
        // host: "0.0.0.0",
        port: 7777,
        // open: true,
        // compress: true,
    }
})
```

### 3）webpack.prod.config.js

```javascript
/**
 * 生产环境
 */
const { CleanWebpackPlugin } = require("clean-webpack-plugin");
const CopyWebpackPlugin = require("copy-webpack-plugin");

const { merge } = require("webpack-merge");
const commonConfig = require("./webpack.common.config");
module.exports = merge(commonConfig, {
    mode: "production",
    plugins: [
        new CleanWebpackPlugin(),
        /**
         * 他会择机根据跟录下去查找
         */
        new CopyWebpackPlugin({
            patterns: [
                {
                    from: "./public",
                    to: "./",
                    globOptions: {
                        ignore: [
                            "**/index.html"
                        ]
                    }
                }
            ]
        })
    ]
})
```

![image-20210921185944336](./images/202203272338054.png)

代码地址：<https://www.lanzouw.com/ifYk0ubka2d>
