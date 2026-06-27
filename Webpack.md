# 一、认识Webpack

webpack地址：<https://www.webpackjs.com/>

​        webpack 是一个现代 JavaScript 应用程序的*静态模块打包器(module bundler)*。当 webpack 处理应用程序时，它会递归地构建一个*依赖关系图(dependency graph)*，其中包含应用程序需要的每个模块，然后将所有这些模块打包成一个或多个 *bundle*。

① 打包bundler：webpack可以将帮助我们进行打包，所以它是一个打包工具

② 静态的static：这样表述的原因是我们最终可以将代码打包成最终的静态资源（部署到静态服务器）；

③ 模块化module：webpack默认支持各种模块化开发，ES Module、CommonJS、AMD等；

④ 现代的modern：我们前端说过，正是因为现代前端开发面临各种各样的问题，才催生了webpack的出现和发 展。

<svg viewBox="0 0 1088 615" version="1.1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" style="background:#2B3A42">
    <g stroke-width="1" fill="none" fill-rule="evenodd">
        <text font-family="'Source Sans Pro', sans-serif" font-size="18" font-weight="600" fill="#86A5BA">
            <tspan x="933.778" y="459">静态资产</tspan>
        </text>
        <g transform="translate(1002, 326)">
            <g>
                <use fill-opacity="0.1" fill="#526B78" fill-rule="evenodd" xlink:href="#path-1"></use>
                <rect stroke="#526B78" stroke-width="4" x="-2" y="-2" width="84" height="84" rx="3"></rect>
            </g>
            <text font-family="'Source Sans Pro', sans-serif" font-size="22" font-weight="500" fill="#FFFFFF">
                <tspan x="18.891" y="46.7096774">.png</tspan>
            </text>
        </g>
        <g transform="translate(1002, 214)">
            <g>
                <use fill-opacity="0.1" fill="#526B78" fill-rule="evenodd" xlink:href="#path-2"></use>
                <rect stroke="#526B78" stroke-width="4" x="-2" y="-2" width="84" height="84" rx="3"></rect>
            </g>
            <text font-family="'Source Sans Pro', sans-serif" font-size="22" font-weight="500" fill="#FFFFFF">
                <tspan x="22.532" y="46.7096774">.css</tspan>
            </text>
        </g>
        <g transform="translate(894, 326)">
            <g>
                <use fill-opacity="0.1" fill="#526B78" fill-rule="evenodd" xlink:href="#path-3"></use>
                <rect stroke="#526B78" stroke-width="4" x="-2" y="-2" width="84" height="84" rx="3"></rect>
            </g>
            <text font-family="'Source Sans Pro', sans-serif" font-size="22" font-weight="500" fill="#FFFFFF">
                <tspan x="21.817" y="46.7096774">.jpg</tspan>
            </text>
        </g>
        <g transform="translate(894, 214)">
            <g>
                <use fill-opacity="0.1" fill="#526B78" fill-rule="evenodd" xlink:href="#path-4"></use>
                <rect stroke="#526B78" stroke-width="4" x="-2" y="-2" width="84" height="84" rx="3"></rect>
            </g>
            <text font-family="'Source Sans Pro', sans-serif" font-size="22" font-weight="500" fill="#FFFFFF">
                <tspan x="29" y="46.7096774">.js</tspan>
            </text>
        </g>
        <g transform="translate(342, 225)" stroke="#7E8C94" stroke-width="4">
            <path d="M499.558824,86.52 C499.558824,86.52 484.852941,81.02 439.908088,109.436667 C394.963235,137.853333 380.992647,164.436667 380.992647,164.436667" stroke-dasharray="7"></path>
            <path d="M499.558824,86.0616667 C499.558824,86.0616667 484.852941,91.5616667 439.908088,63.145 C394.963235,34.7283333 380.992647,8.145 380.992647,8.145" stroke-dasharray="7"></path>
            <path d="M0.477941176,170.395 C0.477941176,170.395 169.382939,98.895 447.847936,98.895" id="Shape_99_" stroke-dasharray="6"></path>
            <path d="M0.477941176,72.395 C0.477941176,72.395 169.382939,0.895 447.847936,0.895" id="Shape_99_" stroke-dasharray="6" transform="translate(224.162939, 36.645000) scale(1, -1) translate(-224.162939, -36.645000) "></path>
        </g>
        <text font-family="'Source Sans Pro', sans-serif" font-size="18" font-weight="600" fill="#86A5BA">
            <tspan x="24.934" y="562">具有相关性的模块</tspan>
        </text>
        <g transform="translate(228, 335)">
            <g>
                <use fill-opacity="0.1" fill="#526B78" fill-rule="evenodd" xlink:href="#path-5"></use>
                <rect stroke="#526B78" stroke-width="4" x="-2" y="-2" width="66" height="66" rx="3"></rect>
            </g>
            <text font-family="'Source Sans Pro', sans-serif" font-size="22" font-weight="500" fill="#FFFFFF">
                <tspan x="12.817" y="38">.jpg</tspan>
            </text>
        </g>
        <g transform="translate(228, 223)">
            <g>
                <use fill-opacity="0.1" fill="#526B78" fill-rule="evenodd" xlink:href="#path-6"></use>
                <rect stroke="#526B78" stroke-width="4" x="-2" y="-2" width="66" height="66" rx="3"></rect>
            </g>
            <text font-family="'Source Sans Pro', sans-serif" font-size="22" font-weight="500" fill="#FFFFFF">
                <tspan x="9.891" y="38">.png</tspan>
            </text>
        </g>
        <g transform="translate(302, 414.500000) scale(1, -1) translate(-302, -414.500000) translate(182, 404)">
            <rect fill="#BBDBEC" x="0" y="0" width="2" height="6"></rect>
            <rect fill="#BBDBEC" x="76" y="6" width="2" height="12"></rect>
            <rect fill="#BBDBEC" transform="translate(75.304690, 4.704683) rotate(-45) translate(-75.304690, -4.704683) " x="74.3046896" y="1.87968342" width="2" height="5.6500001"></rect>
            <rect fill="#BBDBEC" x="2" y="2" width="72" height="2"></rect>
            <polyline stroke="#BBDBEC" stroke-width="2" points="80 12 77 20.8000002 74 12"></polyline>
        </g>
        <g transform="translate(116, 391)">
            <g>
                <use fill-opacity="0.1" fill="#526B78" fill-rule="evenodd" xlink:href="#path-7"></use>
                <rect stroke="#526B78" stroke-width="4" x="-2" y="-2" width="66" height="66" rx="3"></rect>
            </g>
            <text font-family="'Source Sans Pro', sans-serif" font-size="22" font-weight="500" fill="#FFFFFF">
                <tspan x="8.076" y="38">.sass</tspan>
            </text>
        </g>
        <g transform="translate(116, 279)">
            <g>
                <use fill-opacity="0.1" fill="#526B78" fill-rule="evenodd" xlink:href="#path-8"></use>
                <rect stroke="#526B78" stroke-width="4" x="-2" y="-2" width="66" height="66" rx="3"></rect>
            </g>
            <text font-family="'Source Sans Pro', sans-serif" font-size="22" font-weight="500" fill="#FFFFFF">
                <tspan x="8.076" y="38">.sass</tspan>
            </text>
        </g>
        <g transform="translate(182, 201)">
            <rect fill="#BBDBEC" x="0" y="0" width="2" height="6"></rect>
            <rect fill="#BBDBEC" x="76" y="113" width="2" height="12"></rect>
            <rect fill="#BBDBEC" transform="translate(75.304690, 111.704683) rotate(-45) translate(-75.304690, -111.704683) " x="74.3046896" y="108.879683" width="2" height="5.6500001"></rect>
            <rect fill="#BBDBEC" x="26" y="109" width="48" height="2"></rect>
            <rect fill="#BBDBEC" transform="translate(24.704683, 108.304690) rotate(-45) translate(-24.704683, -108.304690) " x="23.7046835" y="105.47969" width="2" height="5.6500001"></rect>
            <rect fill="#BBDBEC" x="22" y="6" width="2" height="101"></rect>
            <rect fill="#BBDBEC" transform="translate(21.304690, 4.704683) rotate(-45) translate(-21.304690, -4.704683) " x="20.3046896" y="1.87968342" width="2" height="5.6500001"></rect>
            <rect fill="#BBDBEC" x="2" y="2" width="18" height="2"></rect>
            <polyline stroke="#BBDBEC" stroke-width="2" points="80 118 77 126.8 74 118"></polyline>
        </g>
        <g transform="translate(182, 189)">
            <rect fill="#BBDBEC" x="0" y="0" width="2" height="6"></rect>
            <rect fill="#BBDBEC" x="76" y="6" width="2" height="19"></rect>
            <rect fill="#BBDBEC" transform="translate(75.304690, 4.704683) rotate(-45) translate(-75.304690, -4.704683) " x="74.3046896" y="1.87968342" width="2" height="5.6500001"></rect>
            <rect fill="#BBDBEC" x="2" y="2" width="72" height="2"></rect>
            <polyline stroke="#BBDBEC" stroke-width="2" points="80 18 77 26.8000002 74 18"></polyline>
        </g>
        <g transform="translate(116, 167)">
            <g>
                <use fill-opacity="0.1" fill="#526B78" fill-rule="evenodd" xlink:href="#path-9"></use>
                <rect stroke="#526B78" stroke-width="4" x="-2" y="-2" width="66" height="66" rx="3"></rect>
            </g>
            <text font-family="'Source Sans Pro', sans-serif" font-size="22" font-weight="500" fill="#FFFFFF">
                <tspan x="20" y="38">.js</tspan>
            </text>
        </g>
        <g transform="translate(110, 470.500000) scale(1, -1) translate(-190, -470.500000) translate(150, 460)">
            <rect fill="#BBDBEC" x="0" y="0" width="2" height="6"></rect>
            <rect fill="#BBDBEC" x="76" y="6" width="2" height="12"></rect>
            <rect fill="#BBDBEC" transform="translate(75.304690, 4.704683) rotate(-45) translate(-75.304690, -4.704683) " x="74.3046896" y="1.87968342" width="2" height="5.6500001"></rect>
            <rect fill="#BBDBEC" x="2" y="2" width="72" height="2"></rect>
            <polyline stroke="#BBDBEC" stroke-width="2" points="80 12 77 20.8000002 74 12"></polyline>
        </g>
        <g transform="translate(4, 447)">
            <g>
                <use fill-opacity="0.1" fill="#526B78" fill-rule="evenodd" xlink:href="#path-10"></use>
                <rect stroke="#526B78" stroke-width="4" x="-2" y="-2" width="66" height="66" rx="3"></rect>
            </g>
            <text font-family="'Source Sans Pro', sans-serif" font-size="22" font-weight="500" fill="#FFFFFF">
                <tspan x="8.076" y="38">.sass</tspan>
            </text>
        </g>
        <g transform="translate(70, 363)">
            <rect fill="#BBDBEC" x="0" y="0" width="2" height="6"></rect>
            <rect fill="#BBDBEC" x="76" y="6" width="2" height="12"></rect>
            <rect fill="#BBDBEC" transform="translate(75.304690, 4.704683) rotate(-45) translate(-75.304690, -4.704683) " x="74.3046896" y="1.87968342" width="2" height="5.6500001"></rect>
            <rect fill="#BBDBEC" x="2" y="2" width="72" height="2"></rect>
            <polyline stroke="#BBDBEC" stroke-width="2" points="80 12 77 20.8000002 74 12"></polyline>
        </g>
        <g transform="translate(4, 335)">
            <g>
                <use fill-opacity="0.1" fill="#526B78" fill-rule="evenodd" xlink:href="#path-11"></use>
                <rect stroke="#526B78" stroke-width="4" x="-2" y="-2" width="66" height="66" rx="3"></rect>
            </g>
            <text font-family="'Source Sans Pro', sans-serif" font-size="22" font-weight="500" fill="#FFFFFF">
                <tspan x="15.38" y="38">.cjs</tspan>
            </text>
        </g>
        <g transform="translate(38, 307)">
            <rect fill="#BBDBEC" x="0" y="22" width="6" height="2"></rect>
            <rect fill="#BBDBEC" x="2" y="6" width="2" height="16"></rect>
            <rect fill="#BBDBEC" transform="translate(4.704683, 4.704683) rotate(45) translate(-4.704683, -4.704683) " x="3.70468347" y="1.87968342" width="2" height="5.6500001"></rect>
            <rect fill="#BBDBEC" x="6" y="2" width="62" height="2"></rect>
            <polyline stroke="#BBDBEC" stroke-width="2" transform="translate(66.400000, 3) rotate(270) translate(-66.400000, -3) " points="69.4000001 -1.4000001 66.4000001 7.4000001 63.4000001 -1.4000001"></polyline>
        </g>
        <g transform="translate(26, 289)">
            <polyline stroke="#BBDBEC" stroke-width="2" points="6 30 3 38.8000002 0 30"></polyline>
            <rect fill="#BBDBEC" x="2" y="0" width="2" height="39"></rect>
            <rect fill="#BBDBEC" x="0" y="0" width="6" height="2"></rect>
        </g>
        <g transform="translate(110, 246.500000) scale(1, -1) translate(-190, -246.500000) translate(150, 236)">
            <rect fill="#BBDBEC" x="0" y="0" width="2" height="6"></rect>
            <rect fill="#BBDBEC" x="76" y="6" width="2" height="12"></rect>
            <rect fill="#BBDBEC" transform="translate(75.304690, 4.704683) rotate(-45) translate(-75.304690, -4.704683) " x="74.3046896" y="1.87968342" width="2" height="5.6500001"></rect>
            <rect fill="#BBDBEC" x="2" y="2" width="72" height="2"></rect>
            <polyline stroke="#BBDBEC" stroke-width="2" points="80 12 77 20.8000002 74 12"></polyline>
        </g>
        <g transform="translate(4, 223)">
            <g>
                <use fill-opacity="0.1" fill="#526B78" fill-rule="evenodd" xlink:href="#path-12"></use>
                <rect stroke="#526B78" stroke-width="4" x="-2" y="-2" width="66" height="66" rx="3"></rect>
            </g>
            <text font-family="'Source Sans Pro', sans-serif" font-size="22" font-weight="500" fill="#FFFFFF">
                <tspan x="10.947" y="38">.hbs</tspan>
            </text>
        </g>
        <g transform="translate(32, 177)">
            <polyline stroke="#BBDBEC" stroke-width="2" points="6 30 3 38.8000002 0 30"></polyline>
            <rect fill="#BBDBEC" x="2" y="0" width="2" height="39"></rect>
            <rect fill="#BBDBEC" x="0" y="0" width="6" height="2"></rect>
        </g>
        <g transform="translate(4, 111)">
            <g>
                <use fill-opacity="0.1" fill="#526B78" fill-rule="evenodd" xlink:href="#path-13"></use>
                <rect stroke="#526B78" stroke-width="4" x="-2" y="-2" width="66" height="66" rx="3"></rect>
            </g>
            <text font-family="'Source Sans Pro', sans-serif" font-size="22" font-weight="500" fill="#FFFFFF">
                <tspan x="20" y="38">.js</tspan>
            </text>
        </g>
    </g>
  </svg>

## 1、安装

然后 [在本地安装 webpack](https://www.webpackjs.com/guides/installation#local-installation)，接着安装 [webpack-cli](https://www.webpackjs.com/guides/getting-started/#using-a-configuration)（此工具用于在命令行中运行 webpack）

```bash
# 全局安装
npm install webpack webpack-cli -g
# 局部安装
npm install webpack webpack-cli -D
```

## 2、使用

1）创建文件夹（demo）

2）在文件夹里面创建src和index.html

注：存放js的文件的必须是src（因为这个时候，你还未配置入口文件名，走的是默认），负责他会报错

默认是走: scr—>index.js

![image-20210901000726408](./images/202203260310920.png)

3）scr里面的文件

① 创建一个js文件夹，里面存放 CommonJS的导出 和 ES Module的导出

CommonJS的导出 ：

```javascript
function fun(){
    return "价格：99.99"
}
module.exports = {
    fun
}
```

ES Module的导出

```javascript
export function add(num1, num2) {
    return num1 * num2
}
```

② scr里面在创建一个 导入的js文件（这个js文件的文件名，必须是 *index.js*）

```javascript
import { add } from "./js/es-module";
const { fun } = require("./js/common");

console.log(add(10, 10));
console.log(fun());
```

4）在demo文件夹的终端下运行，运行之后在跟目录下生成一个dist文件，里面有个main.js

```bash
webpack
```

5）在index.html中引入main.js，浏览器就可以识别了

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    
</body>
</html>
<!-- 
    这块也可以写成模块的引入，这样就不需要webpack，但是这个时候，他就需要起个服务,同时 他也只能识别 ES Module，并且导入的时候 .js 不能省略 
    <script src="./src/index.js" type="module"></script>
-->
<script src="./dist/main.js"></script>
```

 ![image-20210901001614501](./images/202203260314373.png)

<font color=red>注：在实际开发中很少使用全局的webpack。</font>

## 3、创建一个局部的webpack

1）在项目的跟目录下：

```bash
npm init
# 下面这种方式，它可以帮助你生成所有的信息
npm init -y
```

注：这个时候，他会提示你 填入一些信息。

2）下载webpack依赖

```bash
npm install webpack webpack-cli -D
```

package.json 他是记录版本信息的，在下次进行依赖下载的时候，会直接去找这个文件，进行对应的下载（也就是缓存）。

3）局部打包

```bash
npx webpack
```

注：下面这样写，可以指定打包入口和出口

```bash
npx webpack --entry 入口文件的相对路径 --output-path 出口文件的相对路径
```

4）在项目中使用webpack打包的时候，可以不用3的方式

在package.json中创建scripts脚本，执行脚本打包即可

 ![aq43t-mxki3](./images/202203260317121.png)

这个时候，你只要执行  `npm run build` 就好

# 二、webpack配置文件

注：配置文件一般叫：webpack.config.js

## 1、指定打包的入口和出口

build.js

```javascript
const path = require("path");
module.exports = {
    // 入口
    entry: "./src/main.js",
    output: {
        /**
         * 出口文件
         * 这块一般要写绝对路径，但是  可以使用node的 path 方法
         * __dirname 可以获取当前的文件位置 ，这样后面 就能写 相对路径（用于文件的出口位置）
         */
        path: path.resolve(__dirname,"./dist"),
        // 出口文件名
        filename:"bundle.js"
    }
}
```

## 2、修改打包的配置文件名，不叫webpack.config.js

1）在package.json里面  进行修改

```json
{
  "name": "webpack-demo",
  "version": "1.0.0",
  "main": "index.js",
  "scripts": {
    "build": "webpack --config build.js"
  },
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "webpack": "^5.52.0",
    "webpack-cli": "^4.8.0"
  },
  "dependencies": {},
  "keywords": [],
  "description": ""
}
```

一般不建议改配置文件名。

 ![aadfv-g39nl](./images/202203260320423.png)

# 三、webpack对CSS资源进行打包

## 1、webpack依赖图

1）把你需要打包的js文件，都要先导入到入口的js文件

2）在src下创建js文件夹

element.js

```javascript
const div = document.createElement("div");
div.className = "box";
div.innerText = "你好！"
document.body.appendChild(div);
```

3）在入口文件中引入(也就是index.js)

```javascript
import "./js/element"
```

4）进行打包，然后在index.html中引入，就可以使用

 ![image preview](./images/202203260323952.jpg)

## 2、对css文件，进行打包

<font color=red>CSS也要引入入口文件，使用import引入，引入方法，有下面两种（引入位置 *一般是在使用的地方*）。</font>

 ![image preview](./images/202203260323611.png)

注：当webopack要编译css的时候，你需要下载webpack编译的依赖，否则他会报以下的这个错误：

![image preview](./images/202203260324005.png)

### 1）下载依赖

```bash
# css-loader只是负责将.css文件进行解析，并不会将解析之后的css插入到页面中
npm install css-loader -D
# 将样式渲染到页面
npm install style-loader -D
```

### 2）使用内联的方式 （这个方式  不经常使用）

注：用！分割，依然遵循 从右向左。

```javascript
import "style-loader!css-loader!css文件的相对路径"
```

### 3）配置的方式

webpack.config.js文件中配置信息：

① module.rules中允许我们配置多个loader（因为我们也会继续使用其他的loader，来完成其他文件的加载）；

② 这种方式可以更好的表示loader的配置，也方便后期的维护，同时也让你对各个Loader有一个全局的概览；

③ rules属性对应的值是一个数组：[Rule]   数组中存放的是一个个的Rule，Rule是一个对象，对象中可以设置多个属性：

a、test 是对应要匹配的文件正则表达式

b、loader当前你要使用的loader，只不过这种是loader写法的语法糖，一般很少使用，都是用的是 c 的写法

c、use:[ Array ]  这个数组里面是你所需要的全部loader，因为 有些时候一个loader是无法完成需要的，

同时这个 Array 是对象组成的，因为你有时候需要给里面传入参数，这个对象的参数如下：

loader：必须有一个 loader属性，对应的值是一个字符串；

options：可选的属性，值是一个字符串或者对象，值会被传入到loader中。

webpack.config.js

```javascript
const path = require("path");

module.exports = {
    // 入口 (这里默认是index.js)
    entry: "./src/main.js",
    output: {
        /**
         * 出口文件
         * 这块一般要写绝对路径，但是  可以使用node的 path 方法
         * __dirname 可以获取当前的文件位置 ，这样后面 就能写 相对路径（用于文件的出口位置）
         */
        path: path.resolve(__dirname, "./dist"),
        // 出口文件名 (这里默认是main.js)
        filename: "main.js"
    },
    // 
    module: {
        // 这样可以设置多个 loader
        rules: [
            {
                // 这个里面是正则表达式，进行匹配的
                test: /\.css$/,
                // 使用那个loader
                // loader 的语法糖  方式1
                // loader: "css-loader"
                /**
                 * 方法2 
                 * use 加载loader的顺序是 从下往上的
                 */
                use: [
                    { loader: "style-loader" }, // 2、使用 （所以style-loader 要写在css-loader的后面 ）
                    // "css-loader"  简便写法
                    { loader: "css-loader" } // 1、让webpack 能加载css
                ]
            }
        ]
    }
}
```

注：当loader加载顺序错误会报以下错误：

![image preview](./images/202203260325916.png)

# 四、处理less文件

## 1、下载

```bash
# 下载less
npm install less --save -D
# less转css
npx lessc less文件的相对路径.less 编译成css的相对路径.css
# 安装less-loader，让webpack可以对less进行处理
npm install less-loader -D
```

## 2、要在打包的js文件里面引入less

 ![image preview](./images/202203260326356.png)

## 3、在webpacjk.config.js里面进行配置

```javascript
const path = require("path");

module.exports = {
    // 入口 (这里默认是index.js)
    entry: "./src/main.js",
    output: {
        /**
         * 出口文件
         * 这块一般要写绝对路径，但是  可以使用node的 path 方法
         * __dirname 可以获取当前的文件位置 ，这样后面 就能写 相对路径（用于文件的出口位置）
         */
        path: path.resolve(__dirname, "./dist"),
        // 出口文件名 (这里默认是main.js)
        filename: "main.js"
    },
    // 
    module: {
        // 这样可以设置多个 loader
        rules: [
            /* css的处理 */
            {
                // 这个里面是正则表达式，进行匹配的
                test: /\.css$/,  // /\.(less|css)$/
                // 使用那个loader
                // loader 的语法糖  方式1
                // loader: "css-loader"
                /**
                 * 方法2 
                 * use 加载loader的顺序是 从下往上的
                 */
                use: [
                    { loader: "style-loader" }, // 2、使用 （所以style-loader 要写在css-loader的后面 ）
                    // "css-loader"  简便写法
                    { loader: "css-loader" } // 1、让webpack 能加载css
                ]
            },
            /* less的处理 */
            {
                test: /\.less$/i, // 加i是忽略大小写
                use:[
                    // 以下三个都要有
                    "style-loader",
                    "css-loader",
                    "less-loader"
                ]
            }
        ]
    }
}
```

注：less和css也可以配置到一起

```javascript
const path = require("path");

module.exports = {
    // 入口 (这里默认是index.js)
    entry: "./src/main.js",
    output: {
        /**
         * 出口文件
         * 这块一般要写绝对路径，但是  可以使用node的 path 方法
         * __dirname 可以获取当前的文件位置 ，这样后面 就能写 相对路径（用于文件的出口位置）
         */
        path: path.resolve(__dirname, "./dist"),
        // 出口文件名 (这里默认是main.js)
        filename: "main.js"
    },
    // 
    module: {
        // 这样可以设置多个 loader
        rules: [
            {
                test: /\.(less|css)$/i, // 这样写  就是 要么匹配less 要么就是css
                use: [
                    // 以下三个都要有
                    "style-loader",
                    "css-loader",
                    "less-loader"
                ]
            }
        ]
    }
}
```

# 五、PostCSS

## 1、PostCSS的介绍

1）PostCSS是一个通过JavaScript来转换样式的工具；

2）这个工具可以帮助我们进行一些CSS的转换和适配，比如自动添加浏览器前缀、css样式的重置。

## 2、安装

```bash
npm install postcss postcss-cli -D
# 自动添加浏览器前缀
npm install autoprefixer -D
```

转化（转化完成后，他在在css属性上加上浏览器前缀）

```bash
npx postcss --use autoprefixer -o 输出的css的相对路径.css 输入的css文件的相对路径.css 
```

## 3、在webpack中的使用自动给浏览器加上前缀的插件

1）安装

```bash
npm install postcss-loader -D
```

2）使用

```javascript
const path = require("path");

module.exports = {
    // 入口 (这里默认是index.js)
    entry: "./src/main.js",
    output: {
        /**
         * 出口文件
         * 这块一般要写绝对路径，但是  可以使用node的 path 方法
         * __dirname 可以获取当前的文件位置 ，这样后面 就能写 相对路径（用于文件的出口位置）
         */
        path: path.resolve(__dirname, "./dist"),
        // 出口文件名 (这里默认是main.js)
        filename: "main.js"
    },
    // 
    module: {
        // 这样可以设置多个 loader
        rules: [
            {
                // 这个里面是正则表达式，进行匹配的
                test: /\.css$/,
                // 使用那个loader
                // loader 的语法糖  方式1
                // loader: "css-loader"
                /**
                 * 方法2 
                 * use 加载loader的顺序是 从下往上的
                 */
                use: [
                    "style-loader", // 2、使用 （所以style-loader 要写在css-loader的后面 ）
                    // "css-loader"  简便写法
                    "css-loader", // 1、让webpack 能加载css

                    // 在用 postcss-loader 的时候，必须写成对象，你要指定他使用了那些 插件
                    {
                        loader: "postcss-loader",
                        options: {
                            postcssOptions: {
                                // 指定你使用的插件
                                plugins:[
                                    // 自动给浏览器加上前缀
                                    require("autoprefixer")
                                ]
                            }
                        }
                    }
                ]
            }
        ]
    }
}
```

3）上面那样使用，在配置文件里面显示 丑陋，可以抽出（单独配置）

① 在项目目录下创建 postcss.config.js

```javascript
module.exports = {
    // 指定你使用的插件
    plugins: [
        // 自动给浏览器加上前缀
        require("autoprefixer")
    ]
}
```

②在 webpack.config.js里面的配置

```javascript
const path = require("path");

module.exports = {
    // 入口 (这里默认是index.js)
    entry: "./src/main.js",
    output: {
        /**
         * 出口文件
         * 这块一般要写绝对路径，但是  可以使用node的 path 方法
         * __dirname 可以获取当前的文件位置 ，这样后面 就能写 相对路径（用于文件的出口位置）
         */
        path: path.resolve(__dirname, "./dist"),
        // 出口文件名 (这里默认是main.js)
        filename: "main.js"
    },
    // 
    module: {
        // 这样可以设置多个 loader
        rules: [
            {
                // 这个里面是正则表达式，进行匹配的
                test: /\.css$/,
                // 使用那个loader
                // loader 的语法糖  方式1
                // loader: "css-loader"
                /**
                 * 方法2 
                 * use 加载loader的顺序是 从下往上的
                 */
                use: [
                    "style-loader", // 2、使用 （所以style-loader 要写在css-loader的后面 ）
                    // "css-loader"  简便写法
                    "css-loader", // 1、让webpack 能加载css

                    // 在用 postcss-loader 的使用，他在外面 已经写了（他会来这里寻找，如果没有找到 就到跟目录下  去找，看你当前 有没有导出一个对象，把这个对象 当作你的配置信息）
                    "postcss-loader",
                ]
            }
        ]
    }
}
```

## 4、postcss-preset-env的使用（预设环境）

注：① 将一些现代的CSS特性，转成大多数浏览器认识的CSS，并且会根据目标浏览器或者运行时环境 添加所需的polyfill；

​        ② 会自动帮助我们添加autoprefixer。

1）安装

```bash
npm install postcss-preset-env -D
```

2）使用

```javascript
module.exports = {
    // 指定你使用的插件
    plugins: [
        // 自动给浏览器加上前缀
        require("postcss-preset-env")
    ]
}
```

webpack.config.js里面的内容不改变。

 ![image preview](./images/202203260327755.png)

# 六、[asset-modules （资源模块类型）](https://webpack.js.org/guides/asset-modules/#root)

webpack4中loader的使用： <https://v4.webpack.js.org/loaders/#root>

## 1、介绍

1）资产模块是一种模块，它允许人们在不配置额外加载器的情况下使用资产文件（字体、图标等）。

在 webpack 5 之前，通常使用：

① [raw-loader](https://v4.webpack.js.org/loaders/raw-loader/) 将文件作为字符串导入

② [url-loader](https://v4.webpack.js.org/loaders/url-loader/) 将文件作为数据 URI 内联到包中

③ [file-loader](https://v4.webpack.js.org/loaders/file-loader/) 将文件发送到输出目录

2）资产模块类型通过添加 4 种新模块类型来替换所有这些加载器（<font color=red>webpack5+ 以上才可以</font>）：

① asset/resource 发出一个单独的文件并导出 URL，以前可以通过使用 file-loader

② asset/inline 导出资产的数据 URI（也就是base64），以前可以通过使用 url-loader

③ asset/source 导出资产的源代码，以前可以通过使用 raw-loader

④ asset 自动在导出数据 URI 和发出单独的文件之间进行选择，以前可以通过使用 url-loader 资产大小限制(limt)来实现。

## 2、加载图片

注：图片过小的时候，变成base64

webpack.config.js:

```javascript
module.exports = {
    /* 在这不定义输入输出，那么 他就是  走的默认 */
    module: {
        rules: [
            // 处理图片，webpack5+ 已经不需要可
            {
                test: /\.(jpe?g|png|gif|svg)$/,
                type: "asset",
                generator: {
                    /**
                     * webpack5+ 的扩展名 包含点，不用写点 （filename 也可以写在 ouput中，写法如下： assetModuleFilename:'img/[name]_[hash:6][ext]'）
                     * img 存放的文件名
                     * [name] 图片原来的名字
                     * _ 分隔线，可自主定义
                     * [hash:6] 哈希模式，保持前六位
                     * [ext] 图片的扩展名
                     */
                    filename: "img/[name]_[hash:6][ext]",
                },
                // 写限制
                parser: {
                    // 数据的条件
                    dataUrlCondition: {
                        // 这块的单位 b
                        maxSize: 100 * 1024,
                    }
                }
            }
        ]
    }
}
```

## 3、加载字体

webpack.config.js:

```javascript
module.exports = {
    /* 在这不定义输入输出，那么 他就是  走的默认 */
    module: {
        rules: [
            // 处理特殊字体，包括阿里的矢量图标
            {
                test: /\.(eot|ttf|woft2?)$/,
                type: "asset/resource",
                generator: {
                    /**
                     * webpack5+ 的扩展名 包含点，不用写点 （filename 也可以写在 ouput中，写法如下： assetModuleFilename:'font/[name]_[hash:8][ext]'）
                     * img 存放的文件名
                     * [name] 图片原来的名字
                     * - 分隔线，可自主定义
                     * [hash:6] 哈希模式，保持前六位
                     * [ext] 字体的扩展名
                     */
                    filename: "font/[name]-[hash:8][ext]"
                }
            }
        ]
    }
}
```

# 七、[插件(plugins)](https://www.webpackjs.com/concepts/plugins/)

插件是 webpack 的[支柱](https://github.com/webpack/tapable)功能。webpack 自身也是构建于，你在 webpack 配置中用到的相同的插件系统之上，插件目的在于解决 [loader](https://www.webpackjs.com/concepts/loaders) 无法实现的其他事，例：

① Loader是用于特定的模块类型进行转换；

② Plugin可以用于执行更加广泛的任务，比如打包优化、资源管理、环境变量注入等;

③ 插件执行没有顺序。

## 1、clean-webpack-plugin的使用

注：这个插件可以主动删除打包的文件

### 1）安装

```bash
npm install clean-webpack-plugin -D
```

### 2）导入webpack

在webpack.config.js中：

```javascript
const path = require("path");
// CleanWebpackPlugin是一个类
const { CleanWebpackPlugin } = require("clean-webpack-plugin");
module.exports = {
    output: {
        path: path.resolve(__dirname, "./build"),
        filename: "main.js"
    },
    //这个里面是一个个的插件对象 (plugins 与 module 同级)
    plugins: [
        /**
         * CleanWebpackPlugin 他会自动读取上下文中的output中的path所对应的路径，并且去删除
         * 注：这个必须设置output 这个，负责感觉 并不会删除默认值
         */ 
        new CleanWebpackPlugin()
    ]
}
```

## 2、[HtmlWebpackPlugin](https://www.webpackjs.com/plugins/html-webpack-plugin/)

注：生成打包中的入口html。

html-webpack-plugin 官方文档：<https://www.webpackjs.com/plugins/html-webpack-plugin/>

### 1）安装

```bash
npm install html-webpack-plugin -D
```

### 2）使用

在webpack.config.js中导入：

```javascript
const path = require("path");
// CleanWebpackPlugin是一个类
const { CleanWebpackPlugin } = require("clean-webpack-plugin");
const HtmlWebpackPlugin = require("html-webpack-plugin")
module.exports = {
    output: {
        path: path.resolve(__dirname, "./build"),
        filename: "js/main.js" // 指定js的打包目录
    },
    //这个里面是一个个的插件对象
    plugins: [
        /**
         * CleanWebpackPlugin 他会自动读取上下文中的output中的path所对应的路径，并且去删除
         * 注：这个必须设置output 这个，负责感觉 并不会删除默认值
         */ 
        new CleanWebpackPlugin(),
        
        new HtmlWebpackPlugin()
    ]
}
```

### 3）指定html打包模板

① 首先创建一个html文件作为模板

index.html

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta http-equiv="X-UA-Compatible" content="IE=edge">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>指定html打包模板</title>
    </head>
    <body>
    </body>
</html>
```

② webpack.config.js中的配置：

```javascript
const path = require("path");
// CleanWebpackPlugin是一个类
const { CleanWebpackPlugin } = require("clean-webpack-plugin");
const HtmlWebpackPlugin = require("html-webpack-plugin");
/**
 * 给 BASE_URL 里面填充数据，也就是 填充页面的小图标
 */
module.exports = {
    output: {
        path: path.resolve(__dirname, "./build"),
        filename: "js/main.js" // 指定js的打包目录
    },
    //这个里面是一个个的插件对象
    plugins: [
        /**
         * CleanWebpackPlugin 他会自动读取上下文中的output中的path所对应的路径，并且去删除
         * 注：这个必须设置output 这个，感觉 默认值并不会删除
         */ 
        new CleanWebpackPlugin(),
        // 指定html打包模板
        new HtmlWebpackPlugin({
            // 这个是指定tiele
            title: "指定html打包模板",
            // 传入指定模板路径
            template: "./index.html",
        })
    ]
}
```

## 3、DefinePlugin的介绍

<font color=red>注：</font> ① DefinePlugin是和 HtmlWebpackPlugin 配合使用的；

​         ② 有时候模块中会有一个BASE_URL的常量，但是 因为我们并没有设置过该值的时候，就会报以下的错误：![image preview](./images/202203260328390.png)

​          所以这个时候，就要使用DefinePlugin插件。

​         ③ 文档：<https://www.webpackjs.com/plugins/define-plugin/>

### 1）使用

DefinePlugin是一个webpack内置的插件（不需要单独安装）。

index.html

```html
<!DOCTYPE html>
<html lang="">
    <head>
        <meta charset="utf-8">
        <meta http-equiv="X-UA-Compatible" content="IE=edge">
        <meta name="viewport" content="width=device-width,initial-scale=1.0">
        <link rel="icon" href="<%= BASE_URL %>favicon.ico">
        <title>
            <%= htmlWebpackPlugin.options.title %>
        </title>
    </head>

    <body>
        <noscript>
            <strong>We're sorry but <%= htmlWebpackPlugin.options.title %> doesn't work properly without JavaScript enabled.
                    Please enable it to continue.</strong>
        </noscript>
        <div id="app"></div>
    </body>
</html>
```

### 2）定义这个变量

```javascript
const path = require("path");
// CleanWebpackPlugin是一个类
const { CleanWebpackPlugin } = require("clean-webpack-plugin");
// 读取自定义的html模板
const HtmlWebpackPlugin = require("html-webpack-plugin");
/**
 * 给 BASE_URL 里面填充数据，也就是 填充页面的小图标
 * 设置模板里面的常量
 */
const { DefinePlugin } = require("webpack")

module.exports = {
    output: {
        path: path.resolve(__dirname, "./build"),
        filename: "js/main.js" // 指定js的打包目录
    },
    //这个里面是一个个的插件对象
    plugins: [
        /**
         * CleanWebpackPlugin 他会自动读取上下文中的output中的path所对应的路径，并且去删除
         * 注：这个必须设置output 这个，负责感觉 并不会删除默认值
         */
        new CleanWebpackPlugin(),
        // 指定html打包模板
        new HtmlWebpackPlugin({
            // 这个是指定tiele
            title: "指定html打包模板",
            // 传入指定模板路径
            template: "./index.html",
        }),
        new DefinePlugin({
            // 定义值，还需要 在双引号里面加单引号（也就是 路径，和html中<%= BASE_URL %>后面的值，拼接成一个完整的路径）
            BASE_URL: " './' "
        })
    ]
}
```

## 4、CopyWebpackPlugin（将指定文件复制到打包文件下）

### 1）安装

```bash
npm install copy-webpack-plugin -D
```

### 2）使用

webpack.config.js

```javascript
const path = require("path");
// CleanWebpackPlugin是一个类
const { CleanWebpackPlugin } = require("clean-webpack-plugin");
// 读取自定义的html模板
const HtmlWebpackPlugin = require("html-webpack-plugin");
// 设置模板里面的常量
const { DefinePlugin } = require("webpack");
const CopyWebpackPlugin = require("copy-webpack-plugin")
/**
 * 给 BASE_URL 里面填充数据，也就是 填充页面的小图标
 */
module.exports = {
    output: {
        path: path.resolve(__dirname, "./build"),
        filename: "js/main.js" // 指定js的打包目录
    },
    /* 在这不定义输入输出，那么 他就是  走的默认 */
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
            // 处理图片，webpack5+ 已经不需要可
            {
                test: /\.(jpe?g|png|gif|svg)$/,
                type: "asset",
                generator: {
                    /**
                     * webpack5+ 的扩展名 包含点，不用写点 （filename 也可以写在 ouput中，写法如下： assetModuleFilename:'img/[name]_[hash:6][ext]'）
                     * img 存放的文件名
                     * [name] 图片原来的名字
                     * _ 分隔线，可自主定义
                     * [hash:6] 哈希模式，保持前六位
                     * [ext] 图片的扩展名
                     */
                    filename: "img/[name]_[hash:6][ext]",
                },
                // 写限制
                parser: {
                    // 数据的条件
                    dataUrlCondition: {
                        // 这块的单位 b
                        maxSize: 100 * 1024,
                    }
                }
            },
            // 处理特殊字体，包括阿里的矢量图标
            {
                test: /\.(eot|ttf|woff2?)$/,
                type: "asset/resource",
                generator: {
                    /**
                     * webpack5+ 的扩展名 包含点，不用写点 （filename 也可以写在 ouput中，写法如下： assetModuleFilename:'font/[name]_[hash:8][ext]'）
                     * img 存放的文件名
                     * [name] 图片原来的名字
                     * _ 分隔线，可自主定义
                     * [hash:6] 哈希模式，保持前六位
                     * [ext] 字体的扩展名
                     */
                    filename: "font/[name]-[hash:8][ext]"
                }
            }
        ]
    },
    //这个里面是一个个的插件对象
    plugins: [
        /**
         * CleanWebpackPlugin 他会自动读取上下文中的output中的path所对应的路径，并且去删除
         * 注：这个必须设置output 这个，负责感觉 并不会删除默认值
         */
        new CleanWebpackPlugin(),
        // 指定html打包模板
        new HtmlWebpackPlugin({
            // 这个是指定tiele
            title: "指定html打包模板",
            // 传入指定模板路径
            template: "./public/index.html",
        }),
        new DefinePlugin({
            // 定义值，还需要 在双引号里面加单引号
            BASE_URL: " './' "
        }),
        // 拷贝文件到打包文件夹下
        new CopyWebpackPlugin({
            /**
             * 复制
             * 1、里面可以复制多个，用json对象
             * 2、里面的规则：
             * {
                    from: "你要复制的文件夹（☞的是文件夹里面的内容，包括子文件夹）",
                    to: "./", // 他会自己去找 output 的出口 ，但是这个里面 要先定位
                    // 忽略你要复制的文件夹下的某一个文件
                    globOptions: {
                        // 你要忽略的文件名
                        ignore: [
                            // 忽略所有的index.html，包括子目录下的 所以给前面加 **
                            "星星/要忽略的文件名"
                        ]
                    }
                }
             */
            patterns: [
                {
                    from: "public",
                    to: "./", //  ./abc  这样他会在打包的目录下 在创建一个abc文件夹
                    // 忽略你要复制的文件夹下的某一个文件
                    globOptions: {
                        // 你要忽略的文件名
                        ignore: [
                            // 忽略所有的index.html，包括子目录下的
                            "**/index.html"
                        ]
                    }
                }
            ]
        })
    ]
}
```

注：可以使用 code-splitting  进行代码分割。

# 八、Babel的使用

注：Babel官网：<https://babeljs.io/>

​     <https://github.com/jamiebuilds/the-super-tiny-compiler>

## 1、Babel命令行使用

注：babel本身可以作为一个独立的工具（和postcss一样），不和webpack等构建工具配置来单独使用。

1）在命令行尝试使用babel，需要安装如下库：

@babel/core：babel的核心代码，必须安装；

@babel/cli：可以让我们在命令行使用babel （如果在webpack中使用，就不需要安装他了）

```bash
npm install @babel/cli @babel/core -D
```

2）运行

```bash
npx babel 要转义的文件名.js/或者文件夹（这块是相对路径） --out-dir 转移之后的文件位置
# 或者指定你要转义的文件，并且指定转义生成的文件名
npx babel demo.js --out-file dist.js
```

3）但是以上的操作，他并没有对文件进行转换，因为 转换的话，就需要去下载相应的插件 才可以（*也就是转什么的功能 就需要安装什么插件*）,下载插件之后的运行：

```bash
npx babel src --out-dir dist --plugins=@babel/plugin-transform-arrow-functions,插件名（多个插件用逗号隔开）
```

4）箭头函数的转换

① 用箭头函数转换相关的插件

```bash
npm install @babel/plugin-transform-arrow-functions -D
```

② 使用安装的这个插件

```bash
npx babel src --out-dir dist --plugins=@babel/plugin-transform-arrow-functions
```

5） const 成 var的转换

① 安装

```bash
npm install @babel/plugin-transform-block-scoping -D
```

② 使用（这块同时使用箭头函数的转换、const 转成 var）

```bash
npx babel demo.js --out-file dist.js  --plugins=@babel/plugin-transform-arrow-functions,@babel/plugin-transform-block-scoping
```

6）preset

注：像上面一样一个个依赖安装使用，过于麻烦，我们可以使用preset（预设）

① 安装

```bash
npm install @babel/preset-env -D
```

② 使用

```bash
npx babel src --out-dir dist --presets=@babel/preset-env
```

<font color=red>注：</font> 转换到文件夹和文件的区别：

```bash
# 这个是把src目录下的js转换到dist文件夹下
npx babel src --out-dir dist --presets=@babel/preset-env
# 下面这个是把demo.js 转换到 dist.js
npx babel demo.js --out-file dist.js --presets=@babel/preset-env
```

7） Babel的执行阶段

```mermaid
graph LR
A(源代码 自己写的) -->B(解析)
B --> C(转换)
C --> D(代码生成)
D --> E(目标源码)
```

 ![image preview](./images/202203260330027.png)

## 2、Babel在webpack中的使用（babel-loader）

1）安装

```bash
npm install @babel/cli -D
npm install babel-loader -D
```

2）使用

```javascript
const path = require("path");
module.exports = {
    // 设置模式
    // development 开发阶段, 会设置development
    // production 准备打包上线的时候, 设置production
    mode: "development",
    // 设置source-map, 建立js映射文件, 方便调试代码和错误
    devtool: "source-map",
    entry: "./src/index.js",
    output: {
        path: path.resolve(__dirname, "./build"),
        filename: "js/main.js" // 指定js的打包目录
    },
    /* 在这不定义输入输出，那么 他就是  走的默认 */
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
            // 处理图片，webpack5+ 已经不需要可
            {
                test: /\.(jpe?g|png|gif|svg)$/,
                type: "asset",
                generator: {
                    /**
                     * webpack5+ 的扩展名 包含点，不用写点 （filename 也可以写在 ouput中，写法如下： assetModuleFilename:'img/[name]_[hash:6][ext]'）
                     * img 存放的文件名
                     * [name] 图片原来的名字
                     * _ 分隔线，可自主定义
                     * [hash:6] 哈希模式，保持前六位
                     * [ext] 图片的扩展名
                     */
                    filename: "img/[name]_[hash:6][ext]",
                },
                // 写限制
                parser: {
                    // 数据的条件
                    dataUrlCondition: {
                        // 这块的单位 b
                        maxSize: 10 * 1024,
                    }
                }
            },
            // 处理特殊字体，包括阿里的矢量图标
            {
                test: /\.(eot|ttf|woff2?)$/,
                type: "asset/resource",
                generator: {
                    /**
                     * webpack5+ 的扩展名 包含点，不用写点 （filename 也可以写在 ouput中，写法如下： assetModuleFilename:'font/[name]_[hash:8][ext]'）
                     * img 存放的文件名
                     * [name] 图片原来的名字
                     * _ 分隔线，可自主定义
                     * [hash:6] 哈希模式，保持前六位
                     * [ext] 字体的扩展名
                     */
                    filename: "font/[name]-[hash:8][ext]"
                }
            },
            {
                test: /\.js$/,
                use: {
                    loader: "babel-loader",
                    options: {
                        /**
                         * 这样是不同插件的配置（一般情况下不这样使用）
                         * 使用下面这两个插件前，你要先下载：
                         * npm install @babel/plugin-transform-arrow-functions -D
                         * npm install @babel/plugin-transform-block-scoping -D
                        plugins: [
                            "@babel/plugin-transform-arrow-functions",
                            "@babel/plugin-transform-block-scoping"
                        ]
                        */
                        presets: [
                            /**
                             * 这个里面可以写多个
                             * 给里面每个配置参数的写法如下：
                               ["插件名",{}]
                             */
                            "@babel/preset-env"
                        ]
                    }
                }
            }
        ]
    }
}
```

3）把babel-loader的配置抽出

① 在跟目录下，创建如下文件

babel.config.js（或者.cjs，.mjs）文件；

.babelrc.js（或者.babelrc，.cjs，.mjs）文件；

以上二选一

babel.config.js

```javascript
module.exports = {
    // 指定你使用的插件
    plugins: [
        // 自动给浏览器加上前缀
        require("postcss-preset-env")
    ]
}
```

② webpack.config.js中的写法:

![image preview](./images/202203260331423.png)

# 九、webpack自动编译

注：当文件发生修改的时候，自动进行编译，不用修改以下，就打包一次。

## 1、Webpack watch

在该模式下，webpack依赖图中的所有文件，只要有一个发生了更新，那么代码将被重新编译。

### 1）如何开启watch呢？两种方式

方式一：在导出的配置中，添加 watch: true；

方式二：在启动webpack的命令中，添加 --watch的标识；

### 2）在启动webpack的命令中，添加 --watch的标识

① 在package.json里面进行配置

```json
  "scripts": {
    "build": "webpack",
    "watch": "webpack --watch"
  },
```

注：他会根据webpack cli进行编译的。

② 运行：

```bash
npm run watch
```

③ 打开

在生成的打包文件中，使用Live Server打开生成的index.html文件。

### 3）在webpak.config.js中添加配置选项

```javascript
const path = require("path");
const { CleanWebpackPlugin } = require("clean-webpack-plugin");
const HtmlWebpackPlugin = require("html-webpack-plugin");
const { DefinePlugin } = require("webpack");
const CopyWebpackPlugin = require("copy-webpack-plugin")
module.exports = {
    mode: "development",
    devtool: "source-map",
    // 在这打开监听（使用他的时候，你就可以删除package.json里面的配置）
    watch: true,
    entry: "./src/index.js",
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

运行：

```bash
# 这个build的指令实在package.json中配置的，具体的配置 如下：
# "scripts": {
#   "build": "webpack"
# },
npm run build
```

## 2、webpack-dev-server

注：使用webpack-dev-server的使用可以不适用vsc中的live-server。

### 1）安装

```bash
npm install webpack-dev-server -D
```

### 2）在package.json中配置

webpack serve后面可以跟 配置文件地址，写法：webpack serve --config 配置文件的行对路径

```json
  "scripts": {
    "build": "webpack",
    "serve": "webpack serve"
  },
```

### 3）运行

```bash
npm run serve
```

 ![image preview](./images/202203260332729.png)

这个时候的打包生成的文件夹里面是空的，他会放在内存中。

## 3、devServer的配置（也就是webpack-dev-server的配置）

### 1） 模块热替换

① webpack.config.js的配置

```javascript
const path = require("path");
const { CleanWebpackPlugin } = require("clean-webpack-plugin");
const HtmlWebpackPlugin = require("html-webpack-plugin");
const { DefinePlugin } = require("webpack");
const CopyWebpackPlugin = require("copy-webpack-plugin")
module.exports = {
    /**
     * webpack 打包为什么打包的
     * target值内容：web、node
     */
    target:"web",
    mode: "development",
    devtool: "source-map",
    entry: "./src/index.js",
    output: {
        path: path.resolve(__dirname, "./build"),
        filename: "js/main.js" // 指定js的打包目录
    },
    devServer:{
        /**
         * 某些资源在webpak打包中加载不到的资源，就可以在contentBase中去指定加载的文件夹地址中去查找
         * 配置这个的时候 webpack-dev-server 使用  3.11.2 的版本，没有问题，使用最近的会报错，我没解决
         * CopyWebpackPlugin 没有这个的时候，使用上面的 就可以加载到，因为我没有拷贝，所以 就加载不到了（在开发过程中，使用CopyWebpackPlugin 他会浪费资源，所以就可以 使用contentBase，所以 在开发过程中 就可以注销掉下面的CopyWebpackPlugin配置，但是在生产资源时，依然要拷贝的）
         */
        contentBase: "./public",
        /**
         * 模块的热替换：
         * 模块热替换是指在 应用程序运行过程中，替换、添加、删除模块，而无需重新刷新整个页面
         * 比如：你在使用计算器的时候，你不想修改代码之后，计算器的结果也丢失的情况下 使用
         * 
         */
        hot: true,
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
        // 在开发的时候，节省性能  就可以注销
        // new CopyWebpackPlugin({
        //     patterns: [
        //         {
        //             from: "public",
        //             to: "./",
        //             globOptions: {
        //                 ignore: [
        //                     "**/index.html"
        //                 ]
        //             }
        //         }
        //     ]
        // })
    ]
}
```

② 引入需要热替换模块的写法

```javascript
/**
 * 如果使用对每个模块的热替换，这个时候 你就不能使用，以前的写法了
 * 在webpack中一个js文件，就是一个模块
 * 要替换成下面的写法：
 */
import "./js/element"
if(module.hot){
    module.hot.accept("./js/element.js", () => {
        console.log("模块的热替换");
    })
    ... 类似与上面的写法，去指定多个模块
}
```

③ 执行的效果：

 ![image preview](./images/202203260333605.png)

### 2）host配置

①localhost 和 0.0.0.0 的区别

a、localhost：本质上是一个域名，通常情况下会被解析成127.0.0.1;

b、127.0.0.1：回环地址(Loop Back Address)，表达的意思其实是我们主机自己发出去的包，直接被自己接收;

c、正常的数据库包经过：

```mermaid
graph LR
A(应用层) -->B(传输层)
B --> C(网络层)
C --> D(数据链路层)
D --> E(物理层 )
```

d、回环地址，是在网络层直接就被获取到了，是不会经常数据链路层和物理层的。

e、127.0.0.1时，在同一个网段下的主机中，通过ip地址是不能访问的；

f、们监听 0.0.0.0时，在同一个网段下的主机中，通过ip地址是可以访问的。

② host设置主机地址：

默认值是localhost；

如果希望其他地方也可以访问，可以设置为 0.0.0.0。

### 3）port设置监听的端口，默认情况下是8080

![image preview](./images/202203260333587.png)

### 4）compress是否为静态文件开启gzip

注：他对html没有压缩，同时这个开启不开启无所谓，因为他是本地传输。

当你设置了这个之后，浏览器每次的请求资源就会变成，下面的这个样子：

 ![image preview](./images/202203260335635.png)

```javascript
    devServer:{
        /**
         * 是否为静态文件开启gzip 
         * 默认值是 false
         */
        compress: true
    },
```

 ![image preview](./images/202203260336403.png)

## 4、跨域代理（Proxy）

```javascript
devServer: {
    proxy: {
        // /api 是代理的地址
        "/api": {
            // 下面是服务器的地址
            target: "http://localhost:8888",
                /**
                 * "^/api": "这个里面写的内容会被匹配到url里面"
                 * ^/api 是匹配 /api 后面的内容
                 */
                pathRewrite: {
                    "^/api": ""
                },
                    /**
                 * secure：默认情况下不接收转发到https的服务器上且证书无效，如果希望支持，可以设置为false
                 */
                    secure: false,
                        /**
                 * 修改源：一般为true
                 */
                        changeOrigin: true
        }
    }
},
```

## 5、resolve模块解析

### 1）resolve用于设置模块如何被解析

在开发中我们会有各种各样的模块依赖，这些模块可能来自于自己编写的代码，也可能来自第三方库；

resolve可以帮助webpack从每个 require/import 语句中，找到需要引入到合适的模块代码；

webpack 使用 enhanced-resolve 来解析文件路径

### 2）webpack能解析三种文件路径

① 绝对路径

由于已经获得文件的绝对路径，因此不需要再做进一步解析。

② 相对路径

在这种情况下，使用 import 或 require 的资源文件所处的目录，被认为是上下文目录，在 import/require 中给定的相对路径，会拼接此上下文路径，来生成模块的绝对路径。

③ 模块路径

在 resolve.modules中指定的所有目录检索模块， 默认值是 ['node_modules']，所以默认会从node_modules中查找文件。

![image preview](./images/202203260336299.png)

### 3）使用

```javascript
const path = require("path");
const { CleanWebpackPlugin } = require("clean-webpack-plugin");
const HtmlWebpackPlugin = require("html-webpack-plugin");
const { DefinePlugin } = require("webpack");
const CopyWebpackPlugin = require("copy-webpack-plugin")
module.exports = {
    /**
     * webpack 打包为什么打包的
     * target值内容：web、node
     */
    target: "web",
    mode: "development",
    devtool: "source-map",
    entry: "./src/index.js",
    output: {
        path: path.resolve(__dirname, "./build"),
        filename: "js/main.js" // 指定js的打包目录
    },
    devServer: {
        /**
         * 某些资源在webpak打包中加载不到的资源，就可以在contentBase中去指定加载的文件夹地址中去查找
         * 配置这个的时候 webpack-dev-server 使用  3.11.2 的版本，没有问题，使用最近的会报错，我没解决
         * CopyWebpackPlugin 没有这个的时候，使用上面的 就可以加载到，因为我没有拷贝，所以 就加载不到了（在开发过程中，使用CopyWebpackPlugin 他会浪费资源，所以就可以 使用contentBase，所以 在开发过程中 就可以注销掉下面的CopyWebpackPlugin配置，但是在生产资源时，依然要拷贝的）
         */
        contentBase: "./public",
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
        /**
         * 跨域
         */
        proxy: {
            // /api 是代理的地址
            "/api": {
                // 下面是服务器的地址
                target: "http://localhost:8888",
                /**
                 * "^/api": "这个里面写的内容会被匹配到url里面"
                 * ^/api 是匹配 /api 后面的内容
                 */
                pathRewrite: {
                    "^/api": ""
                },
                /**
                 * secure：默认情况下不接收转发到https的服务器上且证书无效，如果希望支持，可以设置为false
                 */
                secure: false,
                /**
                 * 修改源：一般为true
                 */
                changeOrigin: true
            }
        }
    },
    resolve: {
        /**
         * 如果是一个文件:
         * 他会根据extensions里面写的自动给路径后面匹配，去文件夹中查找相应的文件
         * extensions 里面的默认值有：[".js", ".json", ".mjs", ".wasm"]
         * 可以给下面加入 .vue .ts .jsx
         */
        extensions: [".js", ".json", ".mjs", ".wasm"],
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
            "js": path.resolve(__dirname, "./src/js")
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
        // 在开发的时候，节省性能  就可以注销
        // new CopyWebpackPlugin({
        //     patterns: [
        //         {
        //             from: "public",
        //             to: "./",
        //             globOptions: {
        //                 ignore: [
        //                     "**/index.html"
        //                 ]
        //             }
        //         }
        //     ]
        // })
    ]
}
```

全部代码地址：<https://www.lanzouw.com/iaDu3uba7le>
