
@[TOC]( )

# 一、划过div放大

```css
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title></title>
    <style>
        /* 让整个box剧中 */
        .box{
            margin-top:5%;
            margin-left:5%;
            width:500px;
            height:300px;
            position: relative;
            -webkit-transition: all .2s linear;
            transition: all .2s linear;
        }
        .show img{
            width:500px;
            height:300px;
        }
        /* 划过的时候，让其整体放大 */
        .show:hover {
            z-index: 1;
            -webkit-box-shadow: 0 15px 30px rgba(0,0,0,.1);
            box-shadow: 0 15px 30px rgba(0,0,0,.1);
            -webkit-transform: translate3d(0,-2px,0);
            transform: translate3d(0,-2px,0);
        }
    </style>
</head>
<body>
    <div class="box">
        <div class="show">
            <img src="https://dss3.bdstatic.com/70cFv8Sh_Q1YnxGkpoWK1HF6hhy/it/u=2534506313,1688529724&fm=26&gp=0.jpg" alt="">
        </div>
    </div>
</body>
</html>
```

# 二、划过div，给图片一个遮挡，遮挡里面显示图片

```css
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title></title>
    <style>
        /* 让整个box剧中 */
        .box{
            margin-top:5%;
            margin-left:5%;
            width:500px;
            height:300px;
            position: relative;
            -webkit-transition: all .2s linear;
            transition: all .2s linear;
        }
        .show img{
            width:500px;
            height:300px;
        }
        /* 给图片上加一个划过遮挡层 */
        .box:hover .shelter{
            position: absolute;
            left:0;
            top:0;
            width:100%;
            height:100%;
            background:rgba(201,201,201,1);
            opacity:0.5;
        }
        .img img{
            position: absolute;
            left:50%;
            top:50%;
            width:100px;
            height:100px;
            z-index:1;
            display: none;
        }
        .box:hover .img img{
            display: block;
        }
        /* 划过的时候，让其整体放大 */
        .box:hover {
            z-index: 2;
            -webkit-box-shadow: 0 15px 30px rgba(0,0,0,.1);
            box-shadow: 0 15px 30px rgba(0,0,0,.1);
            -webkit-transform: translate3d(0,-2px,0);
            transform: translate3d(0,-2px,0);
        }
    </style>
</head>
<body>
    <div class="box">
        <div class="show">
            <img src="https://dss3.bdstatic.com/70cFv8Sh_Q1YnxGkpoWK1HF6hhy/it/u=2534506313,1688529724&fm=26&gp=0.jpg" alt="">
        </div>
        <!-- 遮挡曾 -->
        <div class="shelter"></div>
        <div class="img">
            <img src="./image/timg.jpg" alt="">
        </div>
    </div>
</body>
</html>
```

# 三、居中

## 1、定位居中

### 1）在屏幕窗口居中

①已知大小的元素在屏幕窗口水平垂直都居中

```css
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title></title>
    <style>
        .box{
            width:100px;
            height:100px;
            background-color: aqua;
            position:fixed;
            left:50%;
            top:50%;
            margin-left:-50px;
            margin-top:-50px;
        }
    </style>
</head>
<body>
    <div class="box">

    </div>
</body>
</html>
```

②未知大小的元素在屏幕窗口水平垂直都居中

```css
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title></title>
    <style>
        .box{
            position:fixed;
            left:0;
            top:0;
            right:0;
            bottom:0;
            margin:auto;
        }
    </style>
</head>
<body>
    <div class="box">

    </div>
</body>
</html>
```

### 2）子元素在父元素中居中

①已知大小的子元素在父元素中居中

```
        父元素{
            /* 相对定位 */
            position: relative;
        }
        子元素{
            width:100px;
            height:100px;
            /* 绝对定位 */
            position:absolute;
            left:50%;
            top:50%;
            margin-left:-50px;
            margin-top:-50px;
        }
```

②未知大小的子元素在父元素中居中

```
        父元素{
            /* 相对定位 */
            position: relative;
        }
        子元素{
            /* 绝对定位 */
            position:absolute;
            left:0;
            top:0;
            right:0;
            bottom:0;
            margin:auto;
        }
```

## 2、使用弹性布局

1）使用flex布局的方式实现未知大小元素在屏幕窗口水平垂直都居中

```
        html,body{
            height:100%;
        }
        /* 让html和body的高度为屏幕窗口的高度（窗口高度自适应） */
        body{
            display:flex;
            justify-content:center;
            align-items:center;
        }
```

2）使用flex布局的方式实现未知大小的子元素在父元素中水平垂直都居中

```
        父元素{
            display:flex;
            justify-content:center;
            align-items:center;
        }
    
```

## 3、使用css3变形来实现

1）使用css3变形的方式实现未知大小的元素在屏幕窗口水平垂直都居中

```
        元素{
            position:fixed;
            left:50%;
            top:50%;
            transform:translateX(-50%) translateY(-50%);
        }
```

2）使用css变形的方式来实现未知大小的子元素，在父元素中水平垂直都居中

```
        父元素{
            position:relative;
        }
        子元素{
            position:absolute;
            left:50%;
            top:50%;
            transform:translate(-50%,-50%);
        }
```

# 四、让屏幕横过来，或者让某个div横着

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title></title>
    <style type="text/css">
        body {
            /* 将body铺满整个屏幕 */
            position: fixed;
            width: 100%;
            height: 100%;
            padding: 0;
            margin: 0;
            /* background-color: rgb(51, 51, 51); */
            overflow: hidden;
        }
        #main {
            /* 注意这里：初始时候我们需要将这个div的长宽给设置一下（ps：宽度要大于高度） */
            width: 560px;
            height: 320px;
        }
        @media screen and (orientation: portrait) {
            /* 竖屏状态下 */
            #main {//把下面的写给元素或者div，就可以 让其横着了
                position: absolute;
                width: 100vh;
                height: 100vw;
                top: 0;
                left: 100vw;
                -webkit-transform: rotate(90deg);
                -moz-transform: rotate(90deg);
                -ms-transform: rotate(90deg);
                transform: rotate(90deg);
                transform-origin: 0% 0%;
            }
        }
        @media screen and (orientation: landscape) {
            /* //横屏状态下 */
            #main {
                position: absolute;
                top: 0;
                left: 0;
                width: 100vw;
                height: 100vh;
            }
        }
    </style>
</head>
 <body>
     <!-- 这是我的画布 -->
     <div id="main">
         啊啊啊啊啊
     </div>
 </body>
</html>
```

# 五、css标签样式  初始化

```css
body,p,ul,ol,dl,dd,h1,h2,h3,h4,h5,h6,hr,td,input,textarea{
    margin:0;
    padding:0;
}
/* 这个 事移动端的，在pc的时候，把它注释掉 */
html{
    font-size: 26.67vw;
}
html,body{
    height: 100%;
}
ul,ol,li{
    list-style: none;
}
a{
    text-decoration:none;
}
em,i{
    font-style: normal;
}
input,button{
    outline:none;
}
/* 去掉下拉框的右边的小三角 */
select {
    appearance:none;
    -moz-appearance:none;
    -webkit-appearance:none;
    outline:none;
}
img{
    border:none;
    display: block;
}
```
