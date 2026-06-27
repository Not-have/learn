## 1、判断json对象中 是否包含某个键

```javascript
let obj = { a: 1, b: 2, c: 3 };
// 判断这个 Object里面是否有这个 字段
console.log(Object.prototype.hasOwnProperty.call(obj, 'a'));//true
```

## 2、事件防抖

<font color=Turquoise1>注：</font>

①本来事件的触发比较频繁,但是,我们只希望这无数次的事件触发中,有部分事件是有效的（如：用户有短暂的停止时才调用函数）。特别是在触发一次，就发一次请求，会有无数次的抖动。

如: 键盘事件：onkeydown,onkeyup,onkeypress,oninput，都是按一次键,触发一次.触发非常频繁。

②使用场景:

搜索框(百度搜索框，淘宝，京东等等),每次用户输入内容都需要发一次请求,从后端拿到关键字对应的内容.

特别是输入汉字时:

我们希望用户输入完成汉字时再触发(发送请求)，

但是,实际情况是:当用户输入一个字母时,就会触发一次事件(发送一次请求)

```javascript
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <title></title>
    <style type="text/css">
        * {
            margin: 0;
            padding: 0;
        }
        ul {
            list-style: none;
        }
    </style>
</head>
<body>
    <input type="text" id="querystr">
    <input type="submit" value="百度一下">
    <ul id="search-list">
        <li>啊啊啊</li>
        <li>哈哈哈</li>
        <li>呵呵呵</li>
    </ul>
</body>
</html>
<script type="text/javascript">
    let myTimer = null;

    $("querystr").oninput = function () {
        //在每次输入时，先清除上一次的定时器，清除了定时器后，就不会发送请求了。
        //即本次输入和上次输入的间隔非常短（小于200ms）时，上次定时器的代码就不会执行，也就不会发请求了。
        //如果本次输入和上次输入之间的时间间隔大于100ms时，上次输入的内容就会发送请求。
        if (myTimer != null) {
            window.clearTimeout(myTimer);
        }
        myTimer = setTimeout(() => {
            let scriptDom = document.createElement("script");
            scriptDom.src = 'https://sp0.baidu.com/5a1Fazu8AA54nxGko9WTAnF6hhy/su?wd=' + this.value + '&json=1&p=3&sid=1438_24869_21080_18560_17001_25177_22160&req=2&bs=1%2B&pbs=1%2B&csor=2&pwd=1%3D&cb=f&_=1511334117083';
            document.body.appendChild(scriptDom);
            scriptDom.remove();
        }, 200);
    }

    function f(data) {
        console.log(data);
        let htmlStr = "";
        data.s.forEach((item) => {
            htmlStr += `<li>${item}</li>`;
        });
        $("search-list").innerHTML = htmlStr;
    }

    function $(id) {
        return document.getElementById(id);
    }
</script>
```

## 3、封装$符获取元素

```javascript
/**
 * 元素尺寸属性 说明
 * clientWidth 获取元素可视部分的宽度，即 CSS 的 width 和 padding 属性值之和，元素边框和滚动条不包括在内，也不包含任何可能的滚动区域
 * clientHeight 获取元素可视部分的高度，即 CSS 的 height 和 padding 属性值之和，元素边框和滚动条不包括在内，也不包含任何可能的滚动区域
 * offsetWidth 元素在页面中占据的宽度总和，包括 width、padding、border 以及滚动条的宽度
 * offsetHeight 元素在页面中占据的高度总和，包括 height、padding、border 以及滚动条的宽度
 * scrollWidth 当元素设置了 overflow:visible 样式属性时，元素的总宽度，也称滚动宽度。在默认状态下，如果该属性值大于 clientWidth 属性值，则元素会显示滚动条，以便能够翻阅被隐藏的区域
 * scrollHeight 当元素设置了 overflow:visible 样式属性时，元素的总高度，也称滚动高度。在默认状态下，如果该属性值大于 clientWidth 属性值，则元素会显示滚动条，以便能够翻阅被隐藏的区域
 */
/**
 * @description: 根据选择器获取dom
 * @param {*} str 选择器（必须加选择器类型，如id加 #XXX）
 * @return {*} 返回dom元素
 */
export function $(str) {
    if (str.charAt(0) == "#") {
        return document.getElementById(str.substring(1, str.length));
    } else if (str.charAt(0) == ".") {
        return document.getElementsByClassName(str.substring(1, str.length))[0];
    } else {
        return document.getElementsByTagName(str)[0];
    }
}

/*
 *                        _oo0oo_
 *                       o8888888o
 *                       88" . "88
 *                       (| -_- |)
 *                       0\  =  /0
 *                     ___/`---'\___
 *                   .' \\|     |// '.
 *                  / \\|||  :  |||// \
 *                 / _||||| -:- |||||- \
 *                |   | \\\  - /// |   |
 *                | \_|  ''\---/''  |_/ |
 *                \  .-\__  '-'  ___/-. /
 *              ___'. .'  /--.--\  `. .'___
 *           ."" '<  `.___\_<|>_/___.' >' "".
 *          | | :  `- \`.;`\ _ /`;.`/ - ` : | |
 *          \  \ `_.   \_ __\ /__ _/   .-` /  /
 *      =====`-.____`.___ \_____/___.-`___.-'=====
 *                        `=---='
 * 
 */
```

## 4、正则获取文件名后缀

```javascript
let url = "XXXXXXXX/1-switch.svg";
console.log(url.replace(/(.*\/)*([^.]+).*/ig,"$2"));//1-switch
```

## 5、随机生成含特殊字符、数字、字母的密码

```javascript
/**
 * 随机密码生成
 * @param length 随机密码 的长度
 * @returns {string} 返回随机生成的密码
 *  /^.*(?=.{10,})(?=.*\d)(?=.*[a-zA-Z])(?=.*[!@#$%^&*?+()]).*$/  这个是验证随机生成的密码   包含数字、字母、特殊字符  至少10位
 *  参考博客：
 *  https://blog.csdn.net/kadxls/article/details/90241948
 *
 */
export default function randomPassword(length) {
    length = Number(length);
    let passwordArray = ['ABCDEFGHIJKLMNOPQRSTUVWXYZ', 'abcdefghijklmnopqrstuvwxyz', '1234567890', '!@#$%&*()+'];
    let password = [];
    let n = 0;
    for (let i = 0; i < length; i++) {
        if (password.length < (length - 4)) {
            let arrayRandom = Math.floor(Math.random() * 4);
            let passwordItem = passwordArray[arrayRandom];
            let item = passwordItem[Math.floor(Math.random() * passwordItem.length)];
            password.push(item);
        } else {
            let newItem = passwordArray[n];
            let lastItem = newItem[Math.floor(Math.random() * newItem.length)];
            let spliceIndex = Math.floor(Math.random() * password.length);
            password.splice(spliceIndex, 0, lastItem);
            n++;
        }
    }
    return password.join("");
}
```

## 6、计算数组里面相同元素的个数

```javascript
    var arr = ["apple", "orange", "apple", "orange", "pear", "orange"];

    function getWordCnt() {
        var obj = {};
        for (var i = 0, l = arr.length; i < l; i++) {
            var item = arr[i];
            obj[item] = (obj[item] + 1) || 1;
        }
        return obj;
    }
    console.log(getWordCnt());
```

## 7、文件流下载

```javascript
/**
 * @description: 文件流改成文件
 * @param {*} obj 文件流
 * @param {*} name 文件名 
 * @param {*} suffix 后缀
 * 调用浏览器的下载
 */
export function downloadFile(obj, name, suffix) {
    const url = window.URL.createObjectURL(new Blob([obj]))
    const link = document.createElement('a')
    link.style.display = 'none'
    link.href = url
    const fileName = parseTime(new Date()) + '-' + name + '.' + suffix
    link.setAttribute('download', fileName)
    document.body.appendChild(link)
    link.click()
    document.body.removeChild(link)
}
```

## 8、复制文本到粘贴板

```javascript
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <link rel="stylesheet" href="https://not-have.github.io/resources/CSS/input-one.css">
    <link rel="stylesheet" href="https://not-have.github.io/resources/CSS/button-one.css">
    <style>
        body {
            padding: 20px;
        }
    </style>
</head>
<body>
    <div class="group">
        <input type="text" required id="inputText">
        <span class="highlight"></span>
        <span class="bar"></span>
        <label>Text</label>
    </div>
    <div class="button" onclick="fun()">点击复制文本到粘贴板 </div>
</body>

</html>
<script>
    fun = () => {
        const str = document.getElementById("inputText").value;
        if (str === "") {
            alert("请输入内容！");
        } else {
            const copyToClipboard = (text) => navigator.clipboard.writeText(text);
            copyToClipboard(str);
        }
    }
</script>
```

## 9、检查日期是否有效

```javascript
const isDateValid = (...val) => !Number.isNaN(new Date(...val).valueOf());   
console.log(isDateValid("2a-04-11")); // false
```

## 10、找出一年中的哪一天

```javascript
/**
 * @description: 
 * @param {*} date Mon Sep 27 2021 14:33:04 GMT+0800 (中国标准时间)v
 * @return {*} 找出一年中的哪一天
 */
const dayOfYear = (date) =>
Math.floor((date - new Date(date.getFullYear(), 0, 0)) / 1000 / 60 / 60 / 24);

console.log(dayOfYear(new Date()));
```

## 11 、找出两日期之间的天数

```javascript
/**
 * @description: 找出两日期之间的天数
 * @param {*} 开始的 中国标准时间
 * @param {*} 结束的 中国标准时间
 * @return {*} 两个日期的相差天数
 */    
const dayDif = (date1, date2) => Math.ceil(Math.abs(date1.getTime() - date2.getTime()) / 86400000);
const data = dayDif(new Date("2020-10-21"), new Date("2021-10-22"));

console.log(data,new Date("2021-10-22"));
```

## 12、清除所有 Cookie

```javascript
const clearCookies = document.cookie.split(';').forEach(cookie => document.cookie = cookie.replace(/^ +/, '').replace(/=.*/, `=;expires=${new Date(0).toUTCString()};path=/`));
```

## 13、生成随机十六进制

```javascript
const randomHex  = () => {
    return `#${Math.floor(Math.random() * 0xffffff).toString(16).padEnd(6, "0")}`
}
console.log(randomHex())
```

## 14、从数组中删除重复项

```javascript
const removeDuplicates = (arr) => [...new Set(arr)];
console.log(removeDuplicates([1, 2, 3, 3, 4, 4, 5, 5, 6]));
```

## 15、使用以下代码检查用户的设备是否处于暗模式

```javascript
const isDarkMode = window.matchMedia && window.matchMedia('(prefers-color-scheme: dark)').matches
console.log(isDarkMode)
```

## 16、将 RGB 转换为十六进制

```javascript
const rgbToHex = (r, g, b) => 
  "#" + ((1 << 24) + (r << 16) + (g << 8) + b).toString(16).slice(1);
rgbToHex(0, 51, 255); // #0033ff
```

## 17、使用内置的 getSelectionproperty 获取用户选择的文本

```javascript
const getSelectedText = () => window.getSelection().toString();
getSelectedText();
```

## 18、判断数据类型

```javascript
/**
 * 判断数据类型
 * 参考文章：https://segmentfault.com/a/1190000018160547
 * @param {*} obj Number | String | Boolean | Null | Undefined | Function | Array | Object
 * @returns array | object | number | string | null | undefined | function | date | regexp
 */
function _typeof(obj) {
    let type = Object.prototype.toString.call(obj);
    return type.match(/\[object (.*?)\]/)[1].toLowerCase();
}

// 几个特别的示例
console.log(_typeof(new Date())) // date
console.log(_typeof(new RegExp())) // regexp
console.log(_typeof(NaN)) // number
```
