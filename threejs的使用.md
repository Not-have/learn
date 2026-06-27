# 一、threejs文档

中文文档：<http://www.webgl3d.cn/>

官方文档：<https://threejs.org/docs/index.html#manual/zh/introduction/Installation>

本地文档启动方式：

<https://github.com/mrdoob/three.js>

```bash
git cloen https://github.com/mrdoob/three.js.git
# cd three.js
npm i
npm run start
```

threejs是基于WebGL封装的。

# 二、下载

```bash
# 安装整个threejs
npm install --save three

# typescript 版时下载，同时也可提高代码提示
npm i @types/three -D
```

# 三、渲染流程

渲染流程：

## 1、引入

```javascript
import * as THREE from 'three';
```

## 2、创建场景

```javascript
import * as THREE from 'three';

const scene = new THREE.Scene();
```

## 3、创建相机

[doc](https://threejs.org/docs/index.html#api/zh/cameras/PerspectiveCamera)

```javascript
import * as THREE from 'three';

// 创建场景
const scene = new THREE.Scene();

// 创建相机（近大远小）
const camera = new THREE.PerspectiveCamera(
    45, // 视角
    window.innerWidth / window.innerHeight, // 窗口大小比例
    0.1, // 近平面
    1000 // 远平面
);

camera.position.z = 5; // 蓝色  // 设置相机位置
camera.position.y = 2; // 红色
camera.position.x = 2; // 绿色

camera.lookAt(0, 0, 0); // 旋转对象以面向世界空间中的点
```

## 4、创建渲染器

 <img src="./images/image-20240407002517482.png" alt="image-20240407002517482" style="zoom:33%;" />

[doc](https://threejs.org/docs/index.html?q=WebGLRenderer#api/zh/renderers/WebGLRenderer)

``` javascript
import * as THREE from 'three';

// 创建场景
const scene = new THREE.Scene();

// 创建相机（近大远小）
const camera = new THREE.PerspectiveCamera(
    45, // 视角
    window.innerWidth / window.innerHeight, // 窗口大小比例
    0.1, // 近平面
    1000 // 远平面
);

camera.position.z = 5; // 蓝色  // 设置相机位置
camera.position.y = 2; // 红色
camera.position.x = 2; // 绿色

camera.lookAt(0, 0, 0); // 旋转对象以面向世界空间中的点

// 创建渲染器
const renderer = new THREE.WebGLRenderer();
renderer.setSize(window.innerWidth, window.innerHeight); // 设置大小
document.body.appendChild(renderer.domElement); // 设置渲染的元素
```

## 5、创建物体

[doc](https://threejs.org/docs/index.html#api/zh/geometries/BoxGeometry)

注：几何体的类型：

 <img src="./images/image-20240406235857937.png" alt="image-20240406235857937" style="zoom:25%;" />

```javascript
import * as THREE from 'three';

// 创建场景
const scene = new THREE.Scene();

// 创建相机（近大远小）
const camera = new THREE.PerspectiveCamera(
    45, // 视角
    window.innerWidth / window.innerHeight, // 窗口大小比例
    0.1, // 近平面
    1000 // 远平面
);

camera.position.z = 5; // 蓝色  // 设置相机位置
camera.position.y = 2; // 红色
camera.position.x = 2; // 绿色

camera.lookAt(0, 0, 0); // 旋转对象以面向世界空间中的点

// 创建渲染器
const renderer = new THREE.WebGLRenderer();
renderer.setSize(window.innerWidth, window.innerHeight); // 设置大小
document.body.appendChild(renderer.domElement); // 设置渲染的元素(document.body 可以改为指定的 DOM)

// 创建物体
const geometry = new THREE.BoxGeometry(1, 1, 1);
```

## 6、创建物体材质

[doc](https://threejs.org/docs/index.html?q=MeshBasicMaterial#api/zh/materials/MeshBasicMaterial)

 <img src="./images/image-20240407000551404.png" alt="image-20240407000551404" style="zoom:25%;" />

```javascript
import * as THREE from 'three';

// 创建场景
const scene = new THREE.Scene();

// 创建相机（近大远小）
const camera = new THREE.PerspectiveCamera(
    45, // 视角
    window.innerWidth / window.innerHeight, // 窗口大小比例
    0.1, // 近平面
    1000 // 远平面
);

camera.position.z = 5; // 蓝色  // 设置相机位置
camera.position.y = 2; // 红色
camera.position.x = 2; // 绿色
camera.lookAt(0, 0, 0); // 旋转对象以面向世界空间中的点

// 创建渲染器
const renderer = new THREE.WebGLRenderer();
renderer.setSize(window.innerWidth, window.innerHeight); // 设置大小
document.body.appendChild(renderer.domElement); // 设置渲染的元素(document.body 可以改为指定的 DOM)

// 创建物体
const geometry = new THREE.BoxGeometry(1, 1, 1);

// 创建材质
const material = new THREE.MeshBasicMaterial({ color: 'green' });
```

## 7、创建网格

[doc](https://threejs.org/docs/index.html?q=Mesh#api/zh/objects/Mesh)

注：网格也属于物体。

```javascript
import * as THREE from 'three';

// 创建场景
const scene = new THREE.Scene();

// 创建相机（近大远小）
const camera = new THREE.PerspectiveCamera(
    45, // 视角
    window.innerWidth / window.innerHeight, // 窗口大小比例
    0.1, // 近平面
    1000 // 远平面
);

camera.position.z = 5; // 蓝色  // 设置相机位置
camera.position.y = 2; // 红色
camera.position.x = 2; // 绿色

camera.lookAt(0, 0, 0); // 旋转对象以面向世界空间中的点

// 创建渲染器
const renderer = new THREE.WebGLRenderer();
renderer.setSize(window.innerWidth, window.innerHeight); // 设置大小
document.body.appendChild(renderer.domElement); // 设置渲染的元素(document.body 可以改为指定的 DOM)

// 创建物体
const geometry = new THREE.BoxGeometry(1, 1, 1);

// 创建材质
const material = new THREE.MeshBasicMaterial({ color: 'green' });

// 创建网格
const cube = new THREE.Mesh(geometry, material); // 将物体和材质，添加进网格
```

## 8、将网格添加到场景

```javascript
import * as THREE from 'three';

// 创建场景
const scene = new THREE.Scene();

// 创建相机（近大远小）
const camera = new THREE.PerspectiveCamera(
    45, // 视角
    window.innerWidth / window.innerHeight, // 窗口大小比例
    0.1, // 近平面
    1000 // 远平面
);

camera.position.z = 5; // 蓝色  // 设置相机位置
camera.position.y = 2; // 红色
camera.position.x = 2; // 绿色

camera.lookAt(0, 0, 0); // 旋转对象以面向世界空间中的点

// 创建渲染器
const renderer = new THREE.WebGLRenderer();
renderer.setSize(window.innerWidth, window.innerHeight); // 设置大小
document.body.appendChild(renderer.domElement); // 设置渲染的元素(document.body 可以改为指定的 DOM)

// 创建物体
const geometry = new THREE.BoxGeometry(1, 1, 1);

// 创建材质
const material = new THREE.MeshBasicMaterial({ color: 'green' });

// 创建网格
const cube = new THREE.Mesh(geometry, material); // 将物体和材质，添加进网格

// 将网格添加到场景
scene.add(cube);
```

## 9、添加坐标器

[doc](https://threejs.org/docs/index.html?q=AxesHelper#api/zh/helpers/AxesHelper)

红色代表 X 轴

绿色代表 Y 轴

蓝色代表 Z 轴

```javascript
import * as THREE from 'three';

// 创建场景
const scene = new THREE.Scene();

// 创建相机（近大远小）
const camera = new THREE.PerspectiveCamera(
    45, // 视角
    window.innerWidth / window.innerHeight, // 窗口大小比例
    0.1, // 近平面
    1000 // 远平面
);

camera.position.z = 5; // 蓝色  // 设置相机位置
camera.position.y = 2; // 红色
camera.position.x = 2; // 绿色

camera.lookAt(0, 0, 0); // 旋转对象以面向世界空间中的点

// 创建渲染器
const renderer = new THREE.WebGLRenderer();
renderer.setSize(window.innerWidth, window.innerHeight); // 设置大小
document.body.appendChild(renderer.domElement); // 设置渲染的元素(document.body 可以改为指定的 DOM)

// 创建物体
const geometry = new THREE.BoxGeometry(1, 1, 1);

// 创建材质
const material = new THREE.MeshBasicMaterial({ color: 'green' });

// 创建网格
const cube = new THREE.Mesh(geometry, material); // 将物体和材质，添加进网格

// 将网格添加到场景
scene.add(cube);

// 添加坐标器
const axesHelper = new THREE.AxesHelper(5); // 坐标系长度为 5
scene.add(axesHelper);
```

## 10、添加轨道控制器

[doc](https://threejs.org/docs/index.html?q=OrbitControls#examples/zh/controls/OrbitControls)

```javascript
import * as THREE from 'three';

// 创建场景
const scene = new THREE.Scene();

// 创建相机（近大远小）
const camera = new THREE.PerspectiveCamera(
    45, // 视角
    window.innerWidth / window.innerHeight, // 窗口大小比例
    0.1, // 近平面
    1000 // 远平面
);

camera.position.z = 5; // 蓝色  // 设置相机位置
camera.position.y = 2; // 红色
camera.position.x = 2; // 绿色

camera.lookAt(0, 0, 0); // 旋转对象以面向世界空间中的点

// 创建渲染器
const renderer = new THREE.WebGLRenderer();
renderer.setSize(window.innerWidth, window.innerHeight); // 设置大小
document.body.appendChild(renderer.domElement); // 设置渲染的元素(document.body 可以改为指定的 DOM)

// 创建物体
const geometry = new THREE.BoxGeometry(1, 1, 1);

// 创建材质
const material = new THREE.MeshBasicMaterial({ color: 'green' });

// 创建网格
const cube = new THREE.Mesh(geometry, material); // 将物体和材质，添加进网格

// 将网格添加到场景
scene.add(cube);

// 添加坐标器
const axesHelper = new THREE.AxesHelper(5); // 坐标系长度为 5
scene.add(axesHelper);

// 添加 轨道控制器
import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls.js'; // 记得放到顶部
const controls = new OrbitControls(camera, renderer.domElement);
controls.enableDamping = true; // 惯性
```

## 11、渲染

[doc 渲染器 ——>  WebGLRenderer ——> render 属性](https://threejs.org/docs/index.html?q=render#api/zh/renderers/WebGLRenderer)

```javascript
import * as THREE from 'three';

// 创建场景
const scene = new THREE.Scene();

// 创建相机（近大远小）
const camera = new THREE.PerspectiveCamera(
    45, // 视角
    window.innerWidth / window.innerHeight, // 窗口大小比例
    0.1, // 近平面
    1000 // 远平面
);

camera.position.z = 5; // 蓝色  // 设置相机位置
camera.position.y = 2; // 红色
camera.position.x = 2; // 绿色

camera.lookAt(0, 0, 0); // 旋转对象以面向世界空间中的点

// 创建渲染器
const renderer = new THREE.WebGLRenderer();
renderer.setSize(window.innerWidth, window.innerHeight); // 设置大小
document.body.appendChild(renderer.domElement); // 设置渲染的元素(document.body 可以改为指定的 DOM)

// 创建物体
const geometry = new THREE.BoxGeometry(1, 1, 1);

// 创建材质
const material = new THREE.MeshBasicMaterial({ color: 'green' });

// 创建网格
const cube = new THREE.Mesh(geometry, material); // 将物体和材质，添加进网格

// 将网格添加到场景
scene.add(cube);

// 添加坐标器
const axesHelper = new THREE.AxesHelper(5); // 坐标系长度为 5
scene.add(axesHelper);

// 添加 轨道控制器
import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls.js';
const controls = new OrbitControls(camera, renderer.domElement);
controls.enableDamping = true; // 惯性

// 渲染
function animate() {
    controls.update(); // 不加这个无法进行拖动
    /**
     * 在Three.js中，requestAnimationFrame是用于执行动画循环的函数。它接受一个回调函数作为参数，该回调函数在浏览器准备好更新动画帧时执行。这样做的好处是，它会根据浏览器的刷新率来调整动画的帧率，以便达到更好的性能和流畅度。
     */
    requestAnimationFrame(animate);
    // 旋转
    // cube.rotation.x += 0.01;
    // cube.rotation.y += 0.01;
    renderer.render(scene, camera);
}
animate();
```

![image-20240407002036049](./images/image-20240407002036049.png)

~~简易 demo：~~

```vue
<template>
    <div ref="demo1" class="demo1"></div>
</template>

<script>
import { defineComponent, ref, onMounted } from "vue";
import * as THREE from "three";
import OrbitControls from 'three-orbitcontrols';
export default {
    name: "demo1",
    components: {},
    setup() {
        let demo1 = ref(null);
        onMounted(() => {
            let el = demo1.value;
            /**
             * 创建场景对象Scene
             */
            var scene = new THREE.Scene();
            /**
             * 创建网格模型
             */
            // var geometry = new THREE.SphereGeometry(60, 40, 40); //创建一个球体几何对象
            var geometry = new THREE.BoxGeometry(100, 100, 100); //创建一个立方体几何对象Geometry
            var material = new THREE.MeshLambertMaterial({
                color: 0x5f9ea0,
                lightMapIntensity: 3,
            }); //材质对象Material
            var mesh = new THREE.Mesh(geometry, material); //网格模型对象Mesh
            scene.add(mesh); //网格模型添加到场景中
            /**
             * 光源设置
             */
            //点光源
            var point = new THREE.PointLight(0xffffff);
            point.position.set(400, 600, 600); //点光源位置
            scene.add(point); //点光源添加到场景中
            //环境光
            var ambient = new THREE.AmbientLight(0x444444);
            scene.add(ambient);
            // console.log(scene)
            // console.log(scene.children)
            /**
             * 相机设置
             */
            var width = demo1.value.offsetWidth; //窗口宽度
            var height = demo1.value.offsetHeight; //窗口高度
            var k = width / height; //窗口宽高比
            var s = 400; //三维场景显示范围控制系数，系数越大，显示的范围越大
            //创建相机对象
            var camera = new THREE.OrthographicCamera(
                -s * k,
                s * k,
                s,
                -s,
                1,
                1000
            );
            camera.position.set(200, 300, 200); //设置相机位置
            camera.lookAt(scene.position); //设置相机方向(指向的场景对象)
            /**
             * 创建渲染器对象
             */
            var renderer = new THREE.WebGLRenderer();
            renderer.setSize(width, height); //设置渲染区域尺寸
            renderer.setClearColor(0xffffff, 1); //设置背景颜色
            el.appendChild(renderer.domElement); //body元素中插入canvas对象
            //执行渲染操作   指定场景、相机作为参数
            // renderer.render(scene, camera);
            var controls = new OrbitControls(camera, renderer.domElement);//创建控件对象
            /**
             * 去除鼠标旋转时，预留的边框
             */
            renderer.domElement.removeAttribute("tabindex")
            controls.addEventListener('change', render);//监听鼠标、键盘事件

            let T0 = new Date(); //上次时间
            function render() {
                let T1 = new Date(); //本次时间
                let t = T1 - T0; //时间差
                T0 = T1; //把本次时间赋值给上次时间
                requestAnimationFrame(render);
                renderer.render(scene, camera); //执行渲染操作
                mesh.rotateY(.00010 * t); //旋转角速度0.001弧度每毫秒
            }
            render();
        });
        return { demo1 };
    },
}
</script>

<style scoped>
.demo1 {
    width: 100%;
    height: 100%;
}
</style>
```

# 四、API

## 1、初步渲染

```js
/*
https://www.bilibili.com/read/readlist/rl594352
*/
import './style.css';

import * as THREE from 'three';

const app = document.querySelector<HTMLDivElement>('#app')!;

// 创建场景
const scene = new THREE.Scene();

// 创建相机（近大远小）
const camera = new THREE.PerspectiveCamera(
    45, // 视角
    window.innerWidth / window.innerHeight, // 窗口大小
    0.1, // 近平面
    1000 // 远平面
);

// 创建渲染器
const renderer = new THREE.WebGLRenderer();
renderer.setSize(window.innerWidth, window.innerHeight);
app.appendChild(renderer.domElement);

// 创建物体
const geometry = new THREE.BoxGeometry(1, 1, 1);
// 创建材质
const material = new THREE.MeshBasicMaterial({ color: 'red' });
// 创建网格
const cube = new THREE.Mesh(geometry, material);

// 将网格添加到场景
scene.add(cube);

// 设置相机位置
camera.position.z = 5;
camera.lookAt(0, 0, 0);

// 渲染
// renderer.render(scene, camera); 
// 渲染函数
function animate() {
    requestAnimationFrame(animate);
    // 旋转
    cube.rotation.x += 0.01;
    cube.rotation.y += 0.01;
    renderer.render(scene, camera);
}
animate();
```

 <img src="./images/image-20240406194956402.png" alt="image-20240406194956402" style="zoom:25%;" />

## 2、坐标器

注：

<font color='#004d82'>z - 蓝色</font>

<font color='#3a7a00'>y - 绿色</font>

<font color=red>x - 红色</font>

```javascript
import './style.css';

import * as THREE from 'three';

const app = document.querySelector<HTMLDivElement>('#app')!;

// 创建场景
const scene = new THREE.Scene();

// 创建相机（近大远小）
const camera = new THREE.PerspectiveCamera(
    45, // 视角
    window.innerWidth / window.innerHeight, // 窗口大小
    0.1, // 近平面
    1000 // 远平面
);

// 创建渲染器
const renderer = new THREE.WebGLRenderer();
renderer.setSize(window.innerWidth, window.innerHeight);
app.appendChild(renderer.domElement);

// 创建物体
const geometry = new THREE.BoxGeometry(1, 1, 1);
// 创建材质
const material = new THREE.MeshBasicMaterial({ color: 'green' });
// 创建网格
const cube = new THREE.Mesh(geometry, material);

// 将网格添加到场景
scene.add(cube);

// 设置相机位置
camera.position.z = 5;
camera.lookAt(0, 0, 0);

// 添加坐标器
const axesHelper = new THREE.AxesHelper(5); // 坐标系长度为 5
scene.add(axesHelper);

// 渲染
// renderer.render(scene, camera); 
// 渲染函数
function animate() {
    requestAnimationFrame(animate);
    // 旋转
    cube.rotation.x += 0.01;
    cube.rotation.y += 0.01;
    renderer.render(scene, camera);
}
animate();
```

## 3、导入轨道控制器

[doc](https://threejs.org/docs/index.html#examples/en/controls/OrbitControls)

```js
import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls.js';

// 添加 轨道控制器
const controls = new OrbitControls(camera, renderer.domElement); // 相机, 渲染元素

// 更新轨道控制器（添加到动画中）
controls.update();
```

## 4、三维向量

[doc](https://threejs.org/docs/index.html#api/zh/math/Vector3)

### 1）物体移动

```javascript
/**
 * https://threejs.org/docs/#api/zh/core/Object3D
 */

import * as THREE from 'three';
import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls.js';

const app = document.querySelector<HTMLDivElement>('#app')!;

// 创建场景
const scene = new THREE.Scene();

// 创建相机（近大远小）
const camera = new THREE.PerspectiveCamera(
    45, // 视角
    window.innerWidth / window.innerHeight, // 窗口大小
    0.1, // 近平面
    1000 // 远平面
);

// 创建渲染器
const renderer = new THREE.WebGLRenderer();
renderer.setSize(window.innerWidth, window.innerHeight);
app.appendChild(renderer.domElement);

// 创建物体
const geometry = new THREE.BoxGeometry(1, 1, 1);
// 创建材质
const material = new THREE.MeshBasicMaterial({ color: 'green' });
// 创建网格
const cube = new THREE.Mesh(geometry, material);

cube.position.set(1, 1, 1); // 移动物体

// 将网格添加到场景
scene.add(cube);

// 设置相机位置
camera.position.z = 5; // 蓝色
camera.position.y = 2; // 红色
camera.position.x = 2; // 绿色

camera.lookAt(0, 0, 0);

// 添加坐标器
const axesHelper = new THREE.AxesHelper(5); // 坐标系长度为 5
scene.add(axesHelper);

// 添加 轨道控制器
const controls = new OrbitControls(camera, renderer.domElement);
// 惯性
controls.enableDamping = true;


// 渲染
// renderer.render(scene, camera); 
// 渲染函数
function animate() {
    controls.update();
    requestAnimationFrame(animate);
    // 旋转
    // cube.rotation.x += 0.01;
    // cube.rotation.y += 0.01;
    renderer.render(scene, camera);
}
animate();
```

### 2）父子元素

```javascript
/**
 * https://threejs.org/docs/#api/zh/core/Object3D
 */

import * as THREE from 'three';
import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls.js';

const app = document.querySelector<HTMLDivElement>('#app')!;

// 创建场景
const scene = new THREE.Scene();

// 创建相机（近大远小）
const camera = new THREE.PerspectiveCamera(
    45, // 视角
    window.innerWidth / window.innerHeight, // 窗口大小
    0.1, // 近平面
    1000 // 远平面
);

// 创建渲染器
const renderer = new THREE.WebGLRenderer();
renderer.setSize(window.innerWidth, window.innerHeight);
app.appendChild(renderer.domElement);

// 创建物体
const geometry = new THREE.BoxGeometry(1, 1, 1);

// 创建材质
const parentMaterial = new THREE.MeshBasicMaterial({ color: 'red' });
const material = new THREE.MeshBasicMaterial({ color: 'green' });

// 创建网格
const parentCube = new THREE.Mesh(geometry, parentMaterial); // 这个是父元素
const cube = new THREE.Mesh(geometry, material); // 子元素

parentCube.add(cube); // 将子元素添加到父元素

parentCube.position.set(-3, 0, 0);
cube.position.set(3, 0, 0); // 移动物体

// 将网格添加到场景
// scene.add(cube);
scene.add(parentCube);

// 设置相机位置
camera.position.z = 5; // 蓝色
camera.position.y = 2; // 红色
camera.position.x = 2; // 绿色

camera.lookAt(0, 0, 0);

// 添加坐标器
const axesHelper = new THREE.AxesHelper(5); // 坐标系长度为 5
scene.add(axesHelper);

// 添加 轨道控制器
const controls = new OrbitControls(camera, renderer.domElement);
// 惯性
controls.enableDamping = true;

// 渲染
// renderer.render(scene, camera); 
// 渲染函数
function animate() {
    controls.update();
    requestAnimationFrame(animate);
    // 旋转
    // cube.rotation.x += 0.01;
    // cube.rotation.y += 0.01;
    renderer.render(scene, camera);
}
animate();
```

![image-20240406230111461](./images/image-20240406230111461.png)

### 3）缩放

注：相对于父元素来进行的放大、缩小。

```javascript
/**
 * https://threejs.org/docs/#api/zh/core/Object3D
 */

import * as THREE from 'three';
import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls.js';

const app = document.querySelector<HTMLDivElement>('#app')!;

// 创建场景
const scene = new THREE.Scene();

// 创建相机（近大远小）
const camera = new THREE.PerspectiveCamera(
    45, // 视角
    window.innerWidth / window.innerHeight, // 窗口大小
    0.1, // 近平面
    1000 // 远平面
);

// 创建渲染器
const renderer = new THREE.WebGLRenderer();
renderer.setSize(window.innerWidth, window.innerHeight);
app.appendChild(renderer.domElement);

// 创建物体
const geometry = new THREE.BoxGeometry(1, 1, 1);

// 创建材质
const parentMaterial = new THREE.MeshBasicMaterial({ color: 'azure' });
const material = new THREE.MeshBasicMaterial({ color: 'green' });

// 创建网格
const parentCube = new THREE.Mesh(geometry, parentMaterial); // 这个是父元素
const cube = new THREE.Mesh(geometry, material); // 子元素

parentCube.add(cube); // 将子元素添加到父元素

parentCube.position.set(-3, 0, 0);
cube.position.set(3, 0, 0); // 移动物体
cube.scale.set(2, 2, 2); // 设置立法体放大

// 将网格添加到场景
// scene.add(cube);
scene.add(parentCube);

// 设置相机位置
camera.position.z = 5; // 蓝色
camera.position.y = 2; // 红色
camera.position.x = 2; // 绿色

camera.lookAt(0, 0, 0);

// 添加坐标器
const axesHelper = new THREE.AxesHelper(5); // 坐标系长度为 5
scene.add(axesHelper);

// 添加 轨道控制器
const controls = new OrbitControls(camera, renderer.domElement);
// 惯性
controls.enableDamping = true;

// 渲染
// renderer.render(scene, camera); 
// 渲染函数
function animate() {
    controls.update();
    requestAnimationFrame(animate);
    // 旋转
    // cube.rotation.x += 0.01;
    // cube.rotation.y += 0.01;
    renderer.render(scene, camera);
}
animate();
```

### 4、旋转

注：旋转是基于 [欧拉角](https://zh.wikipedia.org/wiki/%E6%AC%A7%E6%8B%89%E8%A7%92)。

[欧拉角 doc](https://threejs.org/docs/index.html#api/zh/math/Euler)

```javascript
/**
 * https://threejs.org/docs/#api/zh/core/Object3D
 */

import * as THREE from 'three';
import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls.js';

const app = document.querySelector<HTMLDivElement>('#app')!;

// 创建场景
const scene = new THREE.Scene();

// 创建相机（近大远小）
const camera = new THREE.PerspectiveCamera(
    45, // 视角
    window.innerWidth / window.innerHeight, // 窗口大小
    0.1, // 近平面
    1000 // 远平面
);

// 创建渲染器
const renderer = new THREE.WebGLRenderer();
renderer.setSize(window.innerWidth, window.innerHeight);
app.appendChild(renderer.domElement);

// 创建物体
const geometry = new THREE.BoxGeometry(1, 1, 1);

// 创建材质
const parentMaterial = new THREE.MeshBasicMaterial({ color: 'azure' });
const material = new THREE.MeshBasicMaterial({ color: 'green' });

// 创建网格
const parentCube = new THREE.Mesh(geometry, parentMaterial); // 这个是父元素
const cube = new THREE.Mesh(geometry, material); // 子元素

parentCube.add(cube); // 将子元素添加到父元素

parentCube.position.set(-3, 0, 0);
parentCube.rotation.x = Math.PI / 4; // 旋转

cube.position.set(3, 0, 0); // 移动物体
cube.scale.set(1, 1, 1); // 设置立法体放大
cube.rotation.x = Math.PI / 4; // 旋转（子旋转了 90）

// 将网格添加到场景
// scene.add(cube);
scene.add(parentCube);

// 设置相机位置
camera.position.z = 5; // 蓝色
camera.position.y = 2; // 绿色
camera.position.x = 2; // 红色

camera.lookAt(0, 0, 0);

// 添加坐标器
const axesHelper = new THREE.AxesHelper(5); // 坐标系长度为 5
scene.add(axesHelper);

// 添加 轨道控制器
const controls = new OrbitControls(camera, renderer.domElement);
// 惯性
controls.enableDamping = true;

// 渲染
// renderer.render(scene, camera); 
// 渲染函数
function animate() {
    controls.update();
    requestAnimationFrame(animate);
    // 旋转
    // cube.rotation.x += 0.01;
    // cube.rotation.y += 0.01;
    renderer.render(scene, camera);
}
animate();
```
