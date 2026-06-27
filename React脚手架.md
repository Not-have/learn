@[TOC]

# 一、脚手架

vsc插件：
     ①切实同源策略是浏览器的一个安全功能，不同源的客户端脚本在没有明确授权的情况下，不能读写对方资源。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200403000838611.png)
    ②在线监测数据是否为json数据。

## 1、react安装

1）全局安装脚手架
npm install -g create-react-app
2）初始化
npm init
3)创建项目
create-react-app 项目名
4）切到创建的项目里
cd 项目名
5）启动
npm start

## 2、加手脚使用目录

1）public文件夹：里面的index.html，是整个react项目最终打包的index入口页面的项目模板。但是和我们的代码编写无关，和最终的页面展示是相关的。
2）src文件夹：是我们编写代码的地方。
3）src/index.js：是整个项目的入口js文件。
4）src/app.js：是被index引入的一个组件，也用一个js文件表示。
5）src/index.css：index.js中的的样式文件。
6）src/app.css：是app.js的样式文件。
7）logo.svg：是一个svg的图片文件。用于界面展示使用。
8）创建文件夹：
①assets 静态资源文件夹
②components组件文件夹
③pages页面文件夹

## 3、组件的使用

1）创建一个组件src——>components——>xxx（它里面包括.jsx和.css）
注:装上插件之后rcc，就可以出来下面这些
①在xxx里创建Xxx.jsx(建议是jsx，js也行；同时文件首字母大写)

```java
import React, { Component } from 'react'
//引入css文件
import './index.css';
//自动关联你的文件名 即index；export default 暴露
export default class index extends Component {
    render() {
        return (
            <div>
                
            </div>
        )
    }
}
```

②在xxx.jsx里创建xxx.css(书写样式)；他的文件名与jsx的文件名一样。
注：在xxx.jsx里引入。
2）在app.js根组件里使用（也可在其他页面中使用）

```javascript
import React from 'react';
import './App.css';
//1、 引用
import Index from './components/index'
function App() {
  return (
    <div>
      {/* 2、使用 */}
      <Index />
    </div>
  );
}

export default App;
```

## 4、 正向传值

1）不需要验证数据类型的
①在引用、使用的页面传数据

```java
import React from 'react';
import logo from './logo.svg';
import './App.css';
import Index from './components/index/Index'
function App() {
  return (
    <div className="App">
      {/* 使用 ,在这传数据*/}
      <Index title="props数据传值，传给组件"/>
    </div>
  );
}
export default App;
```

②在组件里接收

```java
import React, { Component } from 'react'
//引入css文件
import './index.css';
//自动关联你的文件名 即Index；export default 暴露
export default class Index extends Component {
    render() {
        return (
            <div>
                <p>{this.props.title}</p>
            </div>
        )
    }
}
```

2）props验证数据类型的（就不能上面那样写，需要换种写法，vsc快捷键rccp）
①在组件页

```java
import React, { Component } from 'react'
import PropTypes from 'prop-types'

export default class Index extends Component {
    static propTypes = {
        //接受数据（数据名:数据类型）
        title: PropTypes.string
    }


    render() {
        return (
            <div>
                {this.props.title}
            </div>
        )
    }
}
```

②在传值页面里

```java
import React from 'react';
import './App.css';
import Index from './components/index/Index'
function App() {
  return (
    <div>
      {/* 使用 ,在这传数据*/}
      <Index title="props数据传值，传给组件"/>
    </div>
  );
}

export default App;
```

## 5、 修改值

```java
import React, { Component } from 'react'
import PropTypes from 'prop-types'
export default class Index extends Component {
    constructor(props){
        super(props);
        this.state={
            text:"我是state的值"
        }
    }
    fun=()=>{
        //修改数据
        this.setState({
            text:"我是修改之后的值"
        })
    }
    render() {
        return (
            <div>
                <p>{this.state.text}</p>
                <button onClick={this.fun}>修改</button>
            </div>
        )
    }
}
```

## 6、便利数据

```java
import React, { Component } from 'react'


export default class Home extends Component {
    constructor(props){
        super(props);
        this.state={
            obj:["拉拉","呵呵","嘿嘿","啊啊"]
        }
    }
    render() {
        // 便利数据
        let newobj=this.state.obj.map((v,i)=>{
            return <p key={i}>{v}</p>;
        })
        return (
            <div>
                {newobj}
            </div>
        )
    }
}
```

## 7、事件传参（this指向问题，在下面）

```java
import React, { Component } from 'react'
export default class Mine extends Component {
    funa=(texta,textb)=>{
        console.log("1:"+texta+"2:"+textb);
    }
    funb=(num1,num2)=>{
        console.log("一:"+num1+"二:"+num2);
    }
    render() {
        return (
            <div>
                <button onClick={this.funa.bind(this,"我是参数一","我是参数二")}>点我调用事件传递参数one</button>
                <button onClick={()=>{this.funb("1","2")}}>点我调用事件传递参数two</button>
            </div>
        )
    }
}
```

## 8、逆向传值

1）在子中（即是他被引用、使用）

```java
import React, { Component } from 'react'

export default class Mineson extends Component {
    // 1.在子组件中创建数据
    constructor(props){
        super(props);
        this.state={
            text:"我是子组件的数据"
        }
    }
    render() {
        return (
            <div>
                <p>子组件</p>
                {/* 2.给子son这个函数 绑定一个参数 ;this.state.text是实参*/}
                {/* 接受父组件传递过来的props函数 并且给他绑定this与参数 */}
                <button onClick={this.props.son.bind(this,this.state.text)}>2、点我吧数据传给夫</button>
            </div>
        )
    }
}
```

2）在父中

```java
import React, { Component } from 'react'
// 夫组件中调用子
import Mineson from './Mineson'
export default class Mine extends Component {
    // 4.这就需要一个形参(创建函数接受子组件的逆向传值)
    father=(texts)=>{
        console.log(texts);
    }
    render() {
        return (
            <div>
                <p>父组件</p>
                {/* 3.给它传一个son函数*/}
                <Mineson son={this.father}/>
            </div>
        )
    }
}
```

## 9、条件渲染（就是是否显示这个内容）

1）if的方式

```java
import React, { Component } from 'react'

export default class Xuanran extends Component {
    render() {
        // if的方式(模拟，不用state)
        let newhtml=""
        if(true){//true显示111；false显示222
            newhtml="111111"
        }else{
            newhtml="222222"
        }
        return (
            <div>
                {newhtml}
            </div>
        )
    }
}
```

2）三目

```java
import React, { Component } from 'react'

export default class Xuanran extends Component {
    render() {
        return (
            <div>
               {/* 三目  true显示111；false显示222*/}
               {false?<p>1111</p>:<p>2222</p>}{/* 这块显示222*/}
            </div>
        )
    }
}
```

3）循环渲染
        Key 可以在 DOM 中的某些元素被增加或删除的时候帮助 React 识别哪些元素发生了变化。因此要给数组中的每一个元素赋予一个确定的标识；一个元素的key最好是这个元素在列表中拥有的一个独一无二的字符串。（减轻比对次数）
①与render同级处写

```java
import React, { Component } from 'react'

export default class Xuanran extends Component {
    constructor(props){
        super(props);
        this.state={
            obj:[
                {text:"app1"},
                {text:"app2"},
                {text:"app3"},
                {text:"app4"},
                {text:"app5"}
            ]
        }
    }
    render() {
        let newhtml=this.state.obj.map((v,i)=>{
            return <p key={i}>{v.text}</p>
        })
        return (
            <div>
              {newhtml}
            </div>
        )
    }
}
```

②写在div里,用花括号包裹，让其当成js运行

```java
import React, { Component } from 'react'

export default class Xuanran extends Component {
    constructor(props){
        super(props);
        this.state={
            obj:[
                {text:"app1"},
                {text:"app2"},
                {text:"app3"},
                {text:"app4"},
                {text:"app5"}
            ]
        }
    }
    render() {
        return (
            <div>
                {
                    this.state.obj.map((v,i)=>{
                        return <p key={i}>{v.text}</p>
                    })
                }
            </div>
        )
    }
}
```

注：渲染便签内的属性方法如下：

```java
    render() {
        let newhtml=this.state.数据名.map((v,i)=>{
        
             // 注这个key值只能写在它的下一级
            return <div key={i}>
                <img src={v.XXX} />
            </div>
        })
        return (
            <div>
                {newhtml}
            </div>
        )
    }
```

4）案例（点击添加，勾选删除）

```java
import React, { Component } from 'react'
export default class Demoe extends Component {
    constructor(props){
        super(props)
        // 1.创建数据
        this.state={
            obj:[
                // 11.给数据加一个变量用来保存当前行 数据的复选框勾选状态  不要忘了在新增哪里也要加
                {title:"吃饭",style:false},
                {title:"睡觉",style:false},
                {title:"在吃饭",style:false}
            ]
        }
        // 5-1.得到输入框的值 创建ref
        this.myref=React.createRef()
    }
    // 4.创建添加的函数
    add=()=>{
          // 5-3.得到输入框的值
          var inputval=this.myref.current.value;
          console.log(inputval)


        //   6.给数组新增内容
        // 6-1把state的数组付给一个新的变量
        var newobj=this.state.obj;
        // 6-2给这个新的变量push内容
        newobj.push({title:inputval,style:false})
        // 6-3修改state中的原始数据
        this.setState({
            obj:newobj
        })
        // 7.清空输入框
        this.myref.current.value=""
    }
    // 10创建出勾选复选框触发的函数
    // 13创建形参接受i
    ckb=(i)=>{
        console.log(i)
        // 14 修改那一数据的style 取反
        let newobj=this.state.obj;
        newobj[i].style=!newobj[i].style
        this.setState({
           obj:newobj
        })
    }
    // 19.创建删除函数
    del=()=>{
        // 20-1创建一个变量用来保存删除之后的数据
        let newobj=[];
        // 20-2开始循环判断 style属性是否为false  如果是false那么就是我们想要的 push存到newobj中
        this.state.obj.forEach((v,i)=>{
            if(v.style==false){
                newobj.push(v)
            }
        })
        // 20-3给state中的数据赋值
        this.setState({
            obj:newobj
        })
    }
    render() {
        return (
            <div>
                <h1>练习--删除</h1>
                 {/* 5-2.得到输入框的值 绑定ref */}
                <input type="text" ref={this.myref}/>
                {/* 3.添加一个点击事件 */}

                <button onClick={this.add}>添加</button>
                {/* 18创建删除按钮并且绑定事件调用删除函数 */}

                <button onClick={this.del}>点我删除</button>
                <ul>
                    {/* 2.便利数据展示 */}
                    {
                        this.state.obj.map((v,i)=>{
                            return(
                                <li key={i}>
                                    {/* 8.添加复选框 */}
                                    {/* 9.可以给输入框绑定一个onChange事件 */}
                                    {/* 12.把当前的下表传递给函数 我们就知道勾选的是谁 */}
                                    <input type="checkbox" onChange={()=>this.ckb(i)}/>
                                    {/* 15.使用三木测试勾选状态并且添加一个span包裹 */}
                                    {/* 16根据真假添加或者删除类名 */}
                                    <span className={v.style?'red':''}> {v.title}--{v.style?'真':'假'}</span>
                             </li>
                            )
                        })
                    }
                </ul>
            </div>
        )
    }
}
```

5）循环渲染-删除内容
父组件里的传给子组件，在子组件里，设置删除按钮（在根据删除的东西，子在逆传给父）
①夫组件

```java
import React, { Component } from 'react'
import Son from './Son'
export default class Father extends Component {
    constructor(props){
        super(props);
        this.state={
            obj:[
                {text:"啦啦啦一"},
                {text:"啦啦啦二"},
                {text:"啦啦啦三"},
                {text:"啦啦啦四"},
                {text:"啦啦啦五"}
            ]
        }
    }
    //de到逆向传过来的数据
    fudel=(e)=>{
        console.log(e);
        let newobj=this.state.obj;
        // 根据下标进行删除
        newobj.splice(e,1);
        this.setState({
            obj:newobj
        })
    }
    render() {
        return (
            <div>
                {
                    this.state.obj.map((v,i)=>{
                        // 两正  一逆
                        return <Son text={v.text} key={i} num={i} fatherdel={this.fudel}/>
                    })
                }
            </div>
        )
    }
}
```

②子组件

```java
import React, { Component } from 'react'


export default class Son extends Component {
    render() {
        return (
            <div>
                {/* 子组件正接收num，然后逆向在给夫传一个i */}
                <p>{this.props.text} <button onClick={this.props.fatherdel.bind(this,this.props.num)}> 删除</button></p>
            </div>
        )
    }
}
```

![](https://img-blog.csdnimg.cn/20200403001900676.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ1NjY5MTc4,size_16,color_FFFFFF,t_70)

## 10、数据请求

1）Axios数据请求
①下载
npm install --save axios
②那个页面或组件要用，就要先在里面进行引入

```java
import axios from 'axios'
```

③在页面或组件中请求的书写方式

```java
import React, { Component } from 'react'
import axios from 'axios'
export default class Qing extends Component {
    //
    componentDidMount(){
        axios({
            url:"请求地址",
            method:"get"
        }).then((ok)=>{
            console.log(ok);
        })
    }
    render() {
        return (
            <div>


            </div>
        )
    }
}
```

③请求get数据的两种方法

```java
        //第一种
        Axios({
            url:'',//地址
            method:'get',//发送方式
            params:{//url请求携带的参数
                wholeId:变量
            }
        }).then((ok)=>{
            console.log(ok)
        })
        

        //第二种
        axios({
            url:`路径?Id=${变量}`,
            method:"get"
        }).then((ok)=>{
            console.log(ok.data.banner);
        })
```

2）jQuery请求
①下载
npm install --save jquery
②那个页面或组件要用，就要先在里面进行引入

```java
import $ from 'jquery'
```

③在页面或组件中请求的书写方式

```java
import React, { Component } from 'react'
import $ from "jquery"
export default class Qing extends Component {
    //
    componentDidMount(){
        $.ajax({
            url:"请求地址",
            type:"get",//请求方式
            success(ok){
                console.log(ok);
            }
        })
    }
    render() {
        return (
            <div>


            </div>
        )
    }
}
```

3)fetch数据交互
①数据请求
注： fetch是原生的，不需要下载

```java
import React, { Component } from 'react'
export default class Qing extends Component {
    constructor(props){
        super(props)
        this.state={
            obj:[]
        }
    }
    //
    componentDidMount(){
        fetch("http://api.artgoer.cn:8084/artgoer/api/v1/user/324380/v3/topic/topicHomeByLabel?pageIndex=1&token=b544cd63-6d42-46fe-a96c-3cf96bae3113&topicId=62187")
        .then((req)=>{return req.json()}) //req是形参
        .then((data)=>{
            console.log(data.data.commentList)
            //修改数据
            this.setState({
                obj:data.data.commentList
            })
        })//data是形参，自己想写神魔就写什么
        //(res)=>{return res.json()} 等价于 (res)=>res.json()   .json()代表转成json格式
    }
    render() {
        return (
            <div>
                {
                    this.state.obj.map((v,i)=>{
                        return <p key={v.id}>{v.commentTxt}</p>
                    })
                }
            </div>
        )
    }
}
```

②数据发送

```java
import React, { Component } from 'react'
export default class Qing extends Component {
    //
    componentDidMount(){
        //发送post
        fetch("地址", {
            method: "POST",
            headers: {'Content-Type': 'application/x-www-form-urlencoded'},
            body: "key=val&key=val"
        })
        .then(req=>req.json())
        .then(function(ok) {console.log(ok)})


        //发送get
        fetch("http://localhost:8888/get?key=val", {
            method: "get"})
            .then(req=>req.json())
            .then(function(ok) {console.log(ok)})
        }
    }
    render() {
        return (
            <div>

            </div>
        )
    }
}
```

4）在数据没请求过来的时候，让其显示一个加载动画

```java
import React, { Component } from 'react';
import axios from "axios";
import {Row,Col} from "antd";

export default class activity extends Component {
    constructor(props){
        super(props);
        this.state={
            photograph:[]
        }
    }
    componentWillMount(){
        axios({
            url:"/banner/one",
            method:"get"
        }).then((ok)=>{
            // console.log(ok.data.activity);
            this.setState({
                photograph:ok.data.activity
            })
        })
    }
    render() {
        // 给他加一个没有数据的时候，显示动画效果
        let arr='';
        //给其一个判断，让其 数据长度等于零的时候，显示图片
        if(this.state.photograph.length == 0){
            arr = <img src="图片路径" alt=""/>
        }else{
            arr = this.state.photograph.map((v,i)=>{
                    return <Col span={5} className="arrangeEach" key={i}  onClick={this.func.bind(this)}>
                    <div className="arrangeimage">
                        <img src={v.picture} alt="" className="photographSon"/>
                    </div>
                    <p>{v.time}</p>
                    <h3>{v.title}</h3>
                </Col>
            })
        }
        return (
            <div className="activitybox">
                {/* 数据未请求到的时候，加的一个动画 */}
                
                {/* 数据请求到了，进行展示 */}
                <Row>
                    {arr}
                </Row>
            </div>
        )
    }
}
```

## 11、refs

1） ref属性（标识组件内部的元素）
①React提供的这个ref属性(不能在函数式组件上使用 ref 属性，因为它们没有实例)表示为对组件真正实例的引用其实就是ReactDOM.render()返回的组件实例
②ReactDOM.render()渲染组件时返回的是组件实例；而渲染dom元素时，返回是具体的dom节点。
2）React的ref有3种用法和使用：
①字符串(官方不推荐使用，因为他是操作DOM)   版本低于16.3 使用它。

```java
import React, { Component } from 'react'
export default class Ref extends Component {
    constructor(props){
        super(props)
        this.state={
            text:""
        }
    }
    fun=function(){
        this.setState({
            text:this.refs.eleminput.value
        })
    }
    render(){
        return (
            <div>
            {/*如果版本低于16.3推荐使用这一种*/}
               <input type="text" ref="eleminput" onInput={()=>{this.fun()}}/>
               <p>输入框的内容是-----{this.state.text}</p>
            </div>
        )
    }
}
```

②回调函数
        回调函数就是在dom节点或组件上挂载函数，函数的形参是dom节点或组件实例，达到的效果与字符串形式是一样的，都是获取其引用。

```java
import React, { Component } from 'react'export default class Ref extends Component { 
   constructor(props){        
     super(props);        
     this.state={            
      text:""        
     }    
   }    
   fun=()=>{        
     this.setState({            
      text:this.haha.value        
     })    
    }    
    render() {        
     return (            
      <div>                
       {/* 他的ref里面是一个回调函数，这个回调函数里面必须要有一个形参，这个形参就是当前元素，他会自动的把当前元素注入到这个形参里 */}                
       <input type="text" onInput={this.fun} ref={(xiaoming)=>{this.haha=xiaoming}}/>  {/* xiaoming是形参，haha是后期调用的名字 */}                
       <p>{this.state.text}</p>            
      </div>        
     )    
    }}
```

③React.createRef() （React16.3以后才可以使用）推荐使用
        使用此方法来创建ref，将其赋值给一个变量，通过ref挂载在dom节点或组件上该ref的current属性将能拿到dom节点或组件的实例。

```java
import React, { Component } from 'react'

export default class Ref extends Component {
    constructor(props){
        super(props);
        //1.初始化ref这个实例对象，然后在赋值给一个变量
        this.xiaoming=React.createRef();//创建
        this.state={
            text:""
        }
    }
    fun=function(){
        this.setState({
            // 3.调用ref,并且获取它的值
            text:this.xiaoming.current.value
        })
    }
    render() {
        return (
            <div>
                {/* 2.使用ref*/}
                <input type="text" ref={this.xiaoming} onInput={()=>{this.fun()}}/>
                <p>输入框的内容是-----{this.state.text}</p>
            </div>
        )
    }
}
```

## 12、react声明周期

1）componentWillMount 在渲染前调用,在客户端也在服务端。

2）componentDidMount : 在第一次渲染后调用，只在客户端。之后组件已经生成了对应的DOM结构，可以通过this.getDOMNode()来进行访问。 如果你想和其他JavaScript框架一起使用，可以在这个方法中调用setTimeout, setInterval或者发送AJAX请求等操作(防止异步操作阻塞UI)。

3）componentWillReceiveProps 在组件接收到一个新的 prop (更新后)时被调用。这个方法在初始化render时不会被调用。

4）shouldComponentUpdate 返回一个布尔值。在组件接收到新的props或者state时被调用。在初始化时或者使用forceUpdate时不被调用。     可以在你确认不需要更新组件时使用。

注：以下这样写父组件更新，子组件不会跟随夫组件更新。

```javascript
 shouldComponentUpdate() {
        return false;
    }
```

5）componentWillUpdate在组件接收到新的props或者state但还没有render时被调用。在初始化时不会被调用。

6）componentDidUpdate 在组件完成更新后立即调用。在初始化时不会被调用。

7）componentWillUnmount在组件从 DOM 中移除之前立刻被调用。

## 13、react数据请求在哪声明周期？

​          对于同步的状态改变，是可以放在componentWillMount，对于异步的，最好好放在componentDidMount。但如果此时有若干细节需要处理，比如你的组件需要渲染子组件，而且子组件取决于父组件的某个属性，那么在子组件的componentDidMount中进行处理会有问题：因为此时父组件中对应的属性可能还没有完整获取，因此就让其在子组件的componentDidUpdate中处理。

## 14、优化组件渲染

1）父组件

```javascript
/**
 * https://developer.mozilla.org/zh-CN/docs/Web/API/Fetch_API/Using_Fetch
 * http://nodejs.cn/api/querystring.html
 */
import React from 'react'
// 这个是node里面的
import DomeSon1 from "./components/DomeSon1";
const myApi = {
    conut: 0,
    subscribe(cb) {
        this.intervalId = setInterval(() => {
            this.conut += 1;
            // console.log(this.conut);
            // 这是一个回调函数
            cb(this.conut);
        }, 1000)
    },
    unSubscribe() {
        clearInterval(this.intervalId);
        this.reselt();
    },
    reselt() {
        this.conut = 0;
    }
}
export default class App extends React.Component {
    // constructor(props) {
    //     super(props);
    //     this.state = {
    //         conut: 0
    //     }
    // }
    state = {
        conut: 0
    }
    
    componentDidMount() {
        myApi.subscribe(num => {
            this.setState({
                conut: num
            })
        })
    }
    // 离开页面的时候，清除定时器
    componentWillUnmount() {
        myApi.unSubscribe();
    }
    
    render() {
        console.log("父页面")
        return (
            <div>
                <p>父组件： {this.state.conut}</p>
    {/* 如果夫给子中传入的数据，不改变的时候，子组件不会渲染视图，当夫给子传入的数据一直改变 的时候，子组件才会渲染 */}
                <DomeSon1 num={1} />
            </div>
        )
    }
}
```

2）子组件

```javascript
import React from "react";

export default class DomeSon1 extends React.Component {
    shouldComponentUpdate(nextProps, nextState, nextContext) {
        // console.log(nextProps, nextState, nextContext)
        if (nextProps.num === this.props.num) {
            return false;
        }
        return true;
    }
    
    render() {
        console.log("子组件")
        return (
            <div>
                child:{this.props.num}
            </div>
        )
    }
}
```

## 15 、*Component* 与 *PureComponent*  的区别

1）Component 不会对数据进行比较

```javascript
import React from 'react';
export default class App extends React.Component {    
    render() {
        return (
            <div>
                这个不会对比数据的改变，如果   他是子组件，那么父组件的数据改变，他会跟着父组件重新渲染
            </div>
        )
    }
}
```

2）PureComponent对数据进项浅比较

```javascript
import React from "react";
export default class DomeSon1 extends React.PureComponent {
    render() {
        console.log("子组件")
        return (
            <div>
                他是子组件，当夫组件传进来的props不改变的时候，他不会  再去渲染这个组件
            </div>
        )
    }
}
```

# 二、脚手架路由

## 1、路由分类

1）React-Router：提供了一些router的核心API，包括Router, Route, Switch等，但是它没有提供 DOM 操作进行跳转的API。
2）React-Router-DOM：提供了 BrowserRouter, Route, Link等 API,我们可以通过 DOM 的事件控制路由。例如点击一个按钮进行跳转，大多数情况下我们是这种情况，所以在开发过程中，我们更多是使用React-Router-DOM。

## 2、路由的属性

1）HashRouter
url中会有个#，例如localhost:3000/#，HashRouter就会出现这种情况，它是通过hash值来对路由进行控制。如果你使用HashRouter，你的路由就会默认有这个#；
2）BrowserRouter
很多情况下我们则不是这种情况，我们不需要这个#，因为它看起来很怪，这时我们就需要用到BrowserRoute。

## 3、 路由-link与switch (一般情况下，用的都是Link)

1）Link 主要API是to，to可以接受string或者一个object，来控制url；
2）NavLink 它可以为当前选中的路由设置类名、样式以及回调函数等（不建议用）；
3）to属性跳转路径activeClassName当元素处于活动状态时应用于元素的样式。

## 4、一级路由（我们用React-Router-DOM）

1）下载路由模块
npm install --save react-router-dom
2）在App.js里引入
注：引包

```java
import {BrowserRouter,Route,Link,NavLink,Redirect,Switch} from 'react-router-dom';
```

```javascript
import React from 'react';
import './App.css';
//BrowserRouter 设置模式；Route 路由配置加出口；Link 跳转
import {BrowserRouter,Route,Link} from 'react-router-dom';
function App() {
  return (
    <div className="App">
    </div>
  );
}
export default App;
```

3）有路由就要有页面，在src——>pages里创建页面，即用来存放路由页面（一个路由对应一个页面，页面文件后缀是 .jsx）
基本页面框框

```java
import React, { Component } from 'react'

export default class Xxx extends Component {
    render() {
        return (
            <div>
                
            </div>
        )
    }
}
```

4）设置路由规则（配置路由），在App.js里配置

```java
//一下是基本的路由配置
import React from 'react';
import './App.css';
//BrowserRouter 设置模式；Route 路由配置加出口；Link 跳转
import {BrowserRouter,Route,Link,Redirect,Switch} from 'react-router-dom';
//1.引入页面
import Index from './pages/Index';
import List from './pages/List';
import Mine from './pages/Mine';
function App() {
  return (
    <div className="App">
      {/* 2.用BrowserRouter包裹所有的路由规则 */}
      <BrowserRouter>
        {/* 4.设置路由导航 */}
        <Link to="/index">index</Link>&nbsp;&nbsp;&nbsp;&nbsp;
        <Link to="/list">list</Link>&nbsp;&nbsp;&nbsp;&nbsp;
        <Link to="/mine">mine</Link>
        {/* 6.switch唯一匹配 */}
        <Switch>
          {/* 3.用Route设置路由规则，并且他也是出口的意思 */}
          {/* <Route path="/路径" component={引用上面引入文件的名}/> */}
          <Route path="/index" component={Index}/>
          <Route path="/list" component={List}/>
          <Route path="/mine" component={Mine}/>
        </Switch>
        {/* 5.路由重定向 */}
        <Redirect to='/index' />
      </BrowserRouter>
    </div>
  );
}
export default App;
```

注：①exact代表当前路由path的路径采用精确匹配，比如说Home的path如果不加上exact,那么path="/about"将会匹配他自己与path="/“这两个，所以一般path=”/"这个路由一般会加上exact，另外需要注意一点的是嵌套路由不要加exact属性，如果父级路由加上，这里例如topics加上该属性，他下面的子路由将不会生效，因为外层强制匹配了。（精准匹配，即和path里面的地址完全一样，才会匹配到，一般加到默认的上面，效率会更高些）

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200403002942232.png)

   ② <Switch>为了解决route的唯一渲染，它是为了保证路由只渲染一个路径。
<Switch>是唯一的，因为它仅仅只会渲染一个路径，当它匹配完一个路径后，就会停止渲染了。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200403003003796.png)

## 5、路由导航只在有些页面里有，有些页面里没有，比如详情页就没有；他的实现方法如下

1）首先要引入页面，在src——>app.js里

```java
import React from 'react';
import './App.css';
//BrowserRouter 设置模式；Route 路由配置加出口；Link 跳转
import {BrowserRouter,Route,Link,Redirect,Switch} from 'react-router-dom';
//1.引入页面
import Index from './pages/Index';
import List from './pages/List';
import Mine from './pages/Mine';
import All from './pages/All';
function App() {
  return (
    <div className="App">
      {/* 2.用BrowserRouter包裹所有的路由规则 */}
      <BrowserRouter>
        {/* 4.设置路由导航 （把它封装成了一个组件）*/}

        <Switch>
          {/* 3.用Route设置路由规则，并且他也是出口的意思 */}
          {/* <Route path="/路径" component={引用上面引入文件的名}/> */}
          <Route path="/index" component={Index}/>
          <Route path="/list" component={List}/>
          <Route path="/mine" component={Mine}/>
          <Route path="/all" component={All}/>
        </Switch>
        {/* 5.路由重定向 */}
        <Redirect to='/index' />
      </BrowserRouter>
    </div>
  );
}
export default App;
```

2）吧路由导航封装成一个组件

```java
import React, { Component } from 'react'
import {BrowserRouter,Route,Link,Redirect,Switch} from 'react-router-dom';
export default class Links extends Component {
    render() {
        return (
            <div>
                {/* 4.设置路由导航 */}
                <Link to="/index">index</Link>&nbsp;&nbsp;&nbsp;&nbsp;
                <Link to="/list">list</Link>&nbsp;&nbsp;&nbsp;&nbsp;
                {/* 比如在mine里面没有导航，就不要在mine这个页面里，引用这个组件了 */}
                <Link to="/mine">mine</Link>
            </div>
        )
    }
}
```

3）那个页面要使用导航，就引用它

```java
import React, { Component } from 'react'
import Links from '../components/links'
export default class Index extends Component {
    render() {
        return (
            <div>
                <Links />
                1
            </div>
        )
    }
}

```

## 6、编程式路由

```java
  <button onClick={()=>{this.props.history.push("/地址")}}>点我去另一个页面（即编程式页面跳转）</button>
```

注：这块  也可以写在一个函数里。

## 7、二级路由

1）首先确定二级路由页面(然后要在一级路由的页面中，引用二级路由的页面)

```java
import React, { Component } from 'react'
import Links from '../components/links'


//引入二级路由页面
import {BrowserRouter,Route,Link,Redirect,Switch} from 'react-router-dom';//引用react-router-dom的相关模块
import Era from './two/Era';
import Erb from './two/Erb';
import Erc from './two/Erc';
export default class List extends Component {
    render() {
        return (
            <div>
                <Links />
                2
                {/* 这个不用写Switch */}
                <Link to="/list/era">era</Link>
                <Link to="/list/erb">erb</Link>
                <Link to="/list/erc">erc</Link>


                <Route path="/list/era" component={Era}/>
                <Route path="/list/erb" component={Erb}/>
                <Route path="/list/erc" component={Erc}/>
            </div>
        )
    }
}
```

注：要在在已进入二级路由的页面，就显示二级路由，只需要在原一级路由的导航里面配置就好（path后面的那个 就是地址）

```java
<Link to="/一级路由地址/二级路由地址">list</Link>&nbsp;
```

# 三、路由传参（在谁的路由里面传，就在那个路由里面接收）

## 1、 路由---传参params方式

1）声明式
①在路由规则中定义参数
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200403003316439.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ1NjY5MTc4,size_16,color_FFFFFF,t_70)
②在路由导航发送参数
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200403003331612.png)
③接受数据(在需要的路由组件中接受)
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020040300335260.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ1NjY5MTc4,size_16,color_FFFFFF,t_70)
2）编程式
①在路由规则中定义参数
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200403003409882.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ1NjY5MTc4,size_16,color_FFFFFF,t_70)
②发送数据
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200403003426334.png)
③接收数据
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020040300344396.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ1NjY5MTc4,size_16,color_FFFFFF,t_70)
3）优势 ： 刷新地址栏，参数依然存在；
      缺点 ： 只能传字符串，并且，如果传的值太多的话，url会变得长而丑陋。

## 2、 路由---传参query

1）在Link中设置发送的数据
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200403003511226.png)
2）在需要接受的路由组建中接受
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200403003527556.png)
3）优势：传参优雅，传递参数可传对象；
     缺点：刷新地址栏，参数丢失
4）解决刷新丢失的问题，写在接收数据的组件中（利用sessionStorage的存储技术）

```java
    componentWillMount(){
        const {location}=this.props;
        let recvParam;
    
        if(location.query&&location.query.id){//判断当前有参数
            recvParam=location.query.id;
            sessionStorage.setItem('data',recvParam);// 存入到sessionStorage中
        }else{
            recvParam=sessionStorage.getItem('data');// 当state没有参数时，取sessionStorage中的参数
        }
        this.setState({
            recvParam
        })
    }
```

# 四、事件（this指向）

## 1、注意的点

1）React事件绑定属性的命名采用小驼峰式写法。
2）绑定函数的过程中不加() 否则函数会立即执行。

## 2、事件修改数据

```java
import React, { Component } from 'react'
export default class Xiushu extends Component {
    constructor(props){
        super(props);
        //1.定义状态数据
        this.state={
            name:"小明"
        }
    }
    // 4.函数的书写位置
    fun=()=>{//等价于 fun=function()但是这块要使用箭头函数，要不然修改数据会报错
        console.log("啊啊啊啊");
        this.setState({
            name:"小绿"
        })
    }
    render(){
        return (
            <div>
                <p>状态机</p>
                {/* 2.调用数据的时候 */}
                {this.state.name}
                {/* 3、点击修改数据的方法 */}
                <button onClick={this.fun}>点击我修改数据</button>
            </div>
        )
    }
}
```

## 3、修改this指向的修改

方式1：通过bind方法进行原地绑定，从而改变this指向；
方式2：通过创建箭头函数；
方式3：在constructor中提前对事件进行绑定；
方式4：将事件调用的写法改为箭头函数的形式。
1）前两种比较简单的

```java
import React, { Component } from 'react'
export default class This extends Component {
    // 1.创建一个普通函数
    funa=function(){
        console.log("通过bind方法进行原地绑定，从而改变this指向");
        console.log(this);
    }
    // 2.箭头函数
    funb=()=>{
        console.log(this);
    }
    render() {
        return (
            <div>
                {/* 1.这块吧this传递给了函数 */}
                <button onClick={this.funa.bind(this)}>1、通过bind方法进行原地绑定</button>
                {/* 2.通过创建箭头函数 */}
                <button onClick={this.funb}>2.通过创建箭头函数</button>
            </div>
        )
    }
}
```

2）难的

```java
import React, { Component } from 'react'
export default class This extends Component {
    // 3-2在constructor中提前对事件进行绑定
    constructor(props){
        super(props);
        //3-3、调用函数(提前绑定，通过bind传给他)
        this.func=this.func.bind(this)
    }

    // 1.创建一个普通函数
    funa=function(){
        console.log("通过bind方法进行原地绑定，从而改变this指向");
        console.log(this);
    }
    // 2.箭头函数
    funb=()=>{
        console.log(this);
    }

    // 3-4.在constructor中提前对事件进行绑定
    func=function(){
        console.log(this);
    }
    // 4.
    fund=function(){
        console.log(this);
    }

    render() {
        return (
            <div>
                {/* 1.这块吧this传递给了函数 */}
                <button onClick={this.funa.bind(this)}>1、通过bind方法进行原地绑定</button>
                {/* 2.通过创建箭头函数 */}
                <button onClick={this.funb}>2.通过创建箭头函数</button>
                {/* 3-1 在constructor中提前对事件进行绑定*/}
                <button onClick={this.func}>3.在constructor中提前对事件进行绑定</button>
                {/* 4.将事件调用的写法改为箭头函数的形式 (代表在函数里面调用一个函数)*/}
                <button onClick={()=>{this.fund()}}>4.将事件调用的写法改为箭头函数的形式</button>
            </div>
        )
    }
}
```

# 五、状态机

## 1、介绍

1）在react中开发者只需要关心数据。数据改变页面就会发生改变;
2）数据等同于状态。状态改变-页面绑定的数据就由react进行改变;
3) 组件被称之为“状态机”，通过更新组件的state来更新对应页面的显示（从新渲染组件 不需要要操作 DOM。

## 2、super的作用

1）constructor--super
①ES6的继承规则得知，不管子类写不写constructor，在new实例的过程都会给补上constructor。
②可以不写constructor，一旦写了constructor，就必须在此函数中写super(),此时组件才有自己的this，在组件的全局中都可以使用this关键字，否则如果只是constructor 而不执行 super() 那么以后的this都是错的！！！super()继承父组件的 this
2） super(props)
        当想在constructor中使用this.props的时候，super需要加入(props)，此时用props也行，用this.props也行，他俩都是一个东西。（不过props可以是任意参数，this.props是固定写法）。

## 3、用法

```java
import React, { Component } from 'react'


export default class Xiushu extends Component {
    constructor(props){
        super(props);
        //1.定义状态数据
        this.state={
            name:"小明"
        }
    }
    // 4.函数的书写位置
    fun=()=>{//等价于 fun=function()但是这块要使用箭头函数，要不然修改数据会报错
        console.log("啊啊啊啊");
        this.setState({
            name:"小绿"
        })
    }
    render(){
        return (
            <div>
                <p>状态机</p>
                {/* 2.调用数据的时候 */}
                {this.state.name}
                {/* 3、点击修改数据的方法 */}
                <button onClick={this.fun}>点击我修改数据</button>
            </div>
        )
    }
}
```

注：有状态的情况下，不能创建函数组件。

# 六、 React 表单

## 1、React负责渲染表单的组件。同时仍然控制用户后续输入时所发生的变化。相应的，其值由React控制的输入表单元素称为“受控组件”

## 2、 表单双向绑定效果案例（同步修改输入框输入内容值改变）

```java
import React, { Component } from 'react'
export default class Shangxiang extends Component {
    constructor(props){
        super(props);
        // 1、创建数据
        this.state={
            text:""
        }
    }
    // 4、修改数据(穿一个形参)
    fun=(e)=>{
        this.setState({
            text:e.target.value// 使用事件对象
        })
    }
    render() {
        return (
            <div>
                {/* 3、绑定事件 */}
                <input type="text" onInput={this.fun}/>
                <br/>
                {/* 2、绑定数据 */}
                <p>{this.state.text}</p>
            </div>
        )
    }
}
```

# 七、 react路由懒加载（异步组件）------[react-loadable](https://www.npmjs.com/package/react-loadable)

## 1、路由的封装

1）在src——>pages里创建路由页面（页面名首字母大写），示例页面如下：

```java
import React, { Component } from 'react'

export default class Mine extends Component {
    render() {
        return (
            <div>
                
            </div>
        )
    }
}
```

2）在src——>router——>index.jsx来存放路由

```java
import React, { Component } from 'react'
import {BrowserRouter,Route,Link,NavLink,Switch,Redirect} from "react-router-dom";
//引入页面
import Index from '../views/Index'
import Mine from '../views/Mine'
export default class index extends Component {
    render() {
        return (
            <div>
                <BrowserRouter>
                    <Switch>
                        <Route path="/index" component={Index} />
                        <Route path="/mine" component={Mine} />
                    </Switch>
                    <Redirect to="/index"/>
                </BrowserRouter>
            </div>
        )
    }
}
```

3）这样就可以删除App.js了；但是要在index.js里进行修改，因为这样写就不可以引入App.js了

```java
import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';
import Index from "../src/router/index";
import * as serviceWorker from './serviceWorker';

ReactDOM.render(<Index />, document.getElementById('root'));

// If you want your app to work offline and load faster, you can change
// unregister() to register() below. Note this comes with some pitfalls.
// Learn more about service workers: https://bit.ly/CRA-PWA
serviceWorker.unregister();
```

4）在写一个组件来存放导航（components——>navigation.jsx）

```java
import React, { Component } from 'react'
import {Link,NavLink} from "react-router-dom";
export default class navigation extends Component {
    render() {
        return (
            <div>
                <Link to="/index">首页</Link>
                <Link to="/mine">我的</Link>
            </div>
        )
    }
```

注：①上面这个有这个报错，不影响整体![在这里插入图片描述](https://img-blog.csdnimg.cn/20200403004156753.png)
②也可以使用NavLink，可以设置当行的样式类（因为他有一个默认的样式类）；写法把Link换成NavLink就好。

5)在路由页面中进行使用，封装的路由组件

```java
import React, { Component } from 'react'
import Navigation from '../components/navigation'
export default class Mine extends Component {
    render() {
        return (
            <div>
                <Navigation />
                <p>我的</p>
            </div>
        )
    }
}
```

## 2、[懒加载](https://www.npmjs.com/package/react-loadable)

1）下载：
npm install --save react-loadable
2）在src——>util——>loadable.js

```java
//创建异步组件/路由懒加载的配置文件
import React from 'react';
// 全是从react-loadable中释放出来的
import Loadable from 'react-loadable';

//通用的过场组件
const loadingComponent =()=>{
    return (
        // 这块可以在路由切换的时候，加入一个动画，实现了 按需加载（即过场动画）
        <div>loading</div>
    )
}

//过场组件默认采用通用的，若传入了loading，则采用传入的过场组件
export default (loader,loading = loadingComponent)=>{
    return Loadable({
        loader,
        loading
    });
}
```

3）在存放的路由的组件进行使用

```java
import React, { Component } from 'react'
import {BrowserRouter,Route,Switch,Redirect} from "react-router-dom";
//传统引入页面
/*
import Index from '../views/Index'
import Mine from '../views/Mine'
*/
// 懒加载的时候引用方式
import loadable from "../util/loadable";//引用那个路由懒加载文件
const Index = loadable(()=>import('../views/Index'));
const Mine = loadable(()=>import('../views/Mine'));

export default class index extends Component {
    render() {
        return (
            <div>
                <BrowserRouter>
                    <Switch>
                        <Route path="/index" component={Index} />
                        <Route path="/mine" component={Mine} />
                    </Switch>
                    <Redirect to="/index"/>
                </BrowserRouter>
            </div>
        )
    }
}
```

注：这样子，会给他一个懒加载，让他在点击的时候，在去渲染别的。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200403004402726.png)

# 八、redux

注：[①](http://www.ruanyifeng.com/blog/2016/09/redux_tutorial_part_one_basic_usages.html)      [②](http://www.ruanyifeng.com/blog/2016/09/redux_tutorial_part_two_async_operations.html)      [③](http://www.ruanyifeng.com/blog/2016/09/redux_tutorial_part_three_react-redux.html)

文档：<http://cn.redux.js.org/>

## 1、 redux是什么

1）Redux是为javascript应用程序提供一个可预测（根据一个固定的输入，必然会得到一个固定的结果）的状态容器。
2）集中的管理react中多个组件的状态。
3）redux是专门作状态管理的js库（不是react插件库可以用在其他js框架中例如vue，但是基本用在react中）。

## 2、 什么时候用redux？

1）某个组件的状态需要共享的时候；
2）某个组件的状态需要在任何地方都可以拿到；
3）一个组件需要改变全局状态；
4)一个组件需要改变另一个组件的状态。

## 3、 redux三大原则

1）单一数据源:整个应用的 state 被储存在一棵 object tree 中，并且这个 object tree 只存在于唯一一个 store 中；
2）State 是只读的:唯一改变 state 的方法就是触发 action，action 是一个用于描述已发生事件的普通对象；
3）使用纯函数来执行修改:为了描述 action 如何改变 state tree ，你需要编写 reducers(一些纯函数，它接收先前的 state 和 action)。

## 4、 redux常用概念

1）Store：管理着整个应用的状态，Store提供了一个方法dispatch()，这个就是用来发送一个动作，去修改Store里面的状态，然后可以通过getState()来重新获得最新的状态(state)；
2）Action：是唯一可以改变状态(state)的方式，服务器的各种推送、用户自己做的一些操作，最终都会转换成一个个的Action，而且这些Action会按顺序执行；
3）Reducer：是用来修改状态的。Reducer 是一个函数，它接受 Action 和当前 State 作为参数，返回一个新的 State。

## 5、 redux常用方法

1）createStore（）作用：创建一个Redux store来存放应用中所有的state，一个应用只能有个store。函数返回store对象；
2）getState()作用：获取数据；
3）dispatch()分发action，这是改变state的唯一方法；
4）subscribe()添加一个变化监听器，每当改变store的时候就会执行。

## 6、 redux执行流程

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200403004742520.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ1NjY5MTc4,size_16,color_FFFFFF,t_70)

## 7、使用

### 1）下载

npm install --save redux

### 2）创建文件redux

①redux——>reducer.js   设置数据处理返回结果

```java
//创建数据
var obj={
    name:"小明",age:22
}
//es6中的暴露;(data中必须要有俩参数，用es6中的形参默认值)
export function data(state=obj.age,action){//action来调用修改的方式
    // 稍后会有很多的修改操作  action.type  判断具体是那个修改操作
    switch(action.type){//type是类型（意思是  你到底有那个修改的类型）
        default:
            return state;//返回 的结果
            break;
    }
}
```

②创建store（redux——>store.js）

```java
//结构（再用export暴露的时候，要用结构）
import {createStore} from "redux";
//引入暴露的data
import {data} from "./reducer"
//实例化一下，在外面就可以使用了
export var store=createStore(data);//吧data传进来，用模块的方式关联起来
```

③在组件中调用

```javascript
import React, { Component } from 'react'
import {store} from '../redux/store'
export default class redux extends Component {    
 constructor(props){        
 super(props);        
  this.state={            
   text:store.getState()        
  }    
 }    
 render() {        
  return (            
   <div>                
    <h1>Rudex</h1>                
    {this.state.text}            
   </div>        
  )    
 }
}
```

### 3）修改数据

①在src——>redux——>action.js

```javascript
//暴露一个函数，进行修改操作，然后接受进来一个形参，稍后在页面 调用的时候，传递进来的形参
export var add=(num)=>{//这个是添加
    //retrn出来一个对象，这个对象里面有修改操作的名字和数据
    return {type:"ADD",data:num}
}
export var del=(num)=>{//这个是删除
    //retrn出来一个对象，这个对象里面有修改操作的名字和数据
    return {type:"DEL",data:num}
}
```

②在src——>redux——>reduce.js里面添加判断，调用action.js里的东西进行数据的修改

```javascript
//创建数据
var obj={
    name:"小明",age:22
}
//es6中的暴露;(data中必须要有俩参数，用es6中的形参默认值)
export function data(state=obj.age,action){//action来调用修改的方式
    // 稍后会有很多的修改操作  action.type  判断具体是那个修改操作
    switch(action.type){//type是类型（意思是  你到底有那个修改的类型）
        //tian加修改数据的操作
        case "ADD":
            console.log(state+action.data);
            return state+action.data;
            break;
        case "DEL":
            console.log(state-action.data);
            return state-action.data;
            break;
        default:
            return state;//返回 的结果
            break;
    }
}
```

③在页面中引用 action.js
注：a、import * as action星号是匹配符，匹配这个action中的所有export    function暴露的函数。
       b、同时也要在页面中进行监听；
       c、在调用的时候，都要加上修改的监听，负责没加的页面 或者组件里的数据不会改变。

```javascript
import React, { Component } from 'react';
//下面他俩要放到正数的第二位，负责会报错
import {store} from '../redux/store'
//引入action里面所有的东西,chu发用dispath
import * as action from '../redux/action';


export default class redux extends Component {
    constructor(props){
        super(props);
        this.state={
            text:store.getState()
        }
    }
    componentWillMount(){
        //监听
        store.subscribe(()=>{
            //修改操作
            this.setState({
                text:store.getState()
            })
        })
    }
    render() {
        return (
            <div>
                <h1>Rudex</h1>
                {this.state.text}
                {/* 修改数据 */}
                {/* 点击执行添加操作，并且加1，并且传个一 给num */}
                <button onClick={()=>{store.dispatch(action.add(1))}}>点我+1</button>
                <button onClick={()=>{store.dispatch(action.del(1))}}>点我-1</button>
            </div>
        )
    }
}
```

### 4）redux数据请求，在组件里面进行数据请求然后再发送给redux

①redux——>reducer.js

```javascript
var obj={
    name:"小明",age:22
}

export function data(state=obj,action){
    switch(action.type){
        default:
            return state;
            break;
    }
}
```

②进行暴露redux——>store.js

```javascript
import {createStore} from 'redux';

import {data} from './reducer';

export var store = createStore(data);
```

③在A组件里面请求数据，并发送到Redux里

```javascript
import React, { Component } from 'react';
import axios from "axios";
// 1.使用redux
import {store} from '../../redux/store';
import * as action from '../../redux/action';


export default class whole extends Component {
    constructor(props){
        super(props);
        this.state={
            dataone:"",
            // 2.使用redux  这个是接收测试的数据，不要在意
            text:store.getState()
        }
    }
    //取得点击事件的事件源，得到value里面的值
    funa=(e)=>{
        console.log(e.target.value);
        this.setState({
            dataone:e.target.value
        })
    }

    componentWillMount(){
        //console.log(this.state.text);
        axios({
            url:'/paging/one',
            method:'get'
        }).then((ok)=>{
            console.log(ok);
            //这个是发送数据
            store.dispatch(action.add(ok));
        })
    }

    render() {
        return (
            <div>
                <div>
                    <button value="a">全部</button>
                    <p>3.redux</p>
                    {/* {this.state.text} */}
                </div>
            </div>
        )
    }
}
```

④在redux——>action.js

```javascript
export var add=(num)=>{//在A组件里  他会自动调用这个函数
    return {type:'EEL',data:num}
}
```

⑤redux——>reducer.js

```javascript
var obj={
    name:"小明",age:22
}

export function data(state=obj,action){
    switch(action.type){
        case 'EEL':
            console.log(action.data);
            return action.data;//它会覆盖上面obj里面的数据，但是  并不会 改变原数据
        default:
            return state;
            break;
    }
}
```

⑥在B组件中接收使用

```javascript
import React, { Component } from 'react';
import { store } from '../redux/store';
export default class Mine extends Component {
    constructor(props){
        super(props);
        this.state={
            lala:store.getState()
        }
    }
    componentWillMount(){
       /* 在这种情况下，加监听会造成 内存泄漏，也会报错；  但是在你的数据，一直发生变化的时候，你就要进行数据监听；
       比如 筛选按钮点击的时候，这块就是点击一次按钮，修改一次数据，所有一定要监听数据的变化，否则就是一直都是初始值
       */
        // store.subscribe(()=>{
        //     this.setState({
        //         lala:store.getState()
        //     })
        // })
        console.log(this.state.lala);
    }
    render() {
        return (
            <div>
               
            </div>
        )
    }
}
```

## 8、redux插件的使用

1）谷歌插件商城下载：

Redux DevTools

![image-20210625101815861](https://i.loli.net/2021/06/25/EOTJvLp38AagQlb.png)

注：安装完成后，如果在控制台没看到Redux，请关闭浏览器 再打开。

2）这个插件需要在项目里 安装依赖

```text
npm install redux-devtools-extension --save-dev
```

3）[使用](https://github.com/zalmoxisus/redux-devtools-extension)

```javascript
import React from 'react';
import ReactDOM from 'react-dom';
import App from './App';
// redux的提示
import logger from "redux-logger";
import thunk from "redux-thunk";
import { composeWithDevTools } from 'redux-devtools-extension';
import { createStore, applyMiddleware } from "redux";
import { Provider } from "react-redux";
// 西面是redux文件管理器
import rootReduce from "./reducers/index.js";

const store = createStore(rootReduce, composeWithDevTools(applyMiddleware(logger, thunk)));

ReactDOM.render(
    <Provider store={store}>
        <App />
    </Provider>,
    document.getElementById('root'));
```

# 九、UI组件的引入

## 1、[AntdUI](https://ant.design/docs/react/use-with-create-react-app-cn)的使用

1）下载
npm install antd --save
2）在App.css里面引入样式

```javascript
@import '~antd/dist/antd.css';
```

注：

也可以在index.js中引入：

```javascript
import 'antd/dist/antd.css'; // or 'antd/dist/antd.less'
```

3）在组件或者页面中引用

```javascript
import {标签名} from 'antd';
```

## 2、按需加载

### 1）手动的

在页面或者组件中直接引入：

```react
import './App.css';
import { Button } from 'antd';
import "antd/es/button/style/css";
function App() {
    return (
        <div className="App">
            <Button>222222</Button>
        </div>
    );
}

export default App;

```

### 2）自动加载

①首先暴露出react的webpack的配置，操作是不可逆的，并且建议在项目刚创建之后操作（文件的结构不能发生改变的时候才能暴露）。

```text
npm run eject
```

②安装插件

```text
npm install babel-plugin-import --save-dev
```

③在package.json中的 *babel* 加入如下代码：

```json
  "babel": {
    "presets": [
      "react-app"
    ],
    "plugins": [
      [
        "import",
        {
          "libraryName": "antd",
          "libraryDirectory": "es",
          "style": "css"
        }
      ]
    ]
  },
```

![image-20210611173025329](https://i.loli.net/2021/06/11/gW3wnAos9O52Vju.png)

# 十、对数据进行处理

## 1、从后台请求数据，根据不同的后端返回的不同字段，进行筛选自己需要的数据（以年份为例）

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200403005543477.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ1NjY5MTc4,size_16,color_FFFFFF,t_70)

```javascript
import React, { Component } from 'react'
import axios from "axios";
export default class whole extends Component {    
 constructor(props){         
 super(props);        
  this.state={            
   list:[]        
  }    
 }    
 componentWillMount(){        
  axios({            
   url:'/paging/one',            
   method:'get'        
  }).then((ok)=>{            
   console.log(ok.data.datalist);            
   this.setState({                
    list:ok.data.datalist            
   })        
  })    
 }    
 ck20(e) {        
  //每点击一次  请求一次数据        
  axios({            
   url:'/paging/one',            
   method:'get'        
  }).then((ok)=>{            
   console.log(ok.data.datalist);            
   //定义一个空数组            
   let arr = [];            
   //循环本来的数组            
   for(let i=0;i<ok.data.datalist.length;i++) {                
   //如果便利出来的year 恒等于 点击事件传过来的参数 
    if(ok.data.datalist[i].year===e) {                    
     //把相等的push到arr里，并且把他赋值给list
     arr.push(ok.data.datalist[i])                    
     this.setState({                        
      list:arr                    
     })                
    }            
   }        
  })    
 }    
 render() {        
  return (            
   <div>               
    <button value="a">全部</Radio.Button>               
    <button value="a" onClick={()=>{this.ck20(2020)}}>2020年</button>
    <button value="b" onClick={()=>{this.ck20(2019)}}>2019年</button>
    <button value="c" onClick={()=>{this.ck20(2018)}}>2018年</button>
   </div>        
  )    
 }
}
```

## 2、页面跳转，进行传参，从列表页，跳转到详情页

1)列表页

```javascript
<Link to={{pathname:'路径',query:{id:要传的变量}}}>列表内容</Link>
```

2）在详情页接收

```javascript
    componentWillMount(){
        const {location}=this.props;
        let recvParam;
        if(location.query&&location.query.id){//判断当前有参数
            recvParam=location.query.id;
            sessionStorage.setItem('data',recvParam);// 存入到sessionStorage中
        }else{
            recvParam=sessionStorage.getItem('data');// 当state没有参数时，取sessionStorage中的参数
        }
        console.log(recvParam);
    }
```

3）也可根据id在详情页进行筛选，选择要在详情页进行展示的数据
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200403010052440.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200403010101956.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ1NjY5MTc4,size_16,color_FFFFFF,t_70)

## 3、数据筛选

（类似于淘宝的  按条件 显示商品，这个一般是后台来做的，没点击一次分类按钮，向后台发送一个数据，然后接受数据，进行展示）
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200403010134902.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200403010142234.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ1NjY5MTc4,size_16,color_FFFFFF,t_70)

```javascript
import React, { Component } from 'react'

import axios from "axios";
export default class whole extends Component {
    constructor(props){
        super(props);
        this.state={
            dataone:""
        }
    }
    //取得点击事件的事件源，得到value里面的值
    funa=(e)=>{
        console.log(e.target.value);
        this.setState({
            dataone:e.target.value
        })
    }
    componentWillMount(){
        axios({
            url:'/paging/one',
            method:'get'
        }).then((ok)=>{
            console.log(ok.data.datalist);
            this.setState({
                list:ok.data.datalist
            })            
        })
    }

    ck20(e) {
        axios({
            url:'/paging/one',
            method:'get'
        }).then((ok)=>{
            console.log(ok.data.datalist);
            let arr = []
            for(let i=0;i<ok.data.datalist.length;i++) {
                if(ok.data.datalist[i].year === e) {
                    arr.push(ok.data.datalist[i])
                    this.setState({
                        list:arr
                    })
                }
            }
        })
    }

    render() {
        return (
            <div>
                 
                 <button value="1" onClick={()=>{this.ck20(2020)}}>2020年<button>
                 <button value="2" onClick={()=>{this.ck20(2019)}}>2019年<button>
                
            </div>
        )
    }
}
```

# 十一、Iconfont的引入

## 1、首先进入阿里矢量图标官网，把需要的图标添加到项目，然后下载至本地

## 2、font-class 引用

1）首先打开下载好的那个文件夹里面的 .html文件；
2）在public静态文件资源里的index.html文件里引入第一步；
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200403010234178.png)
3）把下载好的那个文件夹里的 iconfont.css 拖入public文件夹里；
4）直接在组件或者页面里使用。

```javascript
<span class="iconfont icon-对应的图标名"></span>
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200403010304417.png)

## 3、Symbol 引用

1）首先打开下载好的那个文件夹里面的 .html文件；
2）在public静态文件资源里的index.html文件里引入第一步；
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200403010324888.png)
3）把下载好的那个文件夹里面的 iconfont.js 拖入public文件夹里；
4）加入通用 CSS 代码（引入一次就行）,一般写在App.css这个全局样式里；
5）在组件或者页面中，直接使用（但是要去掉 xlink:）。

```javascript
<svg class="icon" aria-hidden="true">
  <use href="#icon-图标的类名"></use>
</svg>
```

## 4、iconfont图标与文字中心线对齐，使用

```css
    vertical-align:middle;
```

# 十二、回到顶部、两侧跟随广告

## 1、回到顶部

```javascript
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>
    <div>
        <p>111111111111111</p>
        。。。。。。
        <p>111111111111111</p>
    </div>
    <button onclick="fun()">按钮</button>


</body>
</html>

<script>
    var isTop = null;
    window.onscroll = function(){
        //兼容问题  获取滚动条移动的垂直距离
        isTop = document.body.scrollTop || document.documentElement.scrollTop;
        console.log(isTop);
    }
    function fun(){
        document.body.scrollTop = document.documentElement.scrollTop = 0;
    }
</script>
```

## 2、两侧跟随广告

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
    <img src="图片地址" alt="" id="pic"/>
    <div id="demo">
       <p>天王盖地虎，小鸡炖蘑菇</p>
       。 。。。。。
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
            if( obj.offsetTop == target ){
                clearInterval( timer );
            }else{
                obj.style.top = obj.offsetTop + speed + "px";
            }
        },30)
    }
</script>
```

## 3、弹出框

1）index.css

```css
#teacher{
    height:200px;
    width:200px;
    background-color: aqua;
    position: absolute;
    display: none;
}
```

2）index.jsx（这个弹出框  水平垂直都居中）

```javascript
import React, { Component } from 'react'
import './index.css';
export default class login extends Component {    
 constructor(props){        
 super(props);        
  this.state={                    
  
  }    
 }    
 teach=()=>{        
  let teacher = document.getElementById('teacher');        
  teacher.style.display='block';        
  teacher.style.left=window.innerWidth/2 - teacher.offsetWidth/2 + 'px';
  teacher.style.top=window.innerHeight/2 - teacher.offsetHeight/2 + 'px';        
  // 调用按钮的函数        
  this.creatBtn();    
 }    
 // 创建按钮，关闭他出框    
 creatBtn(){        
  let teacher = document.getElementById('teacher');        
  let btnDiv = null;        
  // 创建一个按钮(先给一个空，也可以定义到state里)        
  btnDiv = document.createElement("button");        
  btnDiv.innerHTML='×';        
  btnDiv.style.width='50px';        
  btnDiv.style.height='50px';        
  // 给按钮一个定位        
  btnDiv.style.position='absolute';        
  // 向teacher里添加按钮        
  teacher.appendChild(btnDiv);        
  // 确定这个按钮，所在位置        
  btnDiv.style.left = teacher.offsetWidth - btnDiv.offsetWidth + 'px';        
  // 点击按钮，关闭弹出框（也可以加一个点击弹出框以外的 也关闭弹出框）
  btnDiv.addEventListener('click', function() {
    //给js动态创建的dom添加事件            
    teacher.style.display='none';        
   })    
 }        
 render() {        
  return (            
   <div>                
    <button onClick={this.teach}>教师注册</button>                
     <div id='teacher'>                                    
     </div>            

     </div>        
  )    
 }
}
```

## 4、进度条

```javascript
<!doctype html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title></title>
<style type="text/css" >
    #box{
        width: 200px;
        height: 30px;
        border: 1px solid black;
    }
    #jinDuBox{
        width: 0px;
        height: 100%;
        background-color: skyblue;
        text-align: center;
    }
</style>
</head>
<body style="height: 1000px">
    <div id="box">
        <div id="jinDuBox">


        </div>
    </div>
    <input type="button" value=" 走你 " onclick="testf()" />
</body>
</html>


<script type="text/javascript">
var width =0; //保存当前的宽度


function testf(){
   var myTimer = setInterval(function(){
        width+=2;
        if(width>=200){
            width = 200;
            window.clearInterval(myTimer);
        }
        $("jinDuBox").style.width = width+"px";
        $("jinDuBox").innerHTML = parseInt(width/200*100)+"%";
   },100);
}


function $(id){
    return document.getElementById(id);
}
</script>
```

# 十三、react中[less](https://blog.csdn.net/qq_25520603/article/details/90206399)的[使用](https://www.jianshu.com/p/94ac7250ccf0)

## 1、下载

     npm install less less-loader --save-dev 或者 yarn add less less-loader --save-dev；

## 2、运行

   npm run eject   来暴露webpack的配置文件，你会发现多了config为名的文件夹；
注：如果这步报错，请运行： 运行 git add .    命令，然后再运行  git commit -m "init"    命令，如果这步还是报错，就是没有输入git的用户名 和 邮箱；

## 3、找到config——>webpack.config.js在里面进行修改（一共修改三处）

1）添加less配置
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020040301143178.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ1NjY5MTc4,size_16,color_FFFFFF,t_70)

```javascript
//添加less配置
const lessRegex = /\.less$/;
const lessModuleRegex = /\.module\.less$/;
```

2） 参数lessOptions为添加项
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200403011507533.png)

```javascript
lessOptions,
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200403011540175.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ1NjY5MTc4,size_16,color_FFFFFF,t_70)

```javascript
      // 添加 Less 配置
      {
        loader: require.resolve('less-loader'),
        options: lessOptions,
      },
```

3）less的配置
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200403011615125.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ1NjY5MTc4,size_16,color_FFFFFF,t_70)

```javascript
            // less的配置
            // 添加less配置
            {
              test: lessRegex,
              exclude: lessModuleRegex,
              use: getStyleLoaders(
              {
                  importLoaders: 2,
                  sourceMap: isEnvProduction && shouldUseSourceMap,
              },
              'less-loader'
              ),
              sideEffects: true,
            },
            {
              test: lessModuleRegex,
              use: getStyleLoaders(
              {
                  importLoaders: 2,
                  sourceMap: isEnvProduction && shouldUseSourceMap,
                  modules: true,
                  getLocalIdent: getCSSModuleLocalIdent,
              },
              'less-loader'
              ),
            },
```

## 4、重启项目

  npm start
注：报错解决：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200403011701893.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200403011710282.png)

## 5、less的使用

<https://less.bootcss.com/>

# 十四、登录注册

## 1、点击👀控制密码的显示和显示黑点，点击按钮 得到输入框的内容

```javascript
import React, { Component } from 'react';

export default class log extends Component {
    //组件传值  要是在不用的时候，加  这个会报错
    // static propTypes = {
    //     prop: PropTypes
    // }
    constructor(props){
        super(props);
        this.xiaoming=React.createRef();
        this.state={
            types:"password",
            text:'密码'
           
        }
    }
    handleOk = () => {
        console.log(this.xiaoming.current.value);
    }

    tap=()=>{
        if(this.state.types=='password'){
            this.setState({
                types:'text',
                text:"明文"
            })
        }else{
            this.setState({
                types:'password',
                text:"密文"
            })
        }


    }
    render() {
        return (
            <div>
                <input type={this.state.types} placeholder="密码" ref={this.xiaoming} />
                <button onClick={this.tap}>dian</button>
                
                <Button onClick={this.handleOk}>点击它，可以得到上面输入框的内容</Button>  
            </div>
        )
    }
}
```

## 2、对页面进行判定，没有登录，不允许进入此页面

```javascript
在这里插入代码片

```

## 3、手机号验证，和验证码发送

```javascript
import React, { Component } from 'react'
import "./logon.css";
import Linkage from "../linkage/linkage"
export default class logon extends Component {
    constructor(props){
        super(props);
        // 手机号
        this.cell_phone=React.createRef();
        // 验证码
        this.verification_number=React.createRef();

        this.state={
            // 按钮的
            click:null,
            //倒计时的时间
            hour:''
        }
    }
    // 倒计时  60秒；
    time=()=>{
        // 在这块先要得到手机号，然后去判定手机号，当手机号，格式不否和的时候，给一个提示

        let phone=this.cell_phone.current.value;
        //手机号  正则
        if(!(/^1[3456789]\d{9}$/.test(phone))){ 
            alert("手机号码有误，请重填");  
            // 把手机号的输入框置为空
            this.cell_phone.current.value=''
            return false; 
        }else{
            console.log(phone);
        }
        let  hour = 60;
        let timeClick;
        timeClick=setInterval(()=>{
            hour  --;
            this.setState({
                hour: hour,
                // 当他在60秒以内的时候，给button一个禁止点击事件
                click:'disabled'
            })
            // 如果小于零，清楚定时器
            if(hour===0){
                // 清除定时器
                clearInterval(timeClick);
                this.setState({
                    click:null
                })
            }
        },1000)
    }

    
    render() {
        // 给这块一个秒数倒计时
        let newhtml='';
        if(this.state.hour != ''){
            newhtml = this.state.hour + 's后重新发送'
        }else if(this.state.hour===0){
            newhtml = '请重新获取'
        }else{
            newhtml = ''
        }
        return (
            <div>
             <p>
                 手机号：<input type="text" ref={this.cell_phone} />
                    <button type="submit" onClick={this.time} disabled={click} >获取验证码</button>
                </p>
                  {/* 这块写手机号错误时候的提示 */}
                  
                <p>
                 验证码：<input type="text" ref={this.verification_number} />
                    <i className="after">{newhtml}</i>
                </p> 
            </div>
        )
    }
}

```

## 4、让页面加载完成的时候或别的情况下，光标自动进入输入框

```javascript
import React, { Component } from 'react'
export default class foot extends Component {
    // 组件渲染之后调用（数据请求一般 写在这个里面）
    componentDidMount(){
        this.input.focus();
    }
    render() {
        return (
            <div>
               <input type="text" ref={(input) => this.input = input} />
            </div>
        )
    }
}
```

# 十五、withRouter

  withRouter 高阶组件，也叫：HOC ; 参数是一个组件,同时返回的也是一个组件,zhe类组件我们称为高阶组件。 就是让不是路由切换的组件 也具有路由切换组件的三个属性（location match history）。

# 十六、页面传参

## 1、A页面或者组件发送数据，B页面接收

1）A发送数据

```java
<Link to={{pathname:'/路径',query:{id:"要发送的数据"}}}>发送数据 </Link>
```

2)在B页面进行接收

```java
 //解决刷新页面数据丢失的问题
 componentWillMount(){
        // console.log(this.props.location.query.id);
        const {location} = this.props;
        // hot news 热点新闻
        let hotNews;
        if(location.query && location.query.id){
            hotNews=location.query.id;
            //存到sessionStorage里
            sessionStorage.setItem("hotNewsId",hotNews)
        }else{
         //从sessionStorage里取出
            hotNews=sessionStorage.getItem("hotNewsId")
        }
        console.log(hotNews);
    }
```

## 2、在A页面跳转到B页面，在A页面的组件里发送数据

1）在A页面中

```javascript
import React, { Component } from 'react'
import Match from '../components/组件名';
export default class Notice extends Component {
    render() {
        return (
            <div>
                <组件名 history={this.props.history} />
            </div>
        )
    }
}
```

注：如果不在父里面写这个，他会报一个push未定义的错。
2）在A页面的组件里

```javascript
import React, { Component } from 'react'
export default class Notice extends Component {
   search=()=>{
      // 传参
      this.props.history.push({
        pathname:"/路径",
        //可以发变量、字符串等
        query:{id:要发送的数据}
      });
    }
    render() {
        return (
            <div>
                <button type="submit" onClick={this.fun}>点击</button>
            </div>
        )
    }
}
```

3）在B页面中接收

```javascript
    componentWillMount(){
        console.log(this.props.location.query.id);
    }
```

注：它也会刷新数据丢失，解决的方法如上，思路是把它传过来的数据，存入sessionStorage，当刷新页面后，从sessionStorage里取。

## 3、多个数据传递

1）在A页面发送数据

```javascript
import React, { Component } from 'react'
export default class screen extends Component {
    fun=()=>{
        console.log("aaaaaaaa");
        //传参
        this.props.history.push({
          pathname:"/particulars",
          query:{XXX:{
              title:"啊啊啊啊",
              text:"哈哈哈哈"
          }}
        })
    }
    render() {
        return (
            <div>
        <button type="submit" onClick={this.fun}>搜索</button>
            </div>
        )
    }
}

```

2）在B页面进行接收

```javascript
import React, { Component } from 'react'
export default class particulars extends Component {
    constructor(props){
        super(props);
        this.state={
            data:[]
        }
    }
    //解决刷新页面数据丢失的问题
    componentWillMount(){
        // console.log(this.props.location.query.particularsid);
        const {location} = this.props;
        // particularsid作品展示
        let recvParam;
        if(location.query && location.query.XXX){
            recvParam = location.query.XXX;
            //存到sessionStorage里
            sessionStorage.setItem("XXX",JSON.stringify(recvParam));
        }else{
            //从sessionStorage里取出
            recvParam=JSON.parse(sessionStorage.getItem("XXX"));
        }
        console.log(recvParam);
        this.setState({
            // 列表页传过来的搜索数据
            data:recvParam
        })
    }

    render() {
        return (
            <div>
                <div className="">
     {data.title}   {* 这块展示的是     啊啊啊啊 *}
                </div>
            </div>
        )
    }
}

```

# 十七、React链接转二维码、插入视频

## 1、链接转二维码

1）下载
npm install qrcode.react --save
2）在组件或者页面中使用

```javascript
import React, { Component } from 'react'
import QRCode  from 'qrcode.react';
export default class Presentation extends Component {
constructor(props){
      super(props);
      this.state={
          qrUrl:'链接地址'
      }
    }
    render() {
        return (
            <div>
               <QRCode
          value={this.state.qrUrl}  //value参数为生成二维码的链接
          size={200} //二维码的宽高尺寸
          fgColor="red"  //二维码的颜色
         />
            </div>
        )
    }
}
```

注：要修改样式的话，直接加类名 就好。

## 2、[插入视频](https://video-react.js.org/components/player/)

1）下载
npm install –save video-react
2）引入
import { Player } from 'video-react';
import "../node_modules/video-react/dist/video-react.css";//这块是绝对路径
3）使用

```javascript
import React, { Component } from 'react'
import { Player } from 'video-react';
import "../../node_modules/video-react/dist/video-react.css";
export default class Index extends Component {
    render() {
        return (
            <div>
                <Player ref="player" videoId="video-1" fluid={true} >
                    <source src="https://blz-videos.nosdn.127.net/1/OverWatch/AnimatedShots/Overwatch_AnimatedShot_Soldier76_Hero.mp4" />
                </Player>
            </div>
        )
    }
}

```

注：各种属性的修改，请查看链接文档。

## 3、video画中画的使用（原生）

```javascript
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title></title>
    <style>
        .box{
            height:200px;
            width:350px;
        }
    </style>
</head>
<body>
    <video id="video" src="https://blz-videos.nosdn.127.net/1/OverWatch/AnimatedShots/Overwatch_AnimatedShot_Soldier76_Hero.mp4" controls playsinline loop class="box"></video>         
    <input type="button" id="pipBtn" value="点击进入画中画状态">
</body>
</html>
<script>
    // let pipBtn=document.getElementById("pipBtn");
    var pipWindow;

    pipBtn.addEventListener('click', function(event) {
        console.log('切换Picture-in-Picture模式...');
        // 禁用按钮，防止二次点击
        this.disabled = true;
        try {
            if (video !== document.pictureInPictureElement) {
            // 尝试进入画中画模式
            video.requestPictureInPicture();      
            } else {
            // 退出画中画
            document.exitPictureInPicture();
            }
        } catch(error) {
            console.log('&gt; 出错了！' + error);
        } finally {
            this.disabled = false;
        }
    });

    // 点击切换按钮可以触发画中画模式，
    // 在有些浏览器中，右键也可以切换，甚至可以自动进入画中画模式
    // 因此，一些与状态相关处理需要在专门的监听事件API中执行
    video.addEventListener('enterpictureinpicture', function(event) {
        console.log('&gt; 视频已进入Picture-in-Picture模式');
        pipBtn.value = pipBtn.value.replace('进入', '退出');

        pipWindow = event.pictureInPictureWindow;
        console.log('&gt; 视频窗体尺寸为：'+ pipWindow.width +' x ' + pipWindow.height);
    
        // 添加resize事件，一切都是为了测试API
        pipWindow.addEventListener('resize', onPipWindowResize);
    });
        // 退出画中画模式时候
    video.addEventListener('leavepictureinpicture', function(event) {
        console.log('&gt; 视频已退出Picture-in-Picture模式');
        pipBtn.value = pipBtn.value.replace('退出', '进入');
        // 移除resize事件
        pipWindow.removeEventListener('resize', onPipWindowResize);
    });
    // 视频窗口尺寸改变时候执行的事件
    var onPipWindowResize = function (event) {
        console.log('&gt; 窗口尺寸改变为：'+ pipWindow.width +' x ' + pipWindow.height);
    }
    /* 特征检测 */
    if ('pictureInPictureEnabled' in document == false) {
        console.log('当前浏览器不支持视频画中画。');
        togglePipButton.disabled = true;
    }
</script>

```

# 十八、取的表单元素中的内容

## 1、取的下拉框中的值

```java
import React, { Component } from 'react';
export default class nav extends Component {
 fun=(e)=>{
  console.log(e.target.value);
 }
    render() {
        return (
            <div>
             <select name="" id="" onChange={this.fun}>
                     <option value="一">一</option>
                     <option value="二">二</option>
                     <option value="三">三</option>
                 </select>
            </div>
        )
    }
}
```

## 2、用事件源取得输入框内的值，每次打印是空白 的解决办法（这个是异步的问题）

```javascript
import React, { Component } from 'react';
export default class nav extends Component {
     constructor(props){
        super(props);
        this.state={
            text:''
        }
    }
 fun=(e)=>{
  console.log(e.target.value);
  this.setState({
   text:e.targetvalue
  },function(){
   console.log(this.state.text);
  })
 }
    render() {
        return (
            <div>
             <input type="text" onChange={this.fun} />
            </div>
        )
    }
}
```

## 3、勾选框，判断是否勾选

```java
import React from 'react';
class Text extends React.Component{
    this.Checklist=React.createRef();
    constructor(props){
        super(props);
    }
    agree=()=>{
        // 先获取这个按钮
        // console.log(this.Checklist.current);
        // 勾选之后是true
        if(this.Checklist.current.checked===true){
            console.log('已勾选')
        }else{
            console.log('请勾选协议')
        }
    }
    render(){
     //样式的一种写法
        let obj={
            color:'red'
        }
        
        return <div style={obj}>
            <input type="checkbox" value=" " onClick={this.agree} ref={this.Checklist} />
            我已阅读并同意相关服务条款和隐私政策
        </div>
    }
}
export default Text;

```

# 十九、Fetch数据请求，包括解决跨域

## 1、fetch数据请求

```jsx
/**
 * https://developer.mozilla.org/zh-CN/docs/Web/API/Fetch_API/Using_Fetch
 * http://nodejs.cn/api/querystring.html
 */
import React from "react";
// 这个是node里面的
import qs from "querystring";
export default class App extends React.Component{
    constructor(props) {
        super(props);
        this.state = {
            list:[]
        }
    }
    componentDidMount() {
        // get请求
        fetch("http://iwenwiki.com/api/blueberrypai/getIndexListening.php").then(res => {
             return res.json();
        }).then(data => {
            // console.log(data.listening);
            this.setState({
                list: data.listening
            })
        }).catch(err => {
            console.log(err);
        })
        // post请求
        fetch("http://iwenwiki.com/api/blueberrypai/login.php",{
            method:"post",
            headers:{
                'Content-Type': 'application/x-www-form-urlencoded',
                "Accept":"application/json,text/plain,*/*"
            },
            /*
            // 下面这样传递是错误的
            body:{
                user_id:"iwen@qq.com",
                password:"iwen123",
                verification_code:"crfvw"
            }
            */
            // body:"user_id=iwen@qq.com&password=iwen123&verification_code=crfvw"
            body: qs.stringify({
                user_id: "iwen@qq.com",
                password:"iwen123",
                verification_code:"crfvw"
            })
        }).then(res => res.json())
            .then(data => {
                console.log(data)
            }).catch(err => {
            console.log(err);
        })
    }
    
    render (){
        const {list} = this.state;
        return (
            <div>
                {/* 下面使用了三木运算符 */}
                {
                    list.length > 0 ?
                        list.map((item,index) => {
                            return <p key={index}> {item.title} </p>
                        }) :
                        <div>等待</div>
                }
            </div>
        );
    }
}
```

![image-20210621152853783](https://i.loli.net/2021/06/21/7lBhVjs1OXNIn8H.png)

## 2、跨域

### 1）在 `package.json`里面修改

在最后加入：

```json
"proxy": "http://localhost:3000"
```

这下在请求的时候，就不用在写<http://localhost:3000，直接写“http://localhost:3000”后面的；同时修改了配置文件，要重新运行。>

### 2）在src下创建 *setupProxy.js*

```javascript
/*
下载：
  npm install http-proxy-middleware --save
*/
const proxy = require('http-proxy-middleware');

module.exports = function (app) {
    app.use(
        proxy.createProxyMiddleware('/api', { //  /api 是需要转发的请求,在外面就使用他
            target: 'http://localhost:3100',  //  这里是接口服务器地址
            changeOrigin: true,
            pathRewrite: { '^/api': '' }
        })
    )
}
```

使用：

```javascript
import React from "react";
export default class App extends React.Component{
    constructor(props) {
        super(props);
        this.state = {
            
        }
    }
    componentDidMount() {
        fetch("/api/list").then(data => data.json()).then(res =>{
            console.log(res)
        }).catch(req => {
            console.log(req);
        })
    }
    
    render (){
        return (
            <div>
                解决跨域：https://github.com/facebook/create-react-app/blob/master/docusaurus/docs/proxying-api-requests-in-development.md#configuring-the-proxy-manually
            </div>
        );
    }
}
```
