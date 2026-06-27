官方地址：<https://emscripten.org/>

<h1 style="text-align: center;">window下Emscripten环境搭建</h1>

# 一、下载emsdk

## 1、github地址

```bash
https://github.com/emscripten-core/emsdk
```

## 2、git克隆

```bash
# SSH
git@github.com:emscripten-core/emsdk.git
# HTTPS
https://github.com/emscripten-core/emsdk.git
```

## 3、github拉太慢，提供一个下载地址

<https://wwe.lanzouw.com/i8Lwbxxxttc>

注：把解压后把文件夹名字修改为emsdk（可选）

# 二、安装Python

注：不安装Python ，你无法使用 Emscripten

## 1、使用anaconda安装

地址：<https://mirrors.tuna.tsinghua.edu.cn/anaconda/archive/?C=M&O=A>

注：下载一个新一点的（Python 版本至少3.0以后的）。

## 2、安装流程如下

示例安装版本下载地址：<https://pan.baidu.com/s/1TdEq85MOctLDvq99cHtu_A>

 ![image preview](./images/202203272341679.png)

## 3、测试是否安装成功

 ![image-20211224205142538](./images/202203272341107.png)

# 三、运行指令（cmd）

## 1、进入

```bash
cd emsdk
```

## 2、下载并安装最新的 SDK 工具

```ba
emsdk install latest
```

注：emsdk install latest 如果执行失败，就去执行3，然后在回来执行emsdk install latest，等他执行成功，再去执行3

## 3、使当前用户的“最新”SDK“处于活动状态”（写入 .emscripten 文件）

```bash
emsdk activate latest
```

## 4、看上面的是否执行成功

```bash
emcc -v
```

 ![image-20211224184722663](./images/202203272343188.png)

注：这个测试只能在当前目录下成功，所以要设置系统环境变量

# 四、设置环境变量

## 1、运行

注：找到 emsdk_env.bat 双击运行设置环境变量，但是一般情况下，会失败

双击运行 emcmdprompt.bat 获取要设置的环境变量

 ![image-20211224185342792](./images/202203272343155.png)

## 2、添加

1）电脑 -> 属性 -> 高级系统设置 -> 环境变量，在系统变量的Path值中添加：

 ![image-20211224200531141](./images/202203272343367.png)

2）电脑 -> 属性 -> 高级系统设置 -> 环境变量，在系统变量中新增如下内容：

 ![image-20211224200754878](./images/202203272343878.png)

 ![image-20211224200847110](./images/202203272343796.png)

## 3、在全局下在进行测试

```bash
emcc -v
```

# 五、测试

## 1、新建一个c++文件

```c++
#include <stdio.h>
// 一旦WASM模块被加载，main()中的代码就会执行
int main(int argc, char ** argv) {
    printf("你好\n");
}
```

## 2、编译

```bash
emcc index.c -s WASM=1 -O3 -o index.js
```

注：编译后生成下面两个文件

 ![image-20211224202501978](./images/202203272344159.png)

## 3、运行编译后的文件

注：index.js和index.wasm必须放在同一目录下，把index.js引入html中即可运行。

![image-20211224202749673](./images/202203272344985.png)
