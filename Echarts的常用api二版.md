﻿注：一些经典文章：
1、[地图标点](https://juejin.im/post/6844903989427847181)：<https://juejin.im/post/684490398942784718>

2、[地图下钻](https://juejin.im/post/6844904170512891917)：<https://juejin.im/post/684490417051289191>

3、[标点颜色](https://www.cnblogs.com/wlpower/p/6813501.html)：<https://www.cnblogs.com/wlpower/p/6813501.htm>

4、[w3cschool的详细介绍](https://www.w3cschool.cn/echarts_tutorial/echarts_tutorial-cd232da0.html):<:<:<https://www.w3cschool.cn/echarts_tutorial/echarts_tutorial-cd232da0.htm>>

# 一、Echarts

注：[官网](https://echarts.apache.org/zh/index.html)：<https://echarts.apache.org/zh/index.htm>

​       Echarts[社区](https://gallery.echartsjs.com/explore.html#sort=rank~timeframe=all~author=all)：<https://www.makeapie.com/explore.htm>

```bash
# 下载
npm install --save echarts
```

# 二、图表的常用API属性设置

注：在vue中的使用方法（常用的一种方法）

```javascript
<template>
    <div id="container" style="height:200px">

    </div>
</template>
<script>
import echarts from "echarts";
export default {
    created(){
        this.$nextTick(()=>{
            this.trigger();
        })
    },
    methods:{
        trigger(){
            // 基于准备好的dom，初始化echarts实例
            var myChart = echarts.init(document.getElementById('container'));
            // 指定图表的配置项和数据
            var option = {
                // 这个里面指定图标的样式和数据
            };
            // 使用刚指定的配置项和数据显示图表。
            myChart.setOption(option);
            window.addEventListener("resize", function() {
                myChart.resize();
            });
        }
    }
}
</script>
<style scoped>

</style>
```

## 1、柱状图

1）X轴标签的一些配置
`注：`
①[X轴的配置](https://echarts.apache.org/zh/option.html#xAxis)
②[Y轴的相关配置](https://echarts.apache.org/zh/option.html#yAxis)

```javascript
                xAxis: {
                    type: "category",
                    // 图与x轴
                    boundaryGap: true,
                    data:数据,
                    //取消x轴的辅助线，也就是 图标里面的竖线，方便读数的时候 使用
                    splitLine: {
                     show: false
                    },
                    axisLabel: {
                        fontSize:'12',
                        color: "white",
                        //X轴标签 label 做了特殊处理，防止左右溢出(也就是不显示第一个和最后一个标签)
                        formatter: (value, index) => {
                            if (index === 0 || index === xAxisData.length - 1) {
                                return "";
                            }
                            return value;
                        },
                        //showMaxLabel: true, //显示x轴的所有标签（与下面的二选一）
                        interval:0, //坐标刻度之间的显示间隔，默认就可以了（默认是不重叠）
                        rotate:20 ,  //调整数值改变倾斜的幅度（范围-90到90）
                        margin:-6 //x轴与x轴上标签的距离
                    },
                    axisLine: {
                        // x轴是否显示
                        show: false,
                        //定义x轴颜色
                        lineStyle: {
                         color: 'white'
                        }
                    },
                    //坐标轴刻度相关设置
                    axisTick: {
                        show: false,
                        alignWithLabel: true,//true 的时候有效，可以保证刻度线和标签对齐
                        lineStyle:{
                         color:'white'// x轴刻度的颜色
                        }    
                    },
                },
```

2）柱状图上方显示数据

```javascript
                series: [{
                    barWidth : 15,//柱图宽度
                    backgroundStyle: {
                        color:'#2a3a49'
                    },
                    itemStyle: {//上方显示数值
                        normal: {
                            color:'rgb(71,189,189)',
                            label: {
                                show: true, //开启显示
                                position: 'top', //在上方显示
                                textStyle: { //数值样式
                                    color: 'white',
                                    fontSize:10
                                }
                            }
                        }
                    },
                    // 点击改变柱子颜色
                    emphasis:{
                        itemStyle:{
                            color:"#276363"
                        }
                    }
                }]
```

3）x轴标签长度太长的解决办法：
![在这里插入图片描述](./images/20200826134402121.png)

```javascript
    //写在 xAxis 里
     axisLabel: {
                        // 修饰刻度标签的颜色
                        color: "rgba(255,255,255,.7)",
                        interval: 0,//这个是标签都展示，如何需要隔一个展示一个 就写1，以此类推
                        formatter: function (params) {
                            if(params.indexOf("我自己需要的内容") != -1){//能找到就是true
                                // 取得后四个
                                let ends = params.substr(params.length-4);
                                // 取得前面的
                                let start = params.substr(0,params.length - 4);
                                let and = start + "\n" + ends;
                                return and;
                            }else{
                                var newParamsName = "";// 最终拼接成的字符串
                                var paramsNameNumber = params.length;// 实际标签的个数
                                var provideNumber = 4;// 每行能显示的字的个数
                                var rowNumber = Math.ceil(paramsNameNumber / provideNumber);// 换行的话，需要显示几行，向上取整
                                /**
                                 * 判断标签的个数是否大于规定的个数， 如果大于，则进行换行处理 如果不大于，即等于或小于，就返回原标签
                                 */
                                // 条件等同于rowNumber>1
                                
                                if (paramsNameNumber > provideNumber) {
                                    /** 循环每一行,p表示行 */
                                    for (var p = 0; p < rowNumber; p++) {
                                        var tempStr = "";// 表示每一次截取的字符串
                                        var start = p * provideNumber;// 开始截取的位置
                                        var end = start + provideNumber;// 结束截取的位置
                                        // 此处特殊处理最后一行的索引值
                                        if (p == rowNumber - 1) {
                                            // 最后一次不换行
                                            tempStr = params.substring(start, paramsNameNumber);
                                        } else {
                                            // 每一次拼接字符串并换行
                                            tempStr = params.substring(start, end) + "\n";
                                        }
                                        newParamsName += tempStr;// 最终拼成的字符串
                                    }

                                } else {
                                    // 将旧标签的值赋给新标签
                                    newParamsName = params;
                                }
                                //将最终的字符串返回
                                return newParamsName
                            }
                        }
                    },
```

![在这里插入图片描述](./images/20200826134522839.png)
4）柱状图阴影指示器颜色修改

```javascript
 //文档：https://echarts.apache.org/v4/zh/option.html#tooltip
    tooltip: {
        trigger: 'axis',
        axisPointer: {
            type: 'shadow',
            //设置阴影指示器颜色
            shadowStyle:{
                color:"red"
            }
        }
    },
```

## 2、折线图

```javascript
                series: [
                    {
                        name: '',
                        symbol: "none",//取消折线图滑过的点
                        type: 'line',
                        stack: '总量',//去掉它  数量相同的折线，就可以交叉了
                        data: [数据],
                        //下面这个是设置基线的，也就是及格线
                        markLine: {
                            silent: true,
                            lineStyle: {
                                color: 'red', // 这儿设置安全基线颜色
                                normal: {
                                    type:"solid",
                                    color: 'red',
                                }
                            },
                            data: [{
                                yAxis:200//这个是及格线数字
                            }],
                        },
                        //设置折线图的 颜色
                        itemStyle : { 
                            normal : { 
                                color:'#275F82', //改变折线点的颜色
                                lineStyle:{ 
                                    color:'#253A5D' //改变折线颜色
                                } 
                            } 
                        },
                    }
                ]
            };
```

### 1）给折线图下方添加阴影

```javascript

option = {
    tooltip: {
        show: true,
        trigger: 'axis',
        axisPointer: { // 坐标轴指示器，坐标轴触发有效
            type: 'line', // 默认为直线，可选为：'line' | 'shadow'
            //修改指识线的颜色
            lineStyle: {
                color: "transparent"
            }
        },
        formatter: function(params) {
            console.log(params)
        },
    },
    grid: {
        top: 20,
        bottom: 20,
        left: 30,
        right: 20
    },
    xAxis: [{
        type: 'category',
        axisTick: {
            show: false,
            alignWithLabel: true, //true 的时候有效，可以保证刻度线和标签对齐
        },
        axisLine: {
            lineStyle: {
                color: "#889fcc"
            }
        },
        data: ['1月', '2月', '3月', '4月', '5月', '6月', '7月', '8月', '9月', '10月', '11月', '12月']
    }, ],
    yAxis: [{
        type: 'value',
        splitLine: {
            show: true,
            lineStyle: {
                color: "rgba(136, 159, 204, .2)"
            }
        },
        axisLine: {
            show: false,
            lineStyle: {
                color: "#889fcc"
            }
        },
        axisTick: {
            show: false
        },
    }],
    //手势放大柱状图折线图
    dataZoom: {
        type: "inside"
    },
    series: [{
        //给折线图下方添加阴影
        areaStyle: {
            normal: {
                color: new echarts.graphic.LinearGradient(
                    0,
                    0,
                    0,
                    1,
                    [{
                            offset: 0,
                            color: 'red'
                        },
                        {
                            offset: 1,
                            color: 'red'
                        }
                    ],
                    false
                ),
                shadowColor:'red',
                shadowBlur: 10
            }
        },
        name: '2017 降水量',
        type: 'line',
        smooth: true,
        itemStyle: {
            normal: {
                show: false,
                color: "#3282FF", //改变折线点的颜色
                lineStyle: {
                    color: "#3282FF" //改变折线颜色
                },
                label: {
                    show: false, //开启显示
                    position: 'top', //在上方显示
                    textStyle: { //数值样式
                        color: '#999999',
                        fontSize: 10
                    }
                },

            },
            emphasis: {
                show: true,
                color: "#3282FF",
                borderColor: "#ffffff",
                label: {
                    show: true, //开启显示
                    position: 'top', //在上方显示
                    textStyle: { //数值样式
                        color: '#fff',
                        fontSize: 16,
                        padding: [10, 10, 10, 10],
                        backgroundColor: "rgba(24, 71, 185, .6)",
                        borderRadius: 4,
                    }
                }
            }
        },
        data: [5.9, 6.9, 10.1, 11.7, 4.3, 6.2, 21.6, 43.6, 51.4, 10.4, 17.3, 0.7]
    }]
};
```

![image-20210325211251195](./images/NUOZqtrvTmaSb2f.webp)

## 3、移动端手势放大柱状图折线图

```javascript
    //手势放大柱状图折线图
                dataZoom:{//写在option的下一级，也就是title等的同级处。
                    type:"inside"
                },
```

## 4、点击事件

```javascript
<template>
    <div id="container" style="height:200px">

    </div>
</template>
<script>
import echarts from "echarts";
export default {
    created(){
        this.$nextTick(()=>{
            this.trigger();
        })
    },
    methods:{
        trigger(){
            var myChart = echarts.init(document.getElementById('container'));
            var option = {
                
                
            };
            myChart.setOption(option);
            myChart.getZr().on('click', function(params) {
                console.log(params);
                
            });
            window.addEventListener("resize", function() {
                myChart.resize();
            });
        }
    }
}
</script>
<style scoped>

</style>
```

## 5、案例

### 1)不同线条的折线图

![在这里插入图片描述](./images/20201126103309836.png)

```javascript
option = {
    tooltip: {
        trigger: 'axis',
        axisPointer : { // 坐标轴指示器，坐标轴触发有效
            type : 'line', // 默认为直线，可选为：'line' | 'shadow'
            //修改指识线的颜色
            lineStyle:{
                color:"transparent"
            }
        },
        backgroundColor:'rgba(255,255,255,0.4)',
        textStyle:{
          color:'black',
        },
    },
    title: {
        text:"测试",//标题内容
        left:50,//居中
        textStyle: {
            fontSize:14,//文字大小
            color: "#666666",//文字颜色
            fontWeight:300, //文字粗细
        }
    },
    legend: {
        type: 'scroll',
        right:0,//图例位置
        //设置图例形状
        icon: "roundRect",
        itemWidth: 26,  // 设置宽度
　　    itemHeight:6, // 设置高度
　　    //图例文字的样式
　　    textStyle: { 
            color: '#666666',
            fontSize: 13
        },
        data:['2015 降水量', '2016 降水量','2017 降水量']
    },
    grid: {
        top: 30,
        bottom: 50
    },
    xAxis: [
        {
            type: 'category',
            axisTick: {
                alignWithLabel: true
            },
            boundaryGap: false,
            axisLine: {
                onZero: false,
                
                lineStyle: {
                    // color: colors[2]
                }
            },
            data: ['1月', '2月', '3月', '4月', '5月', '6月', '7月', '8月', '9月', '10月', '11月', '12月']
        },
    ],
    yAxis: [
        {
            type: 'value'
        }
    ],
    //手势放大柱状图折线图
    dataZoom: {
        type: "inside"
    },
    series: [
        {
            name: '2015 降水量',
            type: 'line',
            smooth: true,
            itemStyle: {
                normal: {
                    color: "#F55912", //改变折线点的颜色
                    lineStyle: {
                        color:"#F55912" //改变折线颜色
                    },
                    label: {
                        show: true, //开启显示
                        position: 'top', //在上方显示
                        textStyle: { //数值样式
                            color: '#999999',
                            fontSize: 10
                        }
                    }
                },
                emphasis: {
                    color: "#F55912",
                    borderColor: "#ffffff",
                }
            },
            data: [2.6, 5.9, 9.0, 26.4, 28.7, 70.7, 175.6, 182.2, 48.7, 18.8, 6.0, 2.3]
        },
        {
            name: '2016 降水量',
            type: 'line',
            smooth: true,
            symbol: 'circle',
            itemStyle: {
                normal: {
                    color: "#FFCD1B", //改变折线点的颜色
                    lineStyle: {
                        color:"#FFCD1B" //改变折线颜色
                    },
                    label: {
                        show: true, //开启显示
                        position: 'top', //在上方显示
                        textStyle: { //数值样式
                            color: '#999999',
                            fontSize: 10
                        }
                    }
                },
                emphasis: {
                    color: "#FFCD1B",
                    borderColor: "#ffffff",
                }
            },
            data: [3.9, 5.9, 11.1, 18.7, 48.3, 69.2, 231.6, 46.6, 55.4, 18.4, 10.3, 0.7]
        },
        {
            name: '2017 降水量',
            type: 'line',
            smooth: true,
            itemStyle: {
                normal: {
                    color: "#3282FF", //改变折线点的颜色
                    lineStyle: {
                        color: "#3282FF" //改变折线颜色
                    },
                    label: {
                        show: true, //开启显示
                        position: 'top', //在上方显示
                        textStyle: { //数值样式
                            color: '#999999',
                            fontSize: 10
                        }
                    }
                },
                emphasis: {
                    color: "#3282FF",
                    borderColor: "#ffffff",
                }
            },
            data: [5.9, 6.9, 10.1, 11.7, 4.3, 6.2, 21.6, 43.6, 51.4, 10.4, 17.3, 0.7]
        }
    ]
};
```

### 2）在vue中的使用

![在这里插入图片描述](./images/2020112610391870.png)

```javascript
<!--
 * @Author:
 * @Date: 2020-11-25 18:28:15
 * @LastEditors:
 * @LastEditTime: 2020-11-26 10:37:26
 * @FilePath: 
 * @Description: 
-->
<template>
    <div ref="threeLine" class="threeLine">

    </div>
</template>
<script>
import echarts from "echarts";
export default {
    mounted() {
        this.thread();
    },
    methods: {
        thread() {
            var myChart = echarts.init(this.$refs.threeLine);
            var option = {
                tooltip: {
                    trigger: 'axis',
                    axisPointer: { // 坐标轴指示器，坐标轴触发有效
                        type: 'line', // 默认为直线，可选为：'line' | 'shadow'
                        //修改指识线的颜色
                        lineStyle: {
                            color: "transparent"
                        }
                    },
                    backgroundColor: 'rgba(255,255,255,0.4)',
                    textStyle: {
                        color: 'black',
                        fontSize: 12,
                        fontFamily: "PingFang SC"
                    },
                    extraCssText: 'fontSize:22px'
                },
                title: {
                    text: "测试",//标题内容
                    left: 0,//居中
                    textStyle: {
                        fontSize: 14,//文字大小
                        color: "#666666",//文字颜色
                        fontWeight: 300, //文字粗细
                    }
                },
                legend: {
                    type: 'scroll',
                    right: 0,//图例位置
                    //设置图例形状
                    icon: "roundRect",
                    itemWidth: 26,  // 设置宽度
                    itemHeight: 6, // 设置高度
                    //图例文字的样式
                    textStyle: {
                        color: '#666666',
                        fontSize: 13
                    },
                    data: ['A相', 'B相', 'C相']
                },
                grid: {
                    top: 30,
                    bottom: 20,
                    right:5
                },
                xAxis: [
                    {
                        type: 'category',
                        axisTick: {
                            alignWithLabel: true
                        },
                        boundaryGap: false,
                        axisLine: {
                            onZero: false,

                            lineStyle: {
                                // color: colors[2]
                            }
                        },
                        axisTick: {
                            alignWithLabel: true
                        },
                        axisLine: {
                            show: false
                        },
                        axisTick: {
                            show: false
                        },
                        //修改X轴滑过时候的样式
                        /*
                        axisPointer: {
                            label: {
                                formatter: function (params) {
                                    return '降水量  ' + params.value
                                        + (params.seriesData.length ? '：' + params.seriesData[0].data : '');
                                }
                            }
                        },*/
                        data: ['1月', '2月', '3月', '4月', '5月', '6月', '7月', '8月', '9月', '10月', '11月', '12月']
                    },
                ],
                yAxis: [
                    {
                        type: 'value',
                        // 设置是否有辅助线
                        splitLine: {
                            show: false
                        },
                        axisLine: {
                            show: false
                        },
                        axisTick: {
                            show: false
                        },
                        axisLabel: {
                            show: true
                        }
                    }
                ],
                //手势放大柱状图折线图
                dataZoom: {
                    type: "inside"
                },
                series: [
                    {
                        name: 'A相',
                        type: 'line',
                        smooth: true,
                        itemStyle: {
                            normal: {
                                color: "#F55912", //改变折线点的颜色
                                lineStyle: {
                                    color: "#F55912" //改变折线颜色
                                },
                                // 这个是在折线处，上方展示数值
                                /*
                                label: {
                                    show: true, //开启显示
                                    position: 'top', //在上方显示
                                    textStyle: { //数值样式
                                        color: '#999999',
                                        fontSize: 10
                                    }
                                }
                                */
                            },
                            emphasis: {
                                color: "#F55912",
                                borderColor: "#ffffff",
                            }
                        },
                        data: [2.6, 5.9, 9.0, 26.4, 28.7, 70.7, 175.6, 182.2, 48.7, 18.8, 6.0, 2.3]
                    },
                    {
                        name: 'B相',
                        type: 'line',
                        smooth: true,
                        symbol: 'circle',
                        itemStyle: {
                            normal: {
                                color: "#FFCD1B", //改变折线点的颜色
                                lineStyle: {
                                    color: "#FFCD1B" //改变折线颜色
                                },

                            },
                            emphasis: {
                                color: "#FFCD1B",
                                borderColor: "#ffffff",
                            }
                        },
                        data: [3.9, 5.9, 11.1, 18.7, 48.3, 69.2, 231.6, 46.6, 55.4, 18.4, 10.3, 0.7]
                    },
                    {
                        name: 'C相',
                        type: 'line',
                        smooth: true,
                        itemStyle: {
                            normal: {
                                color: "#3282FF", //改变折线点的颜色
                                lineStyle: {
                                    color: "#3282FF" //改变折线颜色
                                },
                            },
                            emphasis: {
                                color: "#3282FF",
                                borderColor: "#ffffff",
                            }
                        },
                        data: [5.9, 6.9, 10.1, 11.7, 4.3, 6.2, 21.6, 43.6, 51.4, 10.4, 17.3, 0.7]
                    }
                ]
            };
            myChart.setOption(option);
            window.addEventListener("resize", function () {
                myChart.resize();
            });
        }
    }
}
</script>
<style scoped>
.threeLine {
    height: 477px;
}
</style>
```

### 3)有警戒线的柱状图

![在这里插入图片描述](./images/20200916163634511.png)

```javascript
<template>
    <div>
        <div id="apparatus"></div>
    </div>
</template>
<script>
import echarts from "echarts";
export default {
    mounted() {

        this.equipment();

    },
    methods: {
        equipment() {
            var myChart = echarts.init(document.getElementById('apparatus'));
            var option = {
                title: {
                    text: "装置在线率",//标题内容
                    left: "center",//居中
                    textStyle: {
                        fontSize: 19,//文字大小
                        color: "#F3D281",//文字颜色
                        fontWeight: 'normal'//文字粗细
                    }
                },
                // 划过显示 提示框(指示器)
                tooltip: {
                    trigger: 'axis',
                    axisPointer: {  // 坐标轴指示器，坐标轴触发有效
                        type: 'shadow' // 默认为直线，可选为：'line' | 'shadow'
                    },
                    formatter: function (params) {
                        return params[0].name + '：<br/>' + params[0].value;
                    }
                },
                xAxis: {
                    type: 'category',
                    axisLine: {
                        //定义x轴颜色
                        lineStyle: {
                            color: 'white'
                        }
                    },
                    data: ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun']
                },
                yAxis: {
                    type: 'value',
                    axisLine: {
                        //定义x轴颜色
                        lineStyle: {
                            color: 'white'
                        }
                    },
                    // 取消y轴辅助线
                    splitLine: {
                        show: false
                    },
                },
                series: [{
                    data: [120, 200, 150, 80, 70, 110, 130],
                    type: 'bar',
                    // 控制柱状图  是否展示整个高
                    // showBackground: true,
                    itemStyle: {
                        normal: {
                            color: "#34BEF8",
                            // 随机显示
                            //color:function(d){return "#"+Math.floor(Math.random()*(256*256*256-1)).toString(16);}

                            // 定制显示（按顺序）
                            // color: function (params) {
                            //     var colorList = ['#C33531', '#EFE42A', '#64BD3D', '#EE9201', '#29AAE3', '#B74AE5', '#0AAF9F', '#E89589', '#16A085', '#4A235A', '#C39BD3 ', '#F9E79F', '#BA4A00', '#ECF0F1', '#616A6B', '#EAF2F8', '#4A235A', '#3498DB'];
                            //     return colorList[params.dataIndex]
                            // },
                            label: {
                                show: true, //开启显示
                                position: 'top', //在上方显示
                                textStyle: { //数值样式
                                    color: '#34BEF8',
                                    fontSize: 10
                                }
                            }
                        },
                    },
                    emphasis: {
                        itemStyle: {
                            color: "#276363"
                        }
                    },
                    markLine: {
                        symbol: "none", //去掉警戒线最后面的箭头
                        label: {
                            position: "end", //将警示值放在哪个位置，三个值“start”,"middle","end"  开始  中点 结束
                            formatter: "200"
                        },
                        //警示线的样式，虚实  颜色
                        lineStyle: {
                            type: "solid",
                            color: "#FF4B5C",
                        },
                        data: [{
                            silent: false, //鼠标悬停事件  true没有，false有
                            lineStyle: { //警戒线的样式  ，虚实  颜色
                                type: "solid",
                                color: "rgba(238, 99, 99)"
                            },
                            name: '',
                            yAxis: 200
                        }]
                    }
                }]


            };
            myChart.setOption(option);
            window.addEventListener("resize", function () {
                myChart.resize();
            });
        }
    }
}
</script>
<style scoped>
#apparatus {
    height: 300px;
}
</style>
```

### 4)平滑的折线图

![在这里插入图片描述](./images/20200916155027447.png)

```javascript
<template>
    <div>
        <div id="container_four"></div>
    </div>
</template>
<script>
import echarts from "echarts";
export default {
    mounted() {
        this.trigger();
    },
    methods: {
        trigger() {
            var myChart = echarts.init(document.getElementById('container_four'));
            var option = {
                backgroundColor: '#343332',
                xAxis: {
                    type: 'category',
                    // X轴标签对准刻度线的属性
                    boundaryGap: false,
                    data: ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun'],
                    axisLine: {
                        // x轴是否显示
                        show:true,
                        //定义x轴颜色
                        lineStyle: {
                         color: 'white'
                        }
                    },
                    
                },
                yAxis: {
                    type: 'value',
                    axisLine: {
                        //定义x轴颜色
                        lineStyle: {
                         color: 'white'
                        }
                    },
                    // 取消y轴辅助线
                    splitLine: {
                     show: false
                    },
                },
                series: [{
                    symbol: "none",
                    data: [820, 932, 901, 934, 1290, 1330, 1320],
                    type: 'line',
                    smooth: true,
                    itemStyle:{
                        normal:{
                            lineStyle:{
                                color:"#31B4EF"
                            }
                        }
                    },
                    // 设置折线图，下方的 颜色
                    areaStyle: {
                        color:"#33464E",
                        opacity:.4
                    }
                }]
            };
            myChart.setOption(option);
            window.addEventListener("resize", function () {
                myChart.resize();
            });
        }
    },
}
</script>
<style scoped>
#container_four{
    width:100%;
    height:300px;
}
</style>
```

# 三、公用api

## 1、设置图例位置与点击事件

```javascript
                legend: {
                    bottom:0,//图例位置
                    selectedMode:false,//取消图例上的点击事件
                    textStyle: { //图例文字的样式
                     color: '#fff'
                    },
                },
```

## 2、根据屏幕大小去响应图表大小

```javascript
   window.addEventListener("resize", function() {
                myChart.resize();
            });
```

![在这里插入图片描述](./images/20200721103053230.png )

## 3、设置标题

```javascript
   //在option里面（即是在指定图表的配置项和数据）
   title: {
             text: "",//标题内容
             left:"center",//居中
            textStyle: {
              fontSize:16,//文字大小
              color:"white",//文字颜色
              fontWeight:'normal'//文字粗细
             }
            },
```

## 4、图表位置

```javascript
                grid: {
                    top: "16%",
                    left: "center",
                    right: "",
                    bottom: "0%",
                    show: true,//边框
                    borderColor: "#012f4a",
                    containLabel: true,//
                },
```

## 5、放大图例

```javascript
//在option的下一级
                //手势放大柱状图折线图、折线图等（鼠标也可以）
                dataZoom:{
                    type:"inside"
                },
```

## 6、滑过的提示框

```javascript
    tooltip: {
        show: false,//隐藏提示框
        trigger: 'axis',
        // 设置提示线
        // axisPointer: {
        //     type: 'cross',//设置为none时，隐藏提示线
               // 设置提示线颜色
        //     crossStyle: {
        //         color: '#999'
        //     },
        //     lineStyle: {
        //         type: 'dashed'
        //     }
        // },
        backgroundColor: '#5964E4FF', //背景颜色（此时为默认色）
        borderRadius: 36, //边框圆角
        padding: [2, 20], //提示框大小
    },
```

## 7、设置x或者y的辅助线

```javascript
yAxis: {
    // 设置辅助线
    splitLine: {
        show: true,
        lineStyle: {
         color: 'rgba(46, 105, 205, .4)'
        }
    },
}
```

# 四、Echarts_Map的使用

## 1、只能到省级

下载echarts：
`npm install --save echarts`

### 1）省级地图的使用

![在这里插入图片描述](./images/20200729154655939.png)

注：引用各个省的地图，在==node_modules/echarts/map/js==里去找。
![在这里插入图片描述](./images/20200729155105931.png )

```javascript
<!--
 * @Descripttion: (对该文件的信息描述) 
 * @version: (版本)
 * @Author: (作者) 
 * @Date: (文件创建时间)  
 * @LastEditors: (最后更新作者)
 * @LastEditTime: (最后更新时间)
--> 
<template>
    <div id="economizes">
        
    </div>
</template>
<script>
import echarts from "echarts";
import 'echarts/map/js/province/shanxi1.js';
export default {
    created(){
        this.$nextTick(()=>{
            this.province();
        })
    },
    methods:{
        province(){
            var myChart = echarts.init(document.getElementById('economizes'));
            var series = [];
            var option = {
                backgroundColor: '',
                title : {
                    text: '',
                    subtext: '',
                    left: 'center',
                    textStyle : {
                        color: '#fff'
                    }
                },
                geo: {
                    map:"陕西",//这块是重点，在node_modules/echarts/map/js里面去看，你引入的那个，就去看那个的名字 registerMap后面 紧跟的 第一个 就是名字
                    label: {
                        // 显示市区的名字，直接显示，不用划过在显示
                        normal: {
                            show: false,
                            color:'rgba(255, 255, 255, 0.5)'
                        },
                        emphasis: {
                            show: true,
                            color:'#fff'
                        }
                    },
                    // 把中国地图放大了    倍
                    zoom:1.2,
                    roam: false,//这里设置是否允许，放大 我们的地图
                    itemStyle: {
                        normal: {
                            // 地图省份的背景颜色
                            areaColor: 'rgba(20, 41, 87,0.6)',
                            borderColor: 'rgba(43, 196, 243, 1)',
                            borderWidth: 1,
                        },
                        emphasis: {
                            areaColor: '#2B91B7'
                        }
                    }
                },
                series: series      
            };
            myChart.setOption(option);
            window.addEventListener("resize", function() {
                myChart.resize();
            });
        
        }
    }
}
</script>
<style lang="scss" scoped>
    #economizes{
        height:6.5rem;
    }
</style>
```

### 2)散点图

```javascript
<!--
 * @Descripttion: (对该文件的信息描述) 
 * @version: (版本)
 * @Author: (作者) 
 * @Date: (文件创建时间)  
 * @LastEditors: 李勇
 * @LastEditTime: 2020-08-29 15:54:44
--> 
<template>
    <div id="economizes">
        
    </div>
</template>
<script>
import echarts from "echarts";
import 'echarts/map/js/province/jiangxi.js';
export default {
    created(){
        this.$nextTick(()=>{
            this.province();
        })
    },
    methods:{
        province(){
            var myChart = echarts.init(document.getElementById('economizes'));
            var option = {
                backgroundColor: '',
                title : {
                    text: '',
                    subtext: '',
                    left: 'center',
                    textStyle : {
                        color: '#fff'
                    }
                },
                tooltip: {
                    trigger: 'item'
                },
                geo: {
                    map:"江西",//这块是重点，在node_modules/echarts/map/js里面去看，你引入的那个，就去看那个的名字 registerMap后面 紧跟的 第一个 就是名字
                    label: {
                        // 显示市区的名字，直接显示，不用划过在显示
                        normal: {
                            show: true,
                            color:'rgba(255, 255, 255, 0.5)'
                        },
                        emphasis: {
                            show: true,
                            color:'rgba(255, 255, 255, 0.5)'
                        }
                    },
                    // 把中国地图放大了    倍
                    zoom:1.2,
                    roam:true,//这里设置是否允许，放大 我们的地图
                    itemStyle: {
                        normal: {
                            // 地图省份的背景颜色
                            areaColor: 'rgba(20, 41, 87,0.6)',
                            borderColor: 'rgba(43, 196, 243, 1)',
                            borderWidth: 1,
                        },
                        emphasis: {
                            areaColor: ''
                        }
                    }
                },
                series:[
                    
                    {
                        type: 'effectScatter',
                        coordinateSystem: 'geo',
                        data: [{
                            name: '',
                            value:[115.89, 28.48]
                        }, {
                            name: '',
                            value: [117.28, 29.09]
                        }],
                        symbolSize: function (val) {
                            return 8;
                        },
                        itemStyle: {
                            // 点的颜色
                            color: 'rgb(38, 135, 239)',
                            shadowBlur:5,
                        },
                    }
                ]
            };
            myChart.setOption(option);
            window.addEventListener("resize", function() {
                myChart.resize();
            });
        
        }
    }
}
</script>
<style scoped>
    #economizes{
        height:520px;
    }
</style>
```

![在这里插入图片描述](<(<https://img-blog.csdnimg.cn/20200829155641225.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ1NjY5MTc4,size_16,color_FFFFFF,t_7>> =150x180)

### 3）全国地图的使用

参考社区的[例子](https://gallery.echartsjs.com/editor.html?c=x0-ExSkZDM)：[https://gallery.echartsjs.com/editor.html?c=x0-ExSkZDM](https://gallery.echartsjs.com/editor.html?c=x0-ExSkZDM)  (模拟飞机航线)
![在这里插入图片描述](<(<https://img-blog.csdnimg.cn/20200729140725958.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ1NjY5MTc4,size_16,color_FFFFFF,t_7>> =200x150)
注：[背景动画](https://wws.lanzous.com/b01hk520b)图片见:<:<:<https://wws.lanzous.com/b01hk520>>
`不要乱下载经纬度编码，echarts依赖包里面就有。`

```javascript
<template>
    <div>
        <div class="map">
            <!-- 这块写地图 -->
            <div id="chartMap" style="height:8rem;zIndex:9">
                
            </div>
            <!-- 下面的是动画  操作，可以  不用在意 -->
            <div class="map1"></div>
            <div class="map2"></div>
            <div class="map3"></div>
        </div>
    </div>
</template>
<script>
import echarts from "echarts";
import 'echarts/map/js/china.js';//这个在echarts 的依赖包里，不用单独下载
export default {
    components:{
        
    },
    data() {
        return {

        }
    },
    created(){
        this.$nextTick(()=>{
            this.structure();
        })
    },
    mounted() {

    },
    methods:{
        structure(){
            var myChart = echarts.init(document.getElementById('chartMap'));
                var geoCoordMap = {
                    '上海': [121.4648,31.2891],
                    '东莞': [113.8953,22.901],
                    '东营': [118.7073,37.5513],
                    '中山': [113.4229,22.478],
                    '临汾': [111.4783,36.1615],
                    '临沂': [118.3118,35.2936],
                    '丹东': [124.541,40.4242],
                    '丽水': [119.5642,28.1854],
                    '乌鲁木齐': [87.9236,43.5883],
                    '佛山': [112.8955,23.1097],
                    '保定': [115.0488,39.0948],
                    '兰州': [103.5901,36.3043],
                    '包头': [110.3467,41.4899],
                    '北京': [116.4551,40.2539],
                    '北海': [109.314,21.6211],
                    '南京': [118.8062,31.9208],
                    '南宁': [108.479,23.1152],
                    '南昌': [116.0046,28.6633],
                    '南通': [121.1023,32.1625],
                    '厦门': [118.1689,24.6478],
                    '台州': [121.1353,28.6688],
                    '合肥': [117.29,32.0581],
                    '呼和浩特': [111.4124,40.4901],
                    '咸阳': [108.4131,34.8706],
                    '哈尔滨': [127.9688,45.368],
                    '唐山': [118.4766,39.6826],
                    '嘉兴': [120.9155,30.6354],
                    '大同': [113.7854,39.8035],
                    '大连': [122.2229,39.4409],
                    '天津': [117.4219,39.4189],
                    '太原': [112.3352,37.9413],
                    '威海': [121.9482,37.1393],
                    '宁波': [121.5967,29.6466],
                    '宝鸡': [107.1826,34.3433],
                    '宿迁': [118.5535,33.7775],
                    '常州': [119.4543,31.5582],
                    '广州': [113.5107,23.2196],
                    '廊坊': [116.521,39.0509],
                    '延安': [109.1052,36.4252],
                    '张家口': [115.1477,40.8527],
                    '徐州': [117.5208,34.3268],
                    '德州': [116.6858,37.2107],
                    '惠州': [114.6204,23.1647],
                    '成都': [103.9526,30.7617],
                    '扬州': [119.4653,32.8162],
                    '承德': [117.5757,41.4075],
                    '拉萨': [91.1865,30.1465],
                    '无锡': [120.3442,31.5527],
                    '日照': [119.2786,35.5023],
                    '昆明': [102.9199,25.4663],
                    '杭州': [119.5313,29.8773],
                    '枣庄': [117.323,34.8926],
                    '柳州': [109.3799,24.9774],
                    '株洲': [113.5327,27.0319],
                    '武汉': [114.3896,30.6628],
                    '汕头': [117.1692,23.3405],
                    '江门': [112.6318,22.1484],
                    '沈阳': [123.1238,42.1216],
                    '沧州': [116.8286,38.2104],
                    '河源': [114.917,23.9722],
                    '泉州': [118.3228,25.1147],
                    '泰安': [117.0264,36.0516],
                    '泰州': [120.0586,32.5525],
                    '济南': [117.1582,36.8701],
                    '济宁': [116.8286,35.3375],
                    '海口': [110.3893,19.8516],
                    '淄博': [118.0371,36.6064],
                    '淮安': [118.927,33.4039],
                    '深圳': [114.5435,22.5439],
                    '清远': [112.9175,24.3292],
                    '温州': [120.498,27.8119],
                    '渭南': [109.7864,35.0299],
                    '湖州': [119.8608,30.7782],
                    '湘潭': [112.5439,27.7075],
                    '滨州': [117.8174,37.4963],
                    '潍坊': [119.0918,36.524],
                    '烟台': [120.7397,37.5128],
                    '玉溪': [101.9312,23.8898],
                    '珠海': [113.7305,22.1155],
                    '盐城': [120.2234,33.5577],
                    '盘锦': [121.9482,41.0449],
                    '石家庄': [114.4995,38.1006],
                    '福州': [119.4543,25.9222],
                    '秦皇岛': [119.2126,40.0232],
                    '绍兴': [120.564,29.7565],
                    '聊城': [115.9167,36.4032],
                    '肇庆': [112.1265,23.5822],
                    '舟山': [122.2559,30.2234],
                    '苏州': [120.6519,31.3989],
                    '莱芜': [117.6526,36.2714],
                    '菏泽': [115.6201,35.2057],
                    '营口': [122.4316,40.4297],
                    '葫芦岛': [120.1575,40.578],
                    '衡水': [115.8838,37.7161],
                    '衢州': [118.6853,28.8666],
                    '西宁': [101.4038,36.8207],
                    '西安': [109.1162,34.2004],
                    '贵阳': [106.6992,26.7682],
                    '连云港': [119.1248,34.552],
                    '邢台': [114.8071,37.2821],
                    '邯郸': [114.4775,36.535],
                    '郑州': [113.4668,34.6234],
                    '鄂尔多斯': [108.9734,39.2487],
                    '重庆': [107.7539,30.1904],
                    '金华': [120.0037,29.1028],
                    '铜川': [109.0393,35.1947],
                    '银川': [106.3586,38.1775],
                    '镇江': [119.4763,31.9702],
                    '长春': [125.8154,44.2584],
                    '长沙': [113.0823,28.2568],
                    '长治': [112.8625,36.4746],
                    '阳泉': [113.4778,38.0951],
                    '青岛': [120.4651,36.3373],
                    '韶关': [113.7964,24.7028]
                };

                var XAData = [
                    [{name:'西安'}, {name:'北京',value:100}],
                    [{name:'西安'}, {name:'上海',value:100}],
                    [{name:'西安'}, {name:'广州',value:100}],
                    [{name:'西安'}, {name:'西宁',value:100}],
                    [{name:'西安'}, {name:'银川',value:100}]
                ];

                var XNData = [
                    [{name:'西宁'}, {name:'北京',value:100}],
                    [{name:'西宁'}, {name:'上海',value:100}],
                    [{name:'西宁'}, {name:'广州',value:100}],
                    [{name:'西宁'}, {name:'西安',value:100}],
                    [{name:'西宁'}, {name:'银川',value:100}]
                ];

                var YCData = [
                    [{name:'银川'}, {name:'北京',value:100}],
                    [{name:'银川'}, {name:'广州',value:100}],
                    [{name:'银川'}, {name:'上海',value:100}],
                    [{name:'银川'}, {name:'西安',value:100}],
                    [{name:'银川'}, {name:'西宁',value:100}],
                ];
                //这个是小飞机的图标，根据需求替换
                var planePath = 'path://M1705.06,1318.313v-89.254l-319.9-221.799l0.073-208.063c0.521-84.662-26.629-121.796-63.961-121.491c-37.332-0.305-64.482,36.829-63.961,121.491l0.073,208.063l-319.9,221.799v89.254l330.343-157.288l12.238,241.308l-134.449,92.931l0.531,42.034l175.125-42.917l175.125,42.917l0.531-42.034l-134.449-92.931l12.238-241.308L1705.06,1318.313z';
                //var planePath = 'arrow';
                var convertData = function (data) {
                    var res = [];
                    for (var i = 0; i < data.length; i++) {
                        
                        var dataItem = data[i];

                        var fromCoord = geoCoordMap[dataItem[0].name];
                        var toCoord = geoCoordMap[dataItem[1].name];
                        if (fromCoord && toCoord) {
                            res.push({
                                fromName: dataItem[0].name,
                                toName: dataItem[1].name,
                                coords: [fromCoord, toCoord],
                                value: dataItem[1].value
                            });
                        }
                    }
                    return res;
                };

                var color = ['#a6c84c', '#ffa022', '#46bee9'];//航线的颜色
                var series = [];
                [['西安', XAData], ['西宁', XNData], ['银川', YCData]].forEach(function (item, i) {  
                    series.push({
                        name: item[0] + ' Top3',
                        type: 'lines',
                        zlevel: 1,
                        effect: {
                            show: true,
                            period: 6,
                            trailLength: 0.7,
                            color: 'red',   //arrow箭头的颜色
                            symbolSize: 3
                        },
                        lineStyle: {
                            normal: {
                                color: color[i],
                                width: 0,
                                curveness: 0.2
                            }
                        },
                        data: convertData(item[1])
                    },
                    {
                        name: item[0] + ' Top3',
                        type: 'lines',
                        zlevel: 2,
                        symbol: ['none', 'arrow'],
                        symbolSize: 10,
                        effect: {
                            show: true,
                            period: 6,
                            trailLength: 0,
                            symbol: planePath,
                            symbolSize: 15
                        },
                        lineStyle: {
                            normal: {
                                color: color[i],
                                width: 1,
                                opacity: 0.6,
                                curveness: 0.2
                            }
                        },
                        data: convertData(item[1])
                    },
                    {
                        name: item[0] + ' Top3',
                        type: 'effectScatter',
                        coordinateSystem: 'geo',
                        zlevel: 2,
                        rippleEffect: {
                            brushType: 'stroke'
                        },
                        label: {
                            normal: {
                                show: true,
                                position: 'right',
                                formatter: '{b}'
                            }
                        },
                        symbolSize: function (val) {
                            return val[2] / 8;
                        },
                        itemStyle: {
                            normal: {
                                color: color[i],
                            },
                            emphasis: {
                                areaColor: '#2B91B7'
                            }
                        },
                        data: item[1].map(function (dataItem) {
                            return {
                                name: dataItem[1].name,
                                value: geoCoordMap[dataItem[1].name].concat([dataItem[1].value])
                            };
                        })
                    });
                });
            var option = {
                backgroundColor: '',
                title : {
                    text: '',
                    subtext: '',
                    left: 'center',
                    textStyle : {
                        color: '#fff'
                    }
                },
                tooltip : {
                    trigger: 'item', 
                    formatter:function(params, ticket, callback){
                        if(params.seriesType=="effectScatter") {
                            return "线路："+params.data.name+""+params.data.value[2];
                        }else if(params.seriesType=="lines"){
                            return params.data.fromName+">"+params.data.toName+"<br />"+params.data.value;
                        }else{
                            return params.name;
                        }
                    } 
                },
                legend: {
                    orient: 'vertical',
                    top: 'bottom',
                    left: 'right',
                    data:['西安 Top3', '西宁 Top3', '银川 Top3'],
                    textStyle: {
                        color: '#fff'
                    },
                    selectedMode: 'multiple'
                },
                geo: {
                    map: 'china',
                    label: {
                        emphasis: {
                            show: true,
                            color:'#fff'
                        }
                    },
                    // 把中国地图放大了1.2倍
                    zoom:1,
                    roam: true,//这里设置是否允许，放大 我们的地图
                    itemStyle: {
                        normal: {
                            // 地图省份的背景颜色
                            areaColor: 'rgba(20, 41, 87,0.6)',
                            borderColor: 'rgba(43, 196, 243, 1)',
                            borderWidth: 1,
                        },
                        emphasis: {
                            areaColor: '#2B91B7'
                        }
                    }
                },
                series: series
            };
            myChart.setOption(option);
            // 监听浏览器缩放
            window.addEventListener("resize", function() {
                myChart.resize();
            });
        }
    }
};
</script>
<style lang="scss" scoped>
 //这块使用了scss语法，响应式布局 使用了amfe-flexible，所以下面用了rem
    .map {
        position: relative;
        height:8rem;
        .chart {
            position: absolute;
            top: 0;
            left: 0;
            z-index: 5;
            height: 10.125rem;
            width: 100%;
        }
        .map1,
        .map2,
        .map3 {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            width: 6.475rem;
            height: 6.475rem;
            background: url("~@/assets/images/map.png") no-repeat;
            background-size: 100% 100%;
            opacity: 0.3;
        }
        .map2 {
            width: 8.0375rem;
            height: 8.0375rem;
            background-image: url("~@/assets/images/lbx.png");
            opacity: 0.6;
            animation: rotate 15s linear infinite;
            z-index: 2;
        }
        .map3 {
            width: 7.075rem;
            height: 7.075rem;
            background-image: url("~@/assets/images/jt.png");
            animation: rotate1 10s linear infinite;
        }

        @keyframes rotate {
            from {
                transform: translate(-50%, -50%) rotate(0deg);
            }
            to {
                transform: translate(-50%, -50%) rotate(360deg);
            }
        }
        @keyframes rotate1 {
            from {
                transform: translate(-50%, -50%) rotate(0deg);
            }
            to {
                transform: translate(-50%, -50%) rotate(-360deg);
            }
        }
    }
</style>
```

## 2、可以下钻的地图，最小到县(基于高德地图)

![在这里插入图片描述](./images/20200818194355679.png)

1）在public——>index.html中引入：

```html
<script src='http://webapi.amap.com/maps?v=1.4.15&key=cb6152d432c370a781b4b2a80a84e9a6&plugin=AMap.DistrictSearch'></script>
<script src="//webapi.amap.com/ui/1.1/main.js"></script>
```

2）在vue.config.js中引入：

```javascript
module.exports = {
    configureWebpack: {
        // 引入高德  地图
        externals: {
            AMap: "window.AMap",
            AMapUI:'window.AMapUI'
        },
        //在这块配置别名
        resolve: {
            alias: {
                '@': resolve('src')
            }
        }
    },
}
```

3）在页面中进行使用

```javascript
<template>
    <div class="echarts">
        <div class="echarts" ref="sctterMap"></div>
        <!--  -->
        <p class="back" @click.stop="backTop">返回</p>
    </div>
</template>

<script>
import echarts from 'echarts'
export default {
    name: '',
    data() {
        return {
            myCharts: null,
            parentJson: [],
            geoJsonData: {},
            mapData: [],
            parentInfo: [
                {
                    cityName: '全国',
                    level: 'china',
                    code: 100000,
                },
            ],
        }
    },
    mounted() {
        this.getGeoJson(100000);
    },
    methods: {
        //获取geoJson数据
        getGeoJson(adcode) {
            let that = this
            AMapUI.loadUI(['geo/DistrictExplorer'], (DistrictExplorer) => {
                var districtExplorer = (window.districtExplorer = new DistrictExplorer({
                    eventSupport: true, //打开事件支持
                }))
                districtExplorer.loadAreaNode(adcode, function (error, areaNode) {
                    if (error) {
                        console.error(error)
                        return
                    }
                    let Json = areaNode.getSubFeatures()
                    if (Json.length > 0) {
                        that.parentJson = Json
                    } else if (Json.length === 0) {
                        Json = that.parentJson.filter((item) => {
                            if (item.properties.adcode == adcode) {
                                return item
                            }
                        })
                        if (Json.length === 0) return
                    }
                    that.geoJsonData = {
                        features: Json,
                    }
                    that.getMapData()
                })
            })
        },
        //获取数据
        getMapData() {
            this.mapData = this.geoJsonData.features.map((item) => {
                return {
                    name: item.properties.name,
                    value: Math.random() * 1000,
                    level: item.properties.level,
                    cityCode: item.properties.adcode,
                }
            })
            //去渲染echarts
            this.initEcharts()
        },
        initEcharts() {
            this.myChart = echarts.init(this.$refs.sctterMap)
            window.addEventListener('resize', () => {
                this.myChart.resize()
            })
            echarts.registerMap('Map', this.geoJsonData) //注册
            this.myChart.setOption(
                {
                    backgroundColor: '#050038',
                    tooltip: {
                        trigger: 'item',
                        formatter: (p) => {
                            let val = p.value
                            if (window.isNaN(val)) {
                                val = 0
                            }
                            // 这块展示  你划过的时候，显示的内容
                            let txtCon = p.name + ':' + val.toFixed(2)
                            return txtCon
                        },
                    },
                    title: {
                        show: true,
                        x: 'center',
                        y: 'top',
                        text:
                            this.parentInfo[this.parentInfo.length - 1].cityName +
                            '地图实现点击下钻',
                        textStyle: {
                            color: 'rgb(97, 142, 205)',
                            fontSize: 16,
                        },
                    },
                    //这里可以添加echarts内置的，例如下载图片等
                    toolbox: {
                        feature: {
                            dataView: {
                                show: false,
                            },
                            magicType: {
                                show: false,
                            },
                            restore: {
                                show: false,
                            },
                            saveAsImage: {
                                show: true,
                                name:
                                    this.parentInfo[this.parentInfo.length - 1].cityName + '地图',
                                pixelRatio: 2,
                            },
                        },
                        iconStyle: {
                            normal: {
                                borderColor: '#41A7DE',
                            },
                        },
                        itemSize: 15,
                        top: 20,
                        right: 22,
                    },
                    dataRange: {
                        right: '2%',
                        bottom: '3%',
                        icon: 'circle',
                        align: 'left',
                        splitList: [
                            {
                                start: 0,
                                end: 0,
                                label: '未发生',
                                color: '#6ead51',
                            },
                            {
                                start: 0,
                                end: 250,
                                label: '0-150',
                                color: '#92b733',
                            },
                            {
                                start: 250,
                                end: 500,
                                label: '250-500',
                                color: '#c4aa29',
                            },
                            {
                                start: 500,
                                end: 750,
                                label: '500-750',
                                color: '#ce6c2b',
                            },
                            {
                                start: 750,
                                label: '750以上',
                                color: '#c92626',
                            },
                        ],
                        textStyle: {
                            color: '#0fccff',
                            fontSize: 16,
                        },
                    },
                    series: [
                        {
                            name: '地图',
                            type: 'map',
                            map: 'Map',
                            roam: true, //是否可缩放
                            zoom: 1.15, //缩放比例
                            data: this.mapData,
                            itemStyle: {
                                normal: {
                                    show: true,
                                    areaColor: 'rgba(0,0,0,0)',
                                    borderColor: 'rgb(185, 220, 227)',
                                    borderWidth: '1',
                                },
                            },
                            label: {
                                normal: {
                                    show: true, //显示省份标签
                                    textStyle: {
                                        color: 'rgb(249, 249, 249)', //省份标签字体颜色
                                        fontSize: 12,
                                    },
                                },
                                emphasis: {
                                    //对应的鼠标悬浮效果
                                    show: true,
                                    textStyle: {
                                        color: '#000',
                                    },
                                },
                            },
                        },
                    ],
                },
                true
            )
            let that = this
            this.myChart.off('click')
            this.myChart.on('click', (params) => {
                if (
                    that.parentInfo[that.parentInfo.length - 1].code ==
                    params.data.cityCode
                ) {
                    return
                }
                let data = params.data
                that.parentInfo.push({
                    cityName: data.name,
                    level: data.level,
                    code: data.cityCode,
                })
                that.getGeoJson(data.cityCode)
            })
        },
        //返回上一级
        backTop() {
            if (this.parentInfo.length === 1) return
            this.parentInfo.pop()
            this.getGeoJson(this.parentInfo[this.parentInfo.length - 1].code)
        },
    },
}
</script>
<style scoped>
.echarts {
    width: 100%;
    height: 100%;
    position: relative;
}
.back {
    position: absolute;
    left: 2%;
    top: 2%;
    color: #eee;
    z-index: 99999;
    cursor: pointer;
}
</style>
```

注：[dome地址：](https://github.com/Not-have/echarts-map)<)<)<https://github.com/Not-have/echarts-ma>>

## 3、百度地图和echarts的使用

注：在百度地图上标点
1）在public——>index.html中引入：

```html
<script type="text/javascript" src="http://api.map.baidu.com/api?v=2.0&ak=ncAvX3AGsOcdx4rNf2LO1ZDmnND0vSgu"></script>
```

2）在页面或者组件中使用

```javascript
<!--
 * @Author: 
 * @Date: 2020-08-18 20:17:29
 * @LastEditors: 
 * @LastEditTime: 2020-08-18 20:22:54
 * @Description: 
 * @FilePath: 
-->
<template>
    <div id="wander">
        
    </div>
</template>
<script>
import echarts from "echarts";
import 'echarts/extension/bmap/bmap';
export default {
    created(){
        this.$nextTick(()=>{
            this.wanMap();
        })
    },
    methods:{
        wanMap(){
            // 基于准备好的dom，初始化echarts实例
            var myChart = echarts.init(document.getElementById('wander'));
            
            var data = [
                {name: '海门', value: 9},
                {name: '鄂尔多斯', value: 12},
                {name: '招远', value: 12},
                {name: '舟山', value: 12},
                {name: '齐齐哈尔', value: 14},
                {name: '盐城', value: 15},
                {name: '赤峰', value: 16},
                {name: '青岛', value: 18}
            ];

            var geoCoordMap = {
                '海门':[121.15,31.89],
                '鄂尔多斯':[109.781327,39.608266],
                '招远':[120.38,37.35],
                '舟山':[122.207216,29.985295],
                '齐齐哈尔':[123.97,47.33],
                '盐城':[120.13,33.38],
                '赤峰':[118.87,42.28],
                '青岛':[120.33,36.07],
            };

            var convertData = function (data) {
                var res = [];
                for (var i = 0; i < data.length; i++) {
                    var geoCoord = geoCoordMap[data[i].name];
                    if (geoCoord) {
                        res.push({
                            name: data[i].name,
                            value: geoCoord.concat(data[i].value)
                        });
                    }
                }
                return res;
            };
           
            var option = {
                backgroundColor: 'transparent',
                title: {
                    text: '流向图',
                    left: 'center',
                    textStyle: {
                        color: '#000',
                        fontSize:19,
                        fontWeight:'500'//文字粗细         
                    }
                },
                tooltip : {
                    trigger: 'item'
                },
                bmap: {
                    center: [104.114129, 37.550339],
                    zoom: 5,
                    roam: true,
                    mapStyle: {
                        styleJson: [
                            
                        ]
                    }
                },
                series : [
                    {
                        name: 'pm2.5',
                        type: 'scatter',
                        coordinateSystem: 'bmap',
                        data: convertData(data),
                        encode: {
                            value: 2
                        },
                        symbolSize: function (val) {
                            return val[2] / 10;
                        },
                        label: {
                            formatter: '{b}',
                            position: 'right'
                        },
                        itemStyle: {
                            color: '#ddb926'
                        },
                        emphasis: {
                            label: {
                                show: true
                            }
                        }
                    },
                    {
                        name: 'Top 5',
                        type: 'effectScatter',
                        coordinateSystem: 'bmap',
                        data: convertData(data.sort(function (a, b) {
                            return b.value - a.value;
                        }).slice(0, 6)),
                        encode: {
                            value: 2
                        },
                        symbolSize: function (val) {
                            return val[2] / 10;
                        },
                        showEffectOn: 'emphasis',
                        rippleEffect: {
                            brushType: 'stroke'
                        },
                        hoverAnimation: true,
                        label: {
                            formatter: '{b}',
                            position: 'right',
                            show: true
                        },
                        itemStyle: {
                            color: 'red',
                            shadowBlur: 10,
                            shadowColor: '#333'
                        },
                        zlevel: 1
                    },
                    {
                        type: 'custom',
                        coordinateSystem: 'bmap',
                        itemStyle: {
                            opacity: 0.5
                        },
                        animation: false,
                        silent: true,
                        data: [0],
                        z: -10
                    }
                ]

            };
            // 使用刚指定的配置项和数据显示图表。
            myChart.setOption(option);
            window.addEventListener("resize", function() {
                myChart.resize();
            });
        }
    }
}
</script>
<style scoped>
    #wander{
        height:600px;
        width:100%;
        border-radius:10%;

    }
    #wander >>> .BMap_cpyCtrl a{
        display: none;
    }
    #wander >>> .anchorBL img{
        display: none;
    }
    #wander >>> .BMap_cpyCtrl{
        display: none;
    }
</style>
```

# 五、Echarts中使用词云

![在这里插入图片描述](./images/20200722194317731.png)
1、下载echarts：
npm install --save echarts
2、下载echarts-wordcloud：
npm install echarts-wordcloud
3、使用

```javascript
<template>
    <div id="cloud" style="height:500px;width:500px">
         
    </div>
</template>
<script>
//引入
import echarts from "echarts";
import "echarts-wordcloud/dist/echarts-wordcloud";
import "echarts-wordcloud/dist/echarts-wordcloud.min";
export default {
    created(){
        this.$nextTick(()=>{
            this.wordColud();
        })
    },
    methods: {
        wordColud(){
            var myChart = echarts.init(document.getElementById('cloud'));
            const colorList = [
                "rgb(12,57,176)",
                "rgb(16,83,190)",
                "rgb(18,102,201)",
                "rgb(21,122,214)",
                "rgb(25,140,220)",
                "rgb(29,161,223)",
                "rgb(32,180,226)",
                "rgb(37,205,231)"
            ]
            var option = {
                // 这个里面指定图标的样式和数据
                tooltip: {
                    backgroundColor: "#fff",
                    axisPointer: {
                        type: "none"
                    },
                    textStyle: {
                        color: "#565656",
                        lineHeight: 28
                    },
                    padding: 12,
                    extraCssText: "box-shadow: 0px 2px 8px 0px #cacaca;border-radius: 4px;opacity: 0.9;",
                    formatter: function(p) {
                        return `热度：${p.data.count}<br/>趋势：${(
                        p.data.heat_diff_num * 100
                        ).toFixed(2)}%`;
                    }
                },
                series: [{
                    type: "wordCloud",
                    shape: "circle",
                    left: "center",
                    top: "center",
                    width: "80%",
                    height: "80%",
                    right: null,
                    bottom: null,
                    sizeRange: [20, 40],
                    rotationRange: [0, 0],
                    gridSize: 16,
                    drawOutOfBound: false,
                    textStyle: {
                        normal: {
                            fontFamily: "sans-serif",
                            color: function() {
                                let index = Math.floor(Math.random() * colorList.length);
                                return colorList[index];
                            }
                        },
                        emphasis: {
                            fontWeight: "bold"
                        }
                    },
                    data: [{
                        "word": "unine",
                        "count": 2041,
                        "heat_diff_num": 2040,
                        "name": "unine",
                        "value": 2040
                    }, {
                        "word": "生日快乐",
                        "count": 13369,
                        "heat_diff_num": 667.45,
                        "name": "生日快乐",
                        "value": 667.45
                    }, {
                        "word": "男星",
                        "count": 512,
                        "heat_diff_num": 511,
                        "name": "男星",
                        "value": 511
                    }, {
                        "word": "迪士尼",
                        "count": 961,
                        "heat_diff_num": 479.5,
                        "name": "迪士尼",
                        "value": 479.5
                    }, {
                        "word": "羽毛",
                        "count": 11753,
                        "heat_diff_num": 404.2758620689655,
                        "name": "羽毛",
                        "value": 404.2758620689655
                    }, {
                        "word": "超级",
                        "count": 3083,
                        "heat_diff_num": 384.375,
                        "name": "超级",
                        "value": 384.375
                    }, {
                        "word": "教程",
                        "count": 615,
                        "heat_diff_num": 306.5,
                        "name": "教程",
                        "value": 306.5
                    }, {
                        "word": "安卓",
                        "count": 4405,
                        "heat_diff_num": 292.6666666666667,
                        "name": "安卓",
                        "value": 292.6666666666667
                    }, {
                        "word": "信用",
                        "count": 989,
                        "heat_diff_num": 196.8,
                        "name": "信用",
                        "value": 196.8
                    }, {
                        "word": "pv",
                        "count": 1630,
                        "heat_diff_num": 180.11111111111111,
                        "name": "pv",
                        "value": 180.11111111111111
                    }, {
                        "word": "苹果",
                        "count": 532,
                        "heat_diff_num": 176.33333333333334,
                        "name": "苹果",
                        "value": 176.33333333333334
                    }, {
                        "word": "互关",
                        "count": 748,
                        "heat_diff_num": 148.6,
                        "name": "互关",
                        "value": 148.6
                    }, {
                        "word": "注册",
                        "count": 1232,
                        "heat_diff_num": 111,
                        "name": "注册",
                        "value": 111
                    }, {
                        "word": "大佬",
                        "count": 817,
                        "heat_diff_num": 101.125,
                        "name": "大佬",
                        "value": 101.125
                    }, {
                        "word": "网页链接",
                        "count": 1522,
                        "heat_diff_num": 100.46666666666667,
                        "name": "网页链接",
                        "value": 100.46666666666667
                    }, {
                        "word": "预告",
                        "count": 464,
                        "heat_diff_num": 76.33333333333333,
                        "name": "预告",
                        "value": 76.33333333333333
                    }, {
                        "word": "期待",
                        "count": 463,
                        "heat_diff_num": 65.14285714285714,
                        "name": "期待",
                        "value": 65.14285714285714
                    }, {
                        "word": "恋爱",
                        "count": 543,
                        "heat_diff_num": 59.333333333333336,
                        "name": "恋爱",
                        "value": 59.333333333333336
                    }, {
                        "word": "周边",
                        "count": 450,
                        "heat_diff_num": 55.25,
                        "name": "周边",
                        "value": 55.25
                    }, {
                        "word": "主线",
                        "count": 535,
                        "heat_diff_num": 43.583333333333336,
                        "name": "主线",
                        "value": 43.583333333333336
                    }, {
                        "word": "ios",
                        "count": 2718,
                        "heat_diff_num": 36.75,
                        "name": "ios",
                        "value": 36.75
                    }, {
                        "word": "微博",
                        "count": 1086,
                        "heat_diff_num": 35.2,
                        "name": "微博",
                        "value": 35.2
                    }, {
                        "word": "氪金",
                        "count": 709,
                        "heat_diff_num": 34.45,
                        "name": "氪金",
                        "value": 34.45
                    }, {
                        "word": "官方",
                        "count": 458,
                        "heat_diff_num": 34.23076923076923,
                        "name": "官方",
                        "value": 34.23076923076923
                    }, {
                        "word": "可爱",
                        "count": 2280,
                        "heat_diff_num": 33.02985074626866,
                        "name": "可爱",
                        "value": 33.02985074626866
                    }, {
                        "word": "吵架",
                        "count": 560,
                        "heat_diff_num": 30.11111111111111,
                        "name": "吵架",
                        "value": 30.11111111111111
                    }, {
                        "word": "开心",
                        "count": 871,
                        "heat_diff_num": 30.107142857142858,
                        "name": "开心",
                        "value": 30.107142857142858
                    }, {
                        "word": "老公",
                        "count": 551,
                        "heat_diff_num": 29.61111111111111,
                        "name": "老公",
                        "value": 29.61111111111111
                    }, {
                        "word": "回来",
                        "count": 608,
                        "heat_diff_num": 29.4,
                        "name": "回来",
                        "value": 29.4
                    }, {
                        "word": "衣服",
                        "count": 699,
                        "heat_diff_num": 29.391304347826086,
                        "name": "衣服",
                        "value": 29.391304347826086
                    }, {
                        "word": "踢踢",
                        "count": 724,
                        "heat_diff_num": 29.166666666666668,
                        "name": "踢踢",
                        "value": 29.166666666666668
                    }, {
                        "word": "快乐",
                        "count": 907,
                        "heat_diff_num": 28.258064516129032,
                        "name": "快乐",
                        "value": 28.258064516129032
                    }, {
                        "word": "客服",
                        "count": 733,
                        "heat_diff_num": 26.14814814814815,
                        "name": "客服",
                        "value": 26.14814814814815
                    }, {
                        "word": "升华",
                        "count": 569,
                        "heat_diff_num": 26.095238095238095,
                        "name": "升华",
                        "value": 26.095238095238095
                    }, {
                        "word": "生日",
                        "count": 950,
                        "heat_diff_num": 23.358974358974358,
                        "name": "生日",
                        "value": 23.358974358974358
                    }, {
                        "word": "李泽言",
                        "count": 3382,
                        "heat_diff_num": 23.15714285714286,
                        "name": "李泽言",
                        "value": 23.15714285714286
                    }, {
                        "word": "男主",
                        "count": 606,
                        "heat_diff_num": 21.444444444444443,
                        "name": "男主",
                        "value": 21.444444444444443
                    }, {
                        "word": "卡面",
                        "count": 567,
                        "heat_diff_num": 20.807692307692307,
                        "name": "卡面",
                        "value": 20.807692307692307
                    }, {
                        "word": "粉丝",
                        "count": 649,
                        "heat_diff_num": 20.633333333333333,
                        "name": "粉丝",
                        "value": 20.633333333333333
                    }, {
                        "word": "时间",
                        "count": 431,
                        "heat_diff_num": 20.55,
                        "name": "时间",
                        "value": 20.55
                    }, {
                        "word": "叠纸",
                        "count": 831,
                        "heat_diff_num": 19.775,
                        "name": "叠纸",
                        "value": 19.775
                    }, {
                        "word": "视频",
                        "count": 456,
                        "heat_diff_num": 19.727272727272727,
                        "name": "视频",
                        "value": 19.727272727272727
                    }, {
                        "word": "进化",
                        "count": 588,
                        "heat_diff_num": 19.275862068965516,
                        "name": "进化",
                        "value": 19.275862068965516
                    }, {
                        "word": "明天",
                        "count": 646,
                        "heat_diff_num": 19.1875,
                        "name": "明天",
                        "value": 19.1875
                    }, {
                        "word": "好看",
                        "count": 1125,
                        "heat_diff_num": 18.736842105263158,
                        "name": "好看",
                        "value": 18.736842105263158
                    }, {
                        "word": "夫人",
                        "count": 3994,
                        "heat_diff_num": 18.294685990338163,
                        "name": "夫人",
                        "value": 18.294685990338163
                    }, {
                        "word": "东西",
                        "count": 454,
                        "heat_diff_num": 17.916666666666668,
                        "name": "东西",
                        "value": 17.916666666666668
                    }, {
                        "word": "好多",
                        "count": 481,
                        "heat_diff_num": 17.5,
                        "name": "好多",
                        "value": 17.5
                    }, {
                        "word": "剧情",
                        "count": 1665,
                        "heat_diff_num": 17.296703296703296,
                        "name": "剧情",
                        "value": 17.296703296703296
                    }, {
                        "word": "老李",
                        "count": 2078,
                        "heat_diff_num": 16.316666666666666,
                        "name": "老李",
                        "value": 16.316666666666666
                    }, {
                        "word": "复刻",
                        "count": 1461,
                        "heat_diff_num": 15.988372093023257,
                        "name": "复刻",
                        "value": 15.988372093023257
                    }, {
                        "word": "洛洛",
                        "count": 2998,
                        "heat_diff_num": 15.937853107344633,
                        "name": "洛洛",
                        "value": 15.937853107344633
                    }, {
                        "word": "希望",
                        "count": 1197,
                        "heat_diff_num": 15.175675675675675,
                        "name": "希望",
                        "value": 15.175675675675675
                    }, {
                        "word": "钻石",
                        "count": 515,
                        "heat_diff_num": 15.09375,
                        "name": "钻石",
                        "value": 15.09375
                    }, {
                        "word": "sr",
                        "count": 689,
                        "heat_diff_num": 15.023255813953488,
                        "name": "sr",
                        "value": 15.023255813953488
                    }, {
                        "word": "女主",
                        "count": 437,
                        "heat_diff_num": 14.607142857142858,
                        "name": "女主",
                        "value": 14.607142857142858
                    }, {
                        "word": "狗叠",
                        "count": 1782,
                        "heat_diff_num": 13.974789915966387,
                        "name": "狗叠",
                        "value": 13.974789915966387
                    }, {
                        "word": "安官",
                        "count": 577,
                        "heat_diff_num": 13.794871794871796,
                        "name": "安官",
                        "value": 13.794871794871796
                    }, {
                        "word": "交易",
                        "count": 1480,
                        "heat_diff_num": 13.509803921568627,
                        "name": "交易",
                        "value": 13.509803921568627
                    }, {
                        "word": "起子",
                        "count": 1299,
                        "heat_diff_num": 13.119565217391305,
                        "name": "起子",
                        "value": 13.119565217391305
                    }, {
                        "word": "评论",
                        "count": 1363,
                        "heat_diff_num": 12.495049504950495,
                        "name": "评论",
                        "value": 12.495049504950495
                    }, {
                        "word": "凌肖",
                        "count": 2415,
                        "heat_diff_num": 12.416666666666666,
                        "name": "凌肖",
                        "value": 12.416666666666666
                    }, {
                        "word": "白起",
                        "count": 6127,
                        "heat_diff_num": 12.407002188183807,
                        "name": "白起",
                        "value": 12.407002188183807
                    }, {
                        "word": "谢谢",
                        "count": 4156,
                        "heat_diff_num": 11.787692307692307,
                        "name": "谢谢",
                        "value": 11.787692307692307
                    }, {
                        "word": "金币",
                        "count": 508,
                        "heat_diff_num": 11.7,
                        "name": "金币",
                        "value": 11.7
                    }, {
                        "word": "太太",
                        "count": 2969,
                        "heat_diff_num": 11.370833333333334,
                        "name": "太太",
                        "value": 11.370833333333334
                    }, {
                        "word": "约会",
                        "count": 521,
                        "heat_diff_num": 10.840909090909092,
                        "name": "约会",
                        "value": 10.840909090909092
                    }, {
                        "word": "sp",
                        "count": 914,
                        "heat_diff_num": 10.012048192771084,
                        "name": "sp",
                        "value": 10.012048192771084
                    }, {
                        "word": "姐妹",
                        "count": 6893,
                        "heat_diff_num": 9.83805031446541,
                        "name": "姐妹",
                        "value": 9.83805031446541
                    }, {
                        "word": "小号",
                        "count": 440,
                        "heat_diff_num": 8.777777777777779,
                        "name": "小号",
                        "value": 8.777777777777779
                    }, {
                        "word": "体力",
                        "count": 572,
                        "heat_diff_num": 7.411764705882353,
                        "name": "体力",
                        "value": 7.411764705882353
                    }, {
                        "word": "吸吸",
                        "count": 3183,
                        "heat_diff_num": 7.246113989637306,
                        "name": "吸吸",
                        "value": 7.246113989637306
                    }, {
                        "word": "恋与制作人",
                        "count": 45901,
                        "heat_diff_num": 6.704095334004699,
                        "name": "恋与制作人",
                        "value": 6.704095334004699
                    }, {
                        "word": "活动",
                        "count": 2796,
                        "heat_diff_num": 6.096446700507614,
                        "name": "活动",
                        "value": 6.096446700507614
                    }, {
                        "word": "抽到",
                        "count": 704,
                        "heat_diff_num": 6.04,
                        "name": "抽到",
                        "value": 6.04
                    }, {
                        "word": "星河",
                        "count": 496,
                        "heat_diff_num": 5.2784810126582276,
                        "name": "星河",
                        "value": 5.2784810126582276
                    }, {
                        "word": "少女",
                        "count": 9373,
                        "heat_diff_num": 4.102340772999455,
                        "name": "少女",
                        "value": 4.102340772999455
                    }, {
                        "word": "超现实",
                        "count": 9306,
                        "heat_diff_num": 4.0880262438490975,
                        "name": "超现实",
                        "value": 4.0880262438490975
                    }, {
                        "word": "恋爱吧",
                        "count": 9184,
                        "heat_diff_num": 4.057268722466961,
                        "name": "恋爱吧",
                        "value": 4.057268722466961
                    }, {
                        "word": "许墨",
                        "count": 2642,
                        "heat_diff_num": 2.931547619047619,
                        "name": "许墨",
                        "value": 2.931547619047619
                    }, {
                        "word": "周棋洛",
                        "count": 2864,
                        "heat_diff_num": 1.7778855480116391,
                        "name": "周棋洛",
                        "value": 1.7778855480116391
                    }, {
                        "word": "喜欢",
                        "count": 3233,
                        "heat_diff_num": 1.7774914089347078,
                        "name": "喜欢",
                        "value": 1.7774914089347078
                    }, {
                        "word": "ssr",
                        "count": 1701,
                        "heat_diff_num": 1.2955465587044535,
                        "name": "ssr",
                        "value": 1.2955465587044535
                    }, {
                        "word": "奔赴",
                        "count": 11753,
                        "heat_diff_num": 0,
                        "name": "奔赴",
                        "value": 0
                    }, {
                        "word": "飘舞",
                        "count": 11753,
                        "heat_diff_num": 0,
                        "name": "飘舞",
                        "value": 0
                    }, {
                        "word": "开口",
                        "count": 5845,
                        "heat_diff_num": 0,
                        "name": "开口",
                        "value": 0
                    }, {
                        "word": "心领神会",
                        "count": 5778,
                        "heat_diff_num": 0,
                        "name": "心领神会",
                        "value": 0
                    }, {
                        "word": "无需",
                        "count": 5505,
                        "heat_diff_num": 0,
                        "name": "无需",
                        "value": 0
                    }, {
                        "word": "话语",
                        "count": 4406,
                        "heat_diff_num": 0,
                        "name": "话语",
                        "value": 0
                    }, {
                        "word": "ins",
                        "count": 2122,
                        "heat_diff_num": 0,
                        "name": "ins",
                        "value": 0
                    }, {
                        "word": "油管",
                        "count": 1254,
                        "heat_diff_num": 0,
                        "name": "油管",
                        "value": 0
                    }, {
                        "word": "0409",
                        "count": 1251,
                        "heat_diff_num": 0,
                        "name": "0409",
                        "value": 0
                    }, {
                        "word": "动画",
                        "count": 1104,
                        "heat_diff_num": 0,
                        "name": "动画",
                        "value": 0
                    }, {
                        "word": "备案",
                        "count": 844,
                        "heat_diff_num": 0,
                        "name": "备案",
                        "value": 0
                    }, {
                        "word": "vp",
                        "count": 816,
                        "heat_diff_num": 0,
                        "name": "vp",
                        "value": 0
                    }, {
                        "word": "520",
                        "count": 594,
                        "heat_diff_num": 0,
                        "name": "520",
                        "value": 0
                    }, {
                        "word": "偶像",
                        "count": 549,
                        "heat_diff_num": 0,
                        "name": "偶像",
                        "value": 0
                    }, {
                        "word": "艺人",
                        "count": 518,
                        "heat_diff_num": 0,
                        "name": "艺人",
                        "value": 0
                    }, {
                        "word": "饭局",
                        "count": 512,
                        "heat_diff_num": 0,
                        "name": "饭局",
                        "value": 0
                    }, {
                        "word": "脸书",
                        "count": 464,
                        "heat_diff_num": 0,
                        "name": "脸书",
                        "value": 0
                    }]
                }]
            };
            myChart.setOption(option);
        }
    },
}
</script>
<style scoped>

</style>
```

# 六、在Echarts中使用圆图

![在这里插入图片描述](<(<https://img-blog.csdnimg.cn/20200722193643526.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ1NjY5MTc4,size_16,color_FFFFFF,t_7>> =150x100)
1、下载：
`
m install echarts-liquidfill
`
2、在那个页面使用，就在那个页面引入

```javascript
import 'echarts-liquidfill';
```

3、案例：

```javascript
<template>
    <div id="disjunctor" style="height:2.6rem">
        
    </div>
</template>
<script>
import echarts from "echarts";
import 'echarts-liquidfill';
export default {
    created(){
        this.$nextTick(()=>{
            this.junctor();
        })
    },
    methods:{
        junctor(){
            // 基于准备好的dom，初始化echarts实例
            var myChart = echarts.init(document.getElementById('disjunctor'));
            const data =  [0.68, 0.36]
            var option = {
                // x轴
                xAxis:{
                    show:false, // 不显示
                },
                // y轴
                yAxis: {
                    show:false, // 不显示
                },
                grid:{
                    top:'2.5%',
                    right:'40',
                    bottom:'2.5%',
                    left:0,
                }, 
                series: [{
                    type: 'liquidFill',
                    radius: '96%',  // 半径大小
                    center: ['50%', '50%'],    // 布局位置
                    data, // 水球波纹值
                    
                    outline: { // 轮廓设置
                        show: true,
                        borderDistance: 2, // 轮廓间距
                        itemStyle: {
                            borderColor: '#294D99', // 轮廓颜色
                            borderWidth: 4, // 轮廓大小
                            shadowBlur: 'none', // 轮廓阴影
                        }
                    },
                },{
                    type: 'line', // 折线图
                    markLine: {
                        silent: true, // 不触发鼠标事件
                        symbol:'', // 标线两端样式
                        lineStyle:{ // 标线样式
                            color: '#f00',
                            type: 'solid'
                        }, 
                        data: [{ // 标线数据
                            yAxis: data // y 轴
                        }]
                    }
                }]
            };
            // 使用刚指定的配置项和数据显示图表。
            myChart.setOption(option);
            window.addEventListener("resize", function() {
                myChart.resize();
            });
        }
    }
}
</script>
<style lang="scss" scoped>

</style>
```
