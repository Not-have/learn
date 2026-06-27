运动原理：JavaScript 实现运动的原理，就是通过定时器不断改变元素的位置，直至到达目标点后停止运动。通常，要让元素动起来，我们会通过改变元素的 left 和 top 值来改变元素的相对位置。

方法：

①运动的物体使用绝对定位（*position*: absolute;）；

②通过改变定位物体的属性（left、right、top、bottom）值来使物体移动。例如向右或左移动可以使用offsetLeft（offsetRight）来控制左右移动。

步骤：

①开始运动前，先清除已有定时器 （因为：是连续点击按钮，物体会运动越来越快,造成运动混乱）；

②开启定时器，计算速度；

③把运动和停止隔开（if/else),判断停止条件，执行运动。

## 1、匀速运动

```javascript
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title></title>
    <style>
        #box{
            width:100px;
            height:100px;
            background-color: aquamarine;
            position: absolute;
            top:100px;
        }
    </style>
</head>
<body>
    <button>点击</button>
    <div id="box">

    </div>
</body>
</html>
<script>
    let btn = document.querySelector("button");
    let box = document.getElementById("box");
    let time;
    function startMove() {
        // 每次点击的时候，先清除一次定时器，负责他会越来越快
        clearInterval(time);
        time = setInterval(() => {
            box.style.left = box.offsetLeft + 5 + "px";
            // 读取移动的值
            if(box.offsetLeft >= 300){
                // 给一个等于
                box.style.left = 300 + "px";
                clearInterval(time);
            }
        }, 50);
    }

    btn.onclick = function () {
        startMove();
    }
</script>
```

## 2、匀速往返运动

```javascript
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title></title>
    <style>
        #box{
            width:100px;
            height:100px;
            background-color: aquamarine;
            position: absolute;
            top:100px;
        }
    </style>
</head>
<body>
    <button class="start">开始</button>
    <button class="end">返回</button>
    <div id="box">

    </div>
</body>
</html>
<script>
    let start = document.querySelector(".start");
    let end = document.querySelector(".end");
    let box = document.getElementById("box");
    let time;
    function startMove(targat) {
        console.log(targat);
        // 每次点击的时候，先清除一次定时器，负责他会越来越快
        clearInterval(time);
        time = setInterval(() => {
            let speed = targat - box.offsetLeft > 0 ? 5 : -5 ;
            box.style.left = box.offsetLeft + speed + "px";
            if(box.offsetLeft >= 300 || box.offsetLeft <= 0){
                clearInterval(time);
            } 
        }, 50);
    }

    start.onclick = function () {
        startMove(300);
    }
    end.onclick = function (){
        startMove(0);
    }
</script>
```

## 3、鼠标移入，淡入淡出

```javascript
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <title></title>
    <style>
        #box {
            width: 100px;
            height: 100px;
            background-color:aquamarine;
            opacity: 0.1;
            position: absolute;
            left: 200px;
            top: 200px;
        }
    </style>
</head>
<body>
    <div id="box" onmouseover="startMove(1)" onmouseout="startMove(0.1)">

    </div>
</body>
</html>
<script>
    let oBox = document.querySelector("#box");
    let time = null;

    function startMove(targat) {
        clearInterval(time);
        time = setInterval(function () {
            //getComputedStyle获取非行内样式
            let speed = targat - getComputedStyle(oBox, false)["opacity"] > 0 ? 0.01 : -0.01;
            oBox.style.opacity = +getComputedStyle(oBox, false)["opacity"] + speed;
            if (getComputedStyle(oBox, false)["opacity"] >= 1 || getComputedStyle(oBox, false)["opacity"] <= 0.1) {
                clearInterval(time);
            }
        }, 50);
    }
</script>
```

## 4、匀速缓冲运动

```javascript
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <title></title>
    <style>
        #box {
            width: 100px;
            height: 100px;
            background-color: red;
            position: absolute;
            top: 200px;
        }
    </style>
</head>
<body>
    <button>启动</button>
    <div id="box">

    </div>
</body>
</html>
<script>
    let oBox = document.querySelector("#box");
    let oBtn = document.querySelector("button");
    let time;

    oBtn.onclick = function(){
        startMove(oBox,500);
    }

    /**
     * obj 获取的元素
     * target 缓冲移动的距离
    */
    function startMove(obj,target) {
        clearInterval(time);
        time = setInterval(function () {
            let speed = (target - obj.offsetLeft) / 10;
            //前辈大哥推导的算法,拿过来直接用
            //可以去除小数位,精确到整数
            speed = speed > 0 ? Math.ceil(speed) : Math.floor(speed);//核心算法
            obj.style.left = obj.offsetLeft + speed + "px";
            if (obj.offsetLeft == target) {
                clearInterval(time);
            }
        }, 50);
    }
</script>
```

## 5、反弹

```javascript
<!DOCTYPE html>
<html>
 <head>
  <meta charset="utf-8">
  <title></title>
  <style>
   #box{
    height: 25px;
    width: 25px;
    background-color: green;
    border-radius: 50%;
    position: absolute;
    top:200px;
   }
  </style>
 </head>
 <body>
  <button>启动</button>
  <div id="box">
   
  </div>
 </body>
</html>
<script>
 let oBox = document.querySelector("#box");
 let oBtn = document.querySelector("button");
 let time = null;
 
 function startMove(){
  let speedX = 5;
  let speedY = 5;
  
  time = setInterval(function(){
   oBox.style.left = oBox.offsetLeft + speedX + "px";
   oBox.style.top = oBox.offsetTop + speedY + "px";
   if(oBox.offsetLeft < 0){//rang box的边框到视口边框
    speedX *= -1;//等价于spddeX=speedX*-1;(让换个方向继续动)
   }
   
   let maxX = window.innerWidth - oBox.offsetWidth;
   
   if(oBox.offsetLeft > maxX){
    speedX *= -1;
   }
   
   if(oBox.offsetTop < 0){
    speedY *= -1;
   }
   
   let maxY = window.innerHeight - oBox.offsetHeight;
   
   if(oBox.offsetTop > maxY){
    speedY *= -1;
   }
  },10);
 }
 
 oBtn.onclick = function(){
  startMove();
 }
</script>
```

 ![image preview](./images/202203280032327.png)

## 6、抛物线

```javascript
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Document</title>
    <script>
        window.onload = function () {
            //y = ax*x + bx + c ;

            //通过已知公式可以通过x值求得a值;

            var box = document.getElementById("box");

            var target = document.getElementById("target");

            //曲率 >> 可以观察改变a的值会发生什么事情;

            //目标点的相对坐标; 

            var a = 0.008;
            var coord = {

                x: target.offsetLeft - box.offsetLeft,
                y: target.offsetTop - box.offsetTop

            }

            //求系数b
            //y = ax*x + bx + 0

            //bx = y - a*x*x 

            //公式为; b = (y - a*x*x) / x ; 
            var b = (coord.y - a * coord.x * coord.x) / coord.x;

            //原点坐标;
            var center = {

                x: box.offsetLeft,
                y: box.offsetTop

            }
            var speed = 1;
            var offsetX = 0;//在坐标轴上移动的值
            var timer = setInterval(function () {
                //console.log(box.offsetLeft)
                box.style.left = center.x + (offsetX += speed) + "px";
                box.style.top = center.y + (a * offsetX * offsetX + b * offsetX) + "px";

                if (offsetX >= coord.x) {

                    box.style.left = target.offsetLeft + 'px';
                    box.style.top = target.offsetTop + 'px';
                    clearInterval(timer);
                }
                //轨迹;
                var div = document.createElement('div')
                div.className = "box";
                div.style.left = center.x + (offsetX += speed) + "px";
                div.style.top = center.y + (a * offsetX * offsetX + b * offsetX) + "px";
                document.getElementsByTagName("body")[0].appendChild(div);
            }, 30)
        }

    </script>
    <style>
        * {
            margin: 0;
            padding: 0;
        }

        .box {
            width: 2px;
            height: 2px;
            background: #000;
            position: absolute;
        }

        #box {

            width: 30px;
            height: 30px;
            background: red;
            position: absolute;
            left: 100px;
            top: 500px;
        }

        #target {

            width: 30px;
            height: 30px;
            background: #000;
            position: absolute;
            left: 700px;
            top: 300px;
        }
    </style>
</head>
<body>
    <div id="box"></div>
    <div id="target"></div>
</body>
</html>
```

![image preview](./images/202203280033565.png)

## 7、重力回弹（类似与打皮球）

```javascript
<!DOCTYPE html>
<html lang="en">
<head>
 <meta charset="UTF-8">
 <title>Document</title>

 <style>
  *{
   margin: 0;
   padding: 0;
  }
  #container{
   width: 800px;
   height: 400px;
   margin: 30px auto;
   border: 1px solid #b6b6b6;
   position: relative;
  }
  #box{
   width: 50px;
   height: 50px;
   background:aquamarine;
   border-radius: 50%;
   position: absolute;
   left: 100px;
   top: 20px;
  }
 </style>
</head>
<body>
 <div id="container">  
  <div id="box">
   
  </div>
 </div>
</body>
</html>
<script>
     window.onload = function(){

   var g = 1;  //重力加速度;
   var vx = 2; //x轴速度;
   var vy = 0; //y轴速度;

            var ball = document.getElementById("box");
            var container = document.getElementById("container");

            var cH = document.documentElement.clientHeight || document.body.clientHeight;
            var containerHeight = container.offsetHeight;
            var conTop = container.offsetTop;
            var left, top;
   
   setInterval(function () {
                if (ball.offsetTop >= containerHeight - ball.offsetHeight) {

                    ball.style.top = containerHeight - ball.offsetHeight + "px";
                    // 控制弹的 高低
                    vy = -vy * .8;
                }
                vy = vy + g;

                left = ball.offsetLeft + vx;
                top = ball.offsetTop + vy;

                ball.style.left = left + "px";
                ball.style.top = top + "px";
            }, 30)
  }
</script>
```

## 8、案例（两侧跟随的广告）

```javascript
<!DOCTYPE html>
<html>
<head lang="en">
    <meta charset="UTF-8">
    <title></title>
    <style>
        img{
            position: absolute;
            left:0;
            top:50px;
        }
        #demo{
            width:1000px;
            margin:0 auto;
        }
    </style>
</head>
 
<body>
 <img src="https://gitee.com/Green_chicken/picture/raw/master/%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/20210216224103.jpg" alt="" id="pic"/>
    <div id="demo">
        <p>天王盖地虎，小鸡炖蘑菇</p>
        <p>天王盖地虎，小鸡炖蘑菇</p>
        <p>天王盖地虎，小鸡炖蘑菇</p>
        <p>天王盖地虎，小鸡炖蘑菇</p>
        <p>天王盖地虎，小鸡炖蘑菇</p>
        <p>天王盖地虎，小鸡炖蘑菇</p>
        <p>天王盖地虎，小鸡炖蘑菇</p>
        <p>天王盖地虎，小鸡炖蘑菇</p>
        <p>天王盖地虎，小鸡炖蘑菇</p>
        <p>天王盖地虎，小鸡炖蘑菇</p>
        <p>天王盖地虎，小鸡炖蘑菇</p>
        <p>天王盖地虎，小鸡炖蘑菇</p>
        <p>天王盖地虎，小鸡炖蘑菇</p>
        <p>天王盖地虎，小鸡炖蘑菇</p>
        <p>天王盖地虎，小鸡炖蘑菇</p>
        <p>天王盖地虎，小鸡炖蘑菇</p>
        <p>天王盖地虎，小鸡炖蘑菇</p>
        <p>天王盖地虎，小鸡炖蘑菇</p>
        <p>天王盖地虎，小鸡炖蘑菇</p>
        <p>天王盖地虎，小鸡炖蘑菇</p>
        <p>天王盖地虎，小鸡炖蘑菇</p>
        <p>天王盖地虎，小鸡炖蘑菇</p>
        <p>天王盖地虎，小鸡炖蘑菇</p>
        <p>天王盖地虎，小鸡炖蘑菇</p>
        <p>天王盖地虎，小鸡炖蘑菇</p>
        <p>天王盖地虎，小鸡炖蘑菇</p>
        <p>天王盖地虎，小鸡炖蘑菇</p>
        <p>天王盖地虎，小鸡炖蘑菇</p>
        <p>天王盖地虎，小鸡炖蘑菇</p>
        <p>天王盖地虎，小鸡炖蘑菇</p>
        <p>天王盖地虎，小鸡炖蘑菇</p>
        <p>天王盖地虎，小鸡炖蘑菇</p>
        <p>天王盖地虎，小鸡炖蘑菇</p>
        <p>天王盖地虎，小鸡炖蘑菇</p>
        <p>天王盖地虎，小鸡炖蘑菇</p>
        <p>天王盖地虎，小鸡炖蘑菇</p>
    </div>
</body>
</html>
<script type="text/javascript">
 var oPic = document.querySelector("#pic");
 window.onscroll = function(){
  //获取页面滚走的距离
  var sTop = document.documentElement.scrollTop || document.body.scrollTop;
  startMove( oPic  , 50 + sTop );
 }
 var timer = null;
 function startMove(obj,target){
  clearInterval(timer);
  timer = setInterval(function(){
   //缓冲运动原理
   var speed = (target-obj.offsetTop)/10;
   speed =  speed>0 ? Math.ceil(speed) : Math.floor(speed);
   if( obj.offsetTop == Math.floor(target) ){
    clearInterval( timer );
   }else{
    obj.style.top = obj.offsetTop + speed + "px";
   }
  },30)
 } 
</script>
```

![image preview](./images/202203280034675.png)

=======
运动原理：JavaScript 实现运动的原理，就是通过定时器不断改变元素的位置，直至到达目标点后停止运动。通常，要让元素动起来，我们会通过改变元素的 left 和 top 值来改变元素的相对位置。

方法：

①运动的物体使用绝对定位（*position*: absolute;）；

②通过改变定位物体的属性（left、right、top、bottom）值来使物体移动。例如向右或左移动可以使用offsetLeft（offsetRight）来控制左右移动。

步骤：

①开始运动前，先清除已有定时器 （因为：是连续点击按钮，物体会运动越来越快,造成运动混乱）；

②开启定时器，计算速度；

③把运动和停止隔开（if/else),判断停止条件，执行运动。

## 1、匀速运动

```javascript
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title></title>
    <style>
        #box{
            width:100px;
            height:100px;
            background-color: aquamarine;
            position: absolute;
            top:100px;
        }
    </style>
</head>
<body>
    <button>点击</button>
    <div id="box">

    </div>
</body>
</html>
<script>
    let btn = document.querySelector("button");
    let box = document.getElementById("box");
    let time;
    function startMove() {
        // 每次点击的时候，先清除一次定时器，负责他会越来越快
        clearInterval(time);
        time = setInterval(() => {
            box.style.left = box.offsetLeft + 5 + "px";
            // 读取移动的值
            if(box.offsetLeft >= 300){
                // 给一个等于
                box.style.left = 300 + "px";
                clearInterval(time);
            }
        }, 50);
    }

    btn.onclick = function () {
        startMove();
    }
</script>
```

## 2、匀速往返运动

```javascript
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title></title>
    <style>
        #box{
            width:100px;
            height:100px;
            background-color: aquamarine;
            position: absolute;
            top:100px;
        }
    </style>
</head>
<body>
    <button class="start">开始</button>
    <button class="end">返回</button>
    <div id="box">

    </div>
</body>
</html>
<script>
    let start = document.querySelector(".start");
    let end = document.querySelector(".end");
    let box = document.getElementById("box");
    let time;
    function startMove(targat) {
        console.log(targat);
        // 每次点击的时候，先清除一次定时器，负责他会越来越快
        clearInterval(time);
        time = setInterval(() => {
            let speed = targat - box.offsetLeft > 0 ? 5 : -5 ;
            box.style.left = box.offsetLeft + speed + "px";
            if(box.offsetLeft >= 300 || box.offsetLeft <= 0){
                clearInterval(time);
            } 
        }, 50);
    }

    start.onclick = function () {
        startMove(300);
    }
    end.onclick = function (){
        startMove(0);
    }
</script>
```

## 3、鼠标移入，淡入淡出

```javascript
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <title></title>
    <style>
        #box {
            width: 100px;
            height: 100px;
            background-color:aquamarine;
            opacity: 0.1;
            position: absolute;
            left: 200px;
            top: 200px;
        }
    </style>
</head>
<body>
    <div id="box" onmouseover="startMove(1)" onmouseout="startMove(0.1)">

    </div>
</body>
</html>
<script>
    let oBox = document.querySelector("#box");
    let time = null;

    function startMove(targat) {
        clearInterval(time);
        time = setInterval(function () {
            //getComputedStyle获取非行内样式
            let speed = targat - getComputedStyle(oBox, false)["opacity"] > 0 ? 0.01 : -0.01;
            oBox.style.opacity = +getComputedStyle(oBox, false)["opacity"] + speed;
            if (getComputedStyle(oBox, false)["opacity"] >= 1 || getComputedStyle(oBox, false)["opacity"] <= 0.1) {
                clearInterval(time);
            }
        }, 50);
    }
</script>
```

## 4、匀速缓冲运动

```javascript
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <title></title>
    <style>
        #box {
            width: 100px;
            height: 100px;
            background-color: red;
            position: absolute;
            top: 200px;
        }
    </style>
</head>
<body>
    <button>启动</button>
    <div id="box">

    </div>
</body>
</html>
<script>
    let oBox = document.querySelector("#box");
    let oBtn = document.querySelector("button");
    let time;

    oBtn.onclick = function(){
        startMove(oBox,500);
    }

    /**
     * obj 获取的元素
     * target 缓冲移动的距离
    */
    function startMove(obj,target) {
        clearInterval(time);
        time = setInterval(function () {
            let speed = (target - obj.offsetLeft) / 10;
            //前辈大哥推导的算法,拿过来直接用
            //可以去除小数位,精确到整数
            speed = speed > 0 ? Math.ceil(speed) : Math.floor(speed);//核心算法
            obj.style.left = obj.offsetLeft + speed + "px";
            if (obj.offsetLeft == target) {
                clearInterval(time);
            }
        }, 50);
    }
</script>
```

## 5、反弹

```javascript
<!DOCTYPE html>
<html>
 <head>
  <meta charset="utf-8">
  <title></title>
  <style>
   #box{
    height: 25px;
    width: 25px;
    background-color: green;
    border-radius: 50%;
    position: absolute;
    top:200px;
   }
  </style>
 </head>
 <body>
  <button>启动</button>
  <div id="box">
   
  </div>
 </body>
</html>
<script>
 let oBox = document.querySelector("#box");
 let oBtn = document.querySelector("button");
 let time = null;
 
 function startMove(){
  let speedX = 5;
  let speedY = 5;
  
  time = setInterval(function(){
   oBox.style.left = oBox.offsetLeft + speedX + "px";
   oBox.style.top = oBox.offsetTop + speedY + "px";
   if(oBox.offsetLeft < 0){//rang box的边框到视口边框
    speedX *= -1;//等价于spddeX=speedX*-1;(让换个方向继续动)
   }
   
   let maxX = window.innerWidth - oBox.offsetWidth;
   
   if(oBox.offsetLeft > maxX){
    speedX *= -1;
   }
   
   if(oBox.offsetTop < 0){
    speedY *= -1;
   }
   
   let maxY = window.innerHeight - oBox.offsetHeight;
   
   if(oBox.offsetTop > maxY){
    speedY *= -1;
   }
  },10);
 }
 
 oBtn.onclick = function(){
  startMove();
 }
</script>
```

 ![image preview](./images/202203280032327.png)

## 6、抛物线

```javascript
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Document</title>
    <script>
        window.onload = function () {
            //y = ax*x + bx + c ;

            //通过已知公式可以通过x值求得a值;

            var box = document.getElementById("box");

            var target = document.getElementById("target");

            //曲率 >> 可以观察改变a的值会发生什么事情;

            //目标点的相对坐标; 

            var a = 0.008;
            var coord = {

                x: target.offsetLeft - box.offsetLeft,
                y: target.offsetTop - box.offsetTop

            }

            //求系数b
            //y = ax*x + bx + 0

            //bx = y - a*x*x 

            //公式为; b = (y - a*x*x) / x ; 
            var b = (coord.y - a * coord.x * coord.x) / coord.x;

            //原点坐标;
            var center = {

                x: box.offsetLeft,
                y: box.offsetTop

            }
            var speed = 1;
            var offsetX = 0;//在坐标轴上移动的值
            var timer = setInterval(function () {
                //console.log(box.offsetLeft)
                box.style.left = center.x + (offsetX += speed) + "px";
                box.style.top = center.y + (a * offsetX * offsetX + b * offsetX) + "px";

                if (offsetX >= coord.x) {

                    box.style.left = target.offsetLeft + 'px';
                    box.style.top = target.offsetTop + 'px';
                    clearInterval(timer);
                }
                //轨迹;
                var div = document.createElement('div')
                div.className = "box";
                div.style.left = center.x + (offsetX += speed) + "px";
                div.style.top = center.y + (a * offsetX * offsetX + b * offsetX) + "px";
                document.getElementsByTagName("body")[0].appendChild(div);
            }, 30)
        }

    </script>
    <style>
        * {
            margin: 0;
            padding: 0;
        }

        .box {
            width: 2px;
            height: 2px;
            background: #000;
            position: absolute;
        }

        #box {

            width: 30px;
            height: 30px;
            background: red;
            position: absolute;
            left: 100px;
            top: 500px;
        }

        #target {

            width: 30px;
            height: 30px;
            background: #000;
            position: absolute;
            left: 700px;
            top: 300px;
        }
    </style>
</head>
<body>
    <div id="box"></div>
    <div id="target"></div>
</body>
</html>
```

![image preview](./images/202203280033565.png)

## 7、重力回弹（类似与打皮球）

```javascript
<!DOCTYPE html>
<html lang="en">
<head>
 <meta charset="UTF-8">
 <title>Document</title>

 <style>
  *{
   margin: 0;
   padding: 0;
  }
  #container{
   width: 800px;
   height: 400px;
   margin: 30px auto;
   border: 1px solid #b6b6b6;
   position: relative;
  }
  #box{
   width: 50px;
   height: 50px;
   background:aquamarine;
   border-radius: 50%;
   position: absolute;
   left: 100px;
   top: 20px;
  }
 </style>
</head>
<body>
 <div id="container">  
  <div id="box">
   
  </div>
 </div>
</body>
</html>
<script>
     window.onload = function(){

   var g = 1;  //重力加速度;
   var vx = 2; //x轴速度;
   var vy = 0; //y轴速度;

            var ball = document.getElementById("box");
            var container = document.getElementById("container");

            var cH = document.documentElement.clientHeight || document.body.clientHeight;
            var containerHeight = container.offsetHeight;
            var conTop = container.offsetTop;
            var left, top;
   
   setInterval(function () {
                if (ball.offsetTop >= containerHeight - ball.offsetHeight) {

                    ball.style.top = containerHeight - ball.offsetHeight + "px";
                    // 控制弹的 高低
                    vy = -vy * .8;
                }
                vy = vy + g;

                left = ball.offsetLeft + vx;
                top = ball.offsetTop + vy;

                ball.style.left = left + "px";
                ball.style.top = top + "px";
            }, 30)
  }
</script>
```

## 8、案例（两侧跟随的广告）

```javascript
<!DOCTYPE html>
<html>
<head lang="en">
    <meta charset="UTF-8">
    <title></title>
    <style>
        img{
            position: absolute;
            left:0;
            top:50px;
        }
        #demo{
            width:1000px;
            margin:0 auto;
        }
    </style>
</head>
 
<body>
 <img src="https://gitee.com/Green_chicken/picture/raw/master/%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/20210216224103.jpg" alt="" id="pic"/>
    <div id="demo">
        <p>天王盖地虎，小鸡炖蘑菇</p>
        <p>天王盖地虎，小鸡炖蘑菇</p>
        <p>天王盖地虎，小鸡炖蘑菇</p>
        <p>天王盖地虎，小鸡炖蘑菇</p>
        <p>天王盖地虎，小鸡炖蘑菇</p>
        <p>天王盖地虎，小鸡炖蘑菇</p>
        <p>天王盖地虎，小鸡炖蘑菇</p>
        <p>天王盖地虎，小鸡炖蘑菇</p>
        <p>天王盖地虎，小鸡炖蘑菇</p>
        <p>天王盖地虎，小鸡炖蘑菇</p>
        <p>天王盖地虎，小鸡炖蘑菇</p>
        <p>天王盖地虎，小鸡炖蘑菇</p>
        <p>天王盖地虎，小鸡炖蘑菇</p>
        <p>天王盖地虎，小鸡炖蘑菇</p>
        <p>天王盖地虎，小鸡炖蘑菇</p>
        <p>天王盖地虎，小鸡炖蘑菇</p>
        <p>天王盖地虎，小鸡炖蘑菇</p>
        <p>天王盖地虎，小鸡炖蘑菇</p>
        <p>天王盖地虎，小鸡炖蘑菇</p>
        <p>天王盖地虎，小鸡炖蘑菇</p>
        <p>天王盖地虎，小鸡炖蘑菇</p>
        <p>天王盖地虎，小鸡炖蘑菇</p>
        <p>天王盖地虎，小鸡炖蘑菇</p>
        <p>天王盖地虎，小鸡炖蘑菇</p>
        <p>天王盖地虎，小鸡炖蘑菇</p>
        <p>天王盖地虎，小鸡炖蘑菇</p>
        <p>天王盖地虎，小鸡炖蘑菇</p>
        <p>天王盖地虎，小鸡炖蘑菇</p>
        <p>天王盖地虎，小鸡炖蘑菇</p>
        <p>天王盖地虎，小鸡炖蘑菇</p>
        <p>天王盖地虎，小鸡炖蘑菇</p>
        <p>天王盖地虎，小鸡炖蘑菇</p>
        <p>天王盖地虎，小鸡炖蘑菇</p>
        <p>天王盖地虎，小鸡炖蘑菇</p>
        <p>天王盖地虎，小鸡炖蘑菇</p>
        <p>天王盖地虎，小鸡炖蘑菇</p>
    </div>
</body>
</html>
<script type="text/javascript">
 var oPic = document.querySelector("#pic");
 window.onscroll = function(){
  //获取页面滚走的距离
  var sTop = document.documentElement.scrollTop || document.body.scrollTop;
  startMove( oPic  , 50 + sTop );
 }
 var timer = null;
 function startMove(obj,target){
  clearInterval(timer);
  timer = setInterval(function(){
   //缓冲运动原理
   var speed = (target-obj.offsetTop)/10;
   speed =  speed>0 ? Math.ceil(speed) : Math.floor(speed);
   if( obj.offsetTop == Math.floor(target) ){
    clearInterval( timer );
   }else{
    obj.style.top = obj.offsetTop + speed + "px";
   }
  },30)
 } 
</script>
```

![image preview](./images/202203280034675.png)
