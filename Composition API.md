[vue3文档](https://v3.cn.vuejs.org/)：<https://v3.cn.vuejs.org/>

Composition API字面意思是组合API，它是为了实现基于函数的逻辑复用机制而产生的。主要思想是，我们将它们定义为从新的 setup 函数返回的JavaScript变量，而不是将组件的功能（例如state、methods、computed等）定义为对象属性。

vue3中的基本模板：

```javascript
<!-- vue2中的html模板中必须要有一对跟标签，vue3组件模板中可以没有 -->
<template>

</template>

<script lang="ts">
// defineComponent函数，目的事定义一个组件，内部可以传入一个配置对象
import { defineComponent } from "vue";

export default defineComponent({
    name: "HelloWorld",
});
</script>

<style scoped>
</style>
```

# 一、Vue3的响应式的原理

通过 Reflect(反射): 动态对被代理对象的相应属性进行特定的操作。

文档:

<https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Proxy>

<https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Reflect>

```javascript
new Proxy(data, {
 // 拦截读取属性值
    get (target, prop) {
     return Reflect.get(target, prop)
    },
    // 拦截设置属性值或添加新属性
    set (target, prop, value) {
     return Reflect.set(target, prop, value)
    },
    // 拦截删除属性
    deleteProperty (target, prop) {
     return Reflect.deleteProperty(target, prop)
    }
})

proxy.name = 'tom'   
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Proxy 与 Reflect</title>
</head>
<body>
    <script>
        const user = {
            name: "John",
            age: 12
        };
        /* 
        proxyUser是代理对象, user是被代理对象
        后面所有的操作都是通过代理对象来操作被代理对象内部属性
        */
        const proxyUser = new Proxy(user, {

            get(target, prop) {
                console.log('劫持get()', prop)
                //反射对象
                return Reflect.get(target, prop)
            },

            set(target, prop, val) {
                console.log('劫持set()', prop, val)
                return Reflect.set(target, prop, val); // (2)
            },

            deleteProperty(target, prop) {
                console.log('劫持delete属性', prop)
                return Reflect.deleteProperty(target, prop)
            }
        });
        // 读取属性值
        console.log(proxyUser === user)
        console.log(proxyUser.name, proxyUser.age)
        // 设置属性值（通过代理对象修改目标对象上的某个值）
        proxyUser.name = 'bob'
        proxyUser.age = 13
        console.log(user)
        // 添加属性
        proxyUser.sex = '男'
        console.log(user)
        // 删除属性
        delete proxyUser.sex
        console.log(user)
    </script>
</body>
</html>
```

# 二、setup的使用

<font color=red>注：</font>

①setup()函数是vue3中专门新增的方法，可以理解为Composition Api的入口；

②setup在组件创建之前调用的，也就是在beforeCreate、created之前创建，因此没有this；

③setup 函数在创建组件之前被调用，所以在 setup 被执行时，组件实例并没有被创建；因此在 setup 函数中，我们将 没有办法 获取到 this ;

④setup是一个函数，只在初始化时执行一次；

⑤函数如果返回对象, 对象中的属性或方法, html模板中可以直接使用;

⑥一般不要混合使用: methods中可以访问setup提供的属性和方法, 但在setup方法中不能访问data和methods；

⑦setup不能是一个async函数: 因为返回值不再是return的对象, 而是promise, 模板看不到return对象中的属性数据；

⑧一般不要混合使用: methods中可以访问setup提供的属性和方法, 但在setup方法中不能访问data和methods；

⑨setup不能是一个async函数: 因为返回值不再是return的对象, 而是promise, 模板看不到return对象中的属性数据。

```javascript
<template>
    <div>
        {{data.count}} <br />
        {{data.double}} <br />
        <button @click="fun">增加</button>
    </div>
</template>
<script>
/**
 * 如果 定义对象，就要引入reactive，使用计算属性之前 也要引入
 */
import { computed, reactive } from "vue"
export default {
    setup() {
        const data = reactive({
            count: 1,
            // 下面这块如果是一行代码 就可以不return，加花括号 就要return
            double: computed(() => data.count * 2)
        });
        function fun() {
            data.count++
        };
        // 不管是方法，还是变量 只要在模板里面使用就要return返回（data 是属性，可以写为：data:data 简写是下面的）
        return { data, fun }
    }
}
</script>
```

## 1、正向传值

1）父组件中

```javascript
<template>
    <div>
        <SubComp num="22" />
    </div>
</template>
<script>

import SubComp from "子组件路径";
export default {
    components:{
        SubComp
    },
}
</script>
```

2）子组件中

```javascript
<template>
    <div>
        子组件
    </div>
</template>
<script>
export default {
    props:{
        num:{
            type:String
        }
    },
    // 如果要接受父组件里面传过来的,就要在setup里面书写参数，第一个属性，就是这个参数
    setup(props){
        console.log("我是setup");
        console.log(props.num);//22
    },
    
}
</script>
```

## 2、setup的参数

以子父组件为例。

1）父组件

```javascript
<template>
    <div>
        <SubComp num="22" text="你好">
            <p>插槽</p>
        </SubComp>
    </div>
</template>
<script>
import SubComp from "子组件路径";
export default {
    components:{
        SubComp
    },
}
</script>
```

2）子组件

```javascript
<template>
    <div>
        子组件
        <slot></slot>
    </div>
</template>
<script>
export default {
    props: {
        num: {
            type: String
        }
    },
    // 如果要接受父组件里面传过来的,就要在setup里面书写参数，第一个属性，就是这个参数
    /**
     * 第一个参数指的是穿过来的属性
     * 第二个参数是指上下文(可以接受 传值时，在字组件里面为定义的属性)
     * 上下文包括的属性：
       context.attrs
       context.slots
       context.parent
       context.root
       context.emit
       context.refs
     */
    /*
    setup(props, context) {//这块也可以用结构赋值 把context换成结构赋值
        console.log("我是setup");
        console.log(props.num);
        // 用attrs 也可以接收
        console.log(context.attrs.text); //你好
        //获取夫组件 插槽传递过来的数据
        console.log(context.slots.default());
    },
    */
    setup(props, { attrs, slots, emit }) {
        console.log("我是setup");
        console.log(props.num);
        // 用attrs 也可以接收
        console.log(attrs.text); //你好
        //获取夫组件 插槽传递过来的数据
        console.log(slots.default());
    },     
}
</script>
```

## 3、setup参数的具体使用见下方

### 1) props参数

props参数是一个对象，里面有父组件向子组件传递的数据，并且是在子组件中使用props接收到的所有属性（包含props配置声明且传入了的所有属性的对象）。

①父组件

```javascript
<template>
    <h2>父组件</h2>
    {{msg}}
    <hr />
    <Child :text="msg" />
</template>
<script lang="ts">
import { defineComponent, ref } from "vue";
import Child from "组件路径/文件名.vue";
export default defineComponent({
    name: "Home",
    components: {
        Child
    },
    setup() {
        // 定义一个ref类型的数据
        const msg = ref("我是干嘛啦");

        return { msg };
    },
});
</script>
```

②子组件

```javascript
<template>
    <h2>子组件</h2>
</template>
<script>
import { defineComponent } from "vue";
export default defineComponent({
    props:["text"],
    setup(props){
        console.log(props);
        console.log(props.text);
    }
})
</script>
```

### 2）context参数

注：使用context参数的时候，也要写上props参数，负责会报错。

context参数是一个对象：里面有attrs对象（获取当前组件标签上的所有的属性的对象，但是该属性是在props中没有声明接受的搜友的尚需经的对象）、emit方法（分发事件的）、slots对象（插槽）；

```javascript
<template>
    <h2>子组件</h2>
</template>
<script>
import { defineComponent } from "vue";
export default defineComponent({
    /*
    // 一般是这样(那么要用什么，context后面就要点什么)
    setup(props,context){
        // console.log();
        console.log(context.attrs);
        console.log(context.attrs.text);
    }
    */
    // 使用结构赋值 进项简写
    setup(props, { attrs, slots, emit }) {
    }
})
</script>
```

①父组件

```javascript
<template>
    <h2>父组件</h2>
    {{msg}}
    <hr />
    <HelloWorld :text="msg" @fun="fun" />
</template>
<script lang="ts">
import { defineComponent, ref } from "vue";
import HelloWorld from "@/components/HelloWorld.vue";
export default defineComponent({
    name: "Home",
    components: {
        HelloWorld,
    },
    setup() {
        // 定义一个ref类型的数据
        const msg = ref("我是干嘛啦");
        const fun = (val:string) => {
            console.log(val);
            msg.value += val;
        }
        return { msg, fun };
    },
});
</script>
```

②子组件

```javascript
<template>
    <h2>子组件</h2>
    <button @click="fun">更新数据</button>
</template>
<script>
import { defineComponent } from "vue";
export default defineComponent({
    /*
    // 一般是这样
    setup(props,context){
    }
    */
    // 使用结构赋值 进项简写
    setup(props, { attrs, slots, emit }) {
        console.log(attrs);
        console.log(attrs.text);
        // 直接传过去
        emit("fun", "哈哈哈");
        //点击之后 传过去的
        function fun() {
            emit("fun", "点击事件传的");
        }
        return { fun };
    }
})
</script>
```

## 4、逆向传值

1）子组件

```javascript
<template>
    <div>
        子组件
    </div>
</template>
<script>
export default {
    /*
    setup(props, context) {//这块也可以用结构赋值 把context换成结构赋值
        context.emit("myEmit","我是逆向传值");
    },
    */
    setup(props, { emit }) {
        emit("myEmit", "我是逆向传值");
    },
}
</script>
```

2)父组件

```javascript
<template>
    <div>
        <SubComp @myEmit="myEmit"></SubComp>
    </div>
</template>
<script>
import SubComp from "子组件路径";
export default {
    components:{
        SubComp
    },
    setup() {
        function myEmit(val){
            console.log(val);//我是逆向传值
        }
        // 不管是方法，还是变量 只要在模板里面使用就要return返回
        return { myEmit }
    }
}
</script>
```

# 二、Composition常用API

<font color=red>注：会把所有api都放在 *setup* 中。</font>

## 1、ref响应

ref()函数用来给定的值创建一个响应式的数据对象,ref()的返回值是一个对象,这个对象上只包含一个.value属性。

<font color=red> ref是一个函数：</font>

①作用：定义一个响应式的数据，返回的是一个Ref对象，对象中有一个value属性，如果需要对数据进行操作，需要使用该Ref对象调用value属性的方式进行数据的操作；

②一般用来定义一个基本类型的响应式数据；

③模板中操作数据:，不需要.value；

④如果用ref对象/数组, 内部会自动将对象/数组转换为reactive的代理对象。

```javascript
<template>
    <div>
        常用api 
        <hr />
        {{num}}
        <br />
        {{fun()}}
    </div>
</template>
<script>
export default {
    setup() {
        let num = 0;
        let fun = () => {
            num ++;
            console.log(num);
        }
        return { num, fun }
    }
}
</script>
```

调用方式如下：

```javascript
<template>
    <div>
        {{num}}
        <input type="text" v-model="num">
        <!-- 不注销他，上面就没办法输入 -->
        <!-- {{fun(2)}} -->
    </div>
</template>
<script>
// 需要引入
import { ref } from "vue";
export default {
    setup() {
        let num = ref(0);
        let fun = (val) => {
            num.value = val;
        }

        return { num, fun }
    }
}
</script>
```

## 2、reactive是用来创建一个响应式对象

将ref响应式数据挂载到reactive中，当把ref()创建出来值直接挂载到reactive()中时,会自动把响应式数据对象的展开为原始的值,不需要通过.value就可以直接访问到：

①作用: 定义多个数据的响应式

②const proxy = reactive(obj): 接收一个普通对象然后返回该普通对象的响应式代理器对象，返回的是一个proxy 的代理对象，被代理的目标对象就是obj对象

③响应式转换是“深层的”：会影响对象内部所有嵌套的属性

④内部基于 ES6 的 Proxy 实现，通过代理对象操作源对象内部数据都是响应式的

```javascript
<template>
    <div>
        <hr />
        {{num}}
        <input type="text" v-model="num">
        <br />
        <!-- {{fun(2)}} -->
        <hr />
        <!-- {{user.name}} -->
        {{name}}
        {{num2}}
    </div>
</template>
<script>
// 需要引入
import { reactive, ref, toRefs, readonly, isRef } from "vue";
export default {
    setup() {
        let num = ref(0);
        let fun = (val) => {
            num.value = val;
        }

        //isRef() 判断是否是响应式的

        // 用 reactive 声明 也可以是响应式的
        let user = reactive({
            name: "啊啊啊",
            age: 18,
            num2: num, //把ref声明的放在这个 里面，就不用value值了
        })
        function fun2() {
            user.num2 = 100
        }
        fun2();
        // 通过readonly()就可以吧响应式数据变成 不响应的

        //return { num, fun, user }//这样导出一个对象，取值的时候，需要点，那么就可以使用json对象 或者 ...(扩展运算符),只不过 这样展开，数据就不是相应式的了
        //return { num, fun, ...user } //这样就不能响应式了（...）
        return { num, fun, ...toRefs(user) } //引入了这个api，使用了扩展运算符之后，他就又可以响应了
    }
}
</script>
```

使用的时候，建议如下：

```javascript
<!-- vue2中的html模板中必须要有一对跟标签，vue3组件模板中可以没有 -->
<template>
    <button @click="updateUser">更新</button>
    <br />
    {{user}}
</template>
<script lang="ts">
// defineComponent函数，目的事定义一个组件，内部可以传入一个配置对象
import { defineComponent, reactive } from "vue";
export default defineComponent({
    name: "Home",
    setup() {
        //建议写多个对象,这样 下面好调用
        const user = reactive({
            car: {
                name: "小王",
                age: 18,
            },
            wife: {
                name: "小明",
                age: 22,
            },
        });
        // function updateUser(){}
        const updateUser = () => {
            user.car.name = "哈哈哈";
            user.wife.age++
            //删除user里面的实行
            delete user.wife.name
        }
        return { user, updateUser };
    }
});
</script>
<style scoped>
</style>
```

<font color=green>注</font>：直接使用目标对象的方式来更新目标对象中的成员的值，是不可能的，只能使用代理对象的方式来更新数据（响应式数据）。

## 3、computed（计算属性）

computed()用来创建计算属性,返回值是一个ref的实例。

```javascript
<template>
    计算属性:
    name: <input type="text" v-model="name"> <br/>
    age: <input type="number" v-model="age"> <br />
    sex: <input type="text" v-model="sex"> <br />
    {{name}}————{{age}}————{{sex}} <br />
    {{fullName}}
</template>
<script>
import { reactive, toRefs, computed } from 'vue';
export default {
    setup() {
        const user = reactive({
            name: "啊啊",
            age: 100,
            sex: "女"
        })
        // 计算属性(多个 的时候，同下)
        let fullName = computed(() => {
            return "名字：" + user.name + "—— 年龄：" + user.age
        })
        return { ...toRefs(user),fullName }
    }
}
</script>
```

## 4、watch()函数

watch()函数用来监视某些数据项的变化，从而触发某些特定的操作。并且他还有一个停止监听的方法。

1）watch

①与watch配置功能一致

②监视指定的一个或多个响应式数据, 一旦数据变化, 就自动执行监视回调

③默认初始时不执行回调, 但可以通过配置immediate为true, 来指定初始时立即执行第一次

④通过配置deep为true, 来指定深度监视

2）watchEffect函数

①不用直接指定要监视的数据, 回调函数中使用的哪些响应式数据就监视哪些响应式数据

②默认初始时就会执行第一次, 从而可以收集需要监视的数据

③监视数据发生变化时回调

1）ref单个变量

```javascript
<template>
    监听器
    <button @click="fun1">a ———— {{a}}</button>
    <button @click="b++">b ———— {{b}}</button>
</template>
<script>
import { ref, watch, watchEffect } from 'vue'
export default {
    setup() {
        let a = ref(1);
        let b = ref(2);

        function fun1() {
            a.value = a.value + 1;
        }
        watch(() => {
            // 这里面写那个 就监听那个的变化
            console.log(a.value, b.value);
        })
        watchEffect(() => {
            console.log(a.value, b.value);
        })
        // 只监听一个的变化(具体监听 某一个)，还可以确定新值和旧值，并且可以监听多个值，那么就要写成数组watch([a, b], ([newsA, newsB,], [oldsA, oldsB]) => {
        watch(a, (news, olds) => {
            console.log("监听a的变化", a.value, "新值", news, "旧值", olds);
        }, { immediate: true }) //immediate:true 加他可以默认执行一次
        return { a, b, fun1 }
    }
}
</script>
```

2）监听 reactive() 对象的变化

```javascript
<template>
    监听器
    <button @click="a++">a ———— {{a}}</button>
    <button @click="b++">b ———— {{b}}</button>
</template>
<script>
import { reactive, ref, toRefs, watch, watchEffect } from 'vue'
export default {
    setup() {
        const user = reactive({
            a: 1,
            b: 2
        })
        // 监听对象里面某个值的变化(监听多个的时候，使用数组)
        watch(() => user.a, (news, olds) => {
            console.log(news, olds);
        })
        /*
        // 当监听某个值的时候，，就不能监听整个对象，负责会报错
        watchEffect(() => {
            console.log("我是watchEffect，默认会打印一次", a);
        })
        watch(user, () => {
            console.log("监听整个对象，任何一个变化，他都能监听到");
        }, { immediate: true })
        //这个 可以监听变化，但是 不可以具体到那个变化了
        watch(user, (news, olds) => {
            console.log(news, olds);
        })
        */
        return { ...toRefs(user) }
    }
}
</script>
```

注：watchEffect 立即执行传入的一个函数，并响应式追踪其依赖，并在其依赖变更时重新运行该函数。

## 5、toRefs的使用

使用return导出一个对象的时候，那么每个对象都是一个ref函数，但是 导出对象的时候，使用的时候就需要点，很不方便，如果当时使用扩展运算符的时候，他又不会响应数据，所以这时候，就要用到toRefs函数。

问题: reactive 对象取出的所有属性值都是非响应式的。

解决: 利用 toRefs 可以将一个响应式 reactive 对象的所有原始属性转换为响应式的 ref 属性

<font color=red>toRefs可以吧一个响应式对象转换成普通对象，该普通对象的每个property 都是一个ref</font>

```javascript
<template>
    <p>{{name}}</p>
    <p>{{age}}</p>
    <button @click="fun">修改</button>
</template>
<script lang="ts">
import { defineComponent, reactive, toRefs } from "vue";
export default defineComponent({
    setup() {
        const obj = reactive({
            name: "小明",
            age: 88,
        });
        function fun() {
            obj.name = "我改名叫小王";
            console.log(obj);
        }
        // 下面这样写，数据就不实时响应了
        // return { ...obj, fun };
        // toRefs可以吧一个响应式对象转换成普通对象，该普通对象的每个property 都是一个ref
        return { ...toRefs(obj), fun };
    },
});
</script>
```

## 6、ref获取元素

让输入框自动获取焦点：

```javascript
<template>
    <h2>ref 获取页面中的元素</h2>
    <input type="text" ref="inputRef" />
</template>
<script lang="ts">
import { defineComponent, ref, onMounted } from "vue";
export default defineComponent({
    setup() {
        // 需求：当页面加载完毕后，页面中的文本框可以直接 获取焦点（自动获取焦点）
        const inputRef = ref<HTMLElement | null>(null);
        onMounted(() => {
            inputRef.value && inputRef.value.focus();
        });
        return { inputRef };
    },
});
</script>
```

## 7、 shallowReactive 与 shallowRef

1）shallowReactive : 只处理了对象内最外层属性的响应式(也就是浅响应式)

2）shallowRef: 只处理了value的响应式, 不进行对象的reactive处理

3）什么时候用浅响应式呢?

①一般情况下使用ref和reactive即可

②如果有一个对象数据, 结构比较深, 但变化时只是外层属性变化 ===> shallowReactive

③如果有一个对象数据, 后面会产生新的对象来替换 ===> shallowRef

4）劫持的深度

①reactive(深度劫持)

```javascript
<template>
    <p>reactive：{{m1}}</p>
    <button @click="fun">更改数据</button>
</template>
<script lang="ts">
import { defineComponent, reactive } from "vue";
export default defineComponent({
    setup() {
        const m1 = reactive({
            name: "小明",
            age: 22,
            car: {
                name: "自行车",
                color: "红色",
            },
        });
        // 更该数据
        const fun = () => {
            // 更改reactive的方式(深度劫持)
            m1.name += "啊啊啊";
            m1.car.color = "蓝色";
        }
        return { m1,fun };
    },
});
</script>
```

②shallowReactive（浅劫持）

```javascript
<template>
    <p>shallowReactive：{{m2}}</p>
    <button @click="fun">更改数据</button>
</template>
<script lang="ts">
import { defineComponent, shallowReactive } from "vue";
export default defineComponent({
    setup() {
        const m2 = shallowReactive({
            name: "小明",
            age: 22,
            car: {
                name: "自行车",
                color: "红色",
            },
        });
        // 更该数据
        const fun = () => {
            // shallowReactive 的方式
            // m2.name += "改变";
            m2.car.color += "橘色";
        }
        return { m2,fun };
    },
});
</script>
```

③ref（深度劫持）

```javascript
<template>
    <p>ref：{{m3}}</p>
    <button @click="fun">更改数据</button>
</template>
<script lang="ts">
import { defineComponent, ref } from "vue";
export default defineComponent({
    setup() {
        const m3 = ref({
            name: "小明",
            age: 22,
            car: {
                name: "自行车",
                color: "红色",
            },
        });
        // 更该数据
        const fun = () => {
            m3.value.car.name += "改变"; 
        }
        return { m3, fun };
    },
});
</script>
```

④shallowRef（浅劫持）

```javascript
<template>
    <p>shallowRef：{{m4}}</p>
    <button @click="fun">更改数据</button>
</template>
<script lang="ts">
import { defineComponent, shallowRef } from "vue";
export default defineComponent({
    setup() {
        const m4 = shallowRef({
            name: "小明",
            age: 22,
            car: {
                name: "自行车",
                color: "红色",
            },
        });
        // 更该数据
        const fun = () => {
            m4.value.name += "改变";
            m4.value.car.name += "改变";
            console.log(m4)
        }
        return { m4 ,fun };
    },
});
</script>
```

## 8、readonly 与 shallowReadonly

在某些特定情况下, 我们可能不希望对数据进行更新的操作, 那就可以包装生成一个只读代理对象来读取数据, 而不能修改或删除 。

1）readonly是一个深度只读的

```javascript
<template>
    <h3>{{state}}</h3>
    <button @click="update">更新</button>
</template>

<script lang="ts">
import { reactive, readonly, shallowReadonly } from "vue";
/*
readonly: 深度只读数据
  获取一个对象 (响应式或纯对象) 或 ref 并返回原始代理的只读代理。
  只读代理是深层的：访问的任何嵌套 property 也是只读的。
*/
export default {
    setup() {
        const state = reactive({
            a: 1,
            b: {
                c: 2,
            },
        });
        const rState2 = readonly(state);
        // 修改时，直接报错
        const update = () => {
            rState2.b.c++;
        };

        return {
            state,
            update,
        };
    },
};
</script>
```

2）shallowReadonly  浅只读

```javascript
<template>
    <h3>{{state}}</h3>
    <button @click="update">更新</button>
</template>

<script lang="ts">
import { reactive, readonly, shallowReadonly } from "vue";
/*
shallowReadonly: 浅只读数据
 创建一个代理，使其自身的 property 为只读，但不执行嵌套对象的深度只读转换 
*/
export default {
    setup() {
        const state = reactive({
            a: 1,
            b: {
                c: 2,
            },
        });

        const rState2 = shallowReadonly(state);
        // 修改时，直接报错
        const update = () => {
            rState2.a++;
            // 但是 深层次的数据，还是可以修改的
            rState2.b.c++;
        };

        return {
            state,
            update,
        };
    },
};
</script>
```

## 9、toRaw 与 markRaw

1）toRaw

返回由 `reactive` 或 `readonly` 方法转换成响应式代理的普通对象。

这是一个还原方法，可用于临时读取，访问不会被代理/跟踪，写入时也不会触发界面更新。

```javascript
<template>
    <h2>{{state}}</h2>
    <button @click="fun">测试</button>
</template>

<script lang="ts">
import { markRaw, reactive, toRaw } from "vue";
export default {
    setup() {
        const state = reactive<any>({
            name: "tom",
            age: 25,
        });

        const fun = () => {
            // 把代理对象变成了普通对象，但是界面 依然没有更新
            const user = toRaw(state);
            user.age++; // 界面不会更新
        };
        return {
            state,
            fun
        };
    },
};
</script>
```

2）markRaw

标记一个对象，使其永远不会转换为代理。返回对象本身

```javascript
<template>
    <h2>{{state}}</h2>
    <button @click="fun">测试</button>
</template>

<script lang="ts">
import { markRaw, reactive, toRaw } from "vue";
interface Info{
    name:string;
    age:number;
    likes ?: string[];
}
export default {
    setup() {
        const state = reactive<Info>({
            name: "tom",
            age: 25,
        });

        const fun = () => {
            const likes = ["a", "b"];
            // 使用markRaw标记的对象数据，从此以后 都不能再成为 代理对象了
            state.likes = markRaw(likes);//这样在界面上  就不会更改了
            setInterval(() => {
                if(state.likes){
                    state.likes[0] += "--";
                    console.log("定时器走起来");
                }
            },1000)
        };
        return {
            state,
            fun,
        };
    },
};
</script>
```

应用场景:

①有些值不应被设置为响应式的，例如复杂的第三方类实例或 Vue 组件对象。

②当渲染具有不可变数据源的大列表时，跳过代理转换可以提高性能。

## 10、toRef的使用

为源响应式对象上的某个属性创建一个 ref对象, 二者内部操作的是同一个数据值, 更新时二者是同步的；区别ref: 拷贝了一份新的数据值单独操作, 更新时相互不影响。

1）父组件

```javascript
<template>
    <h2>state：{{state}}</h2>
    <h2>age：{{age}}</h2>
    <h2>money：{{money}}</h2>
    <button @click="fun">更新数据</button>
    <hr >
    <!-- 到组件里面取数据 可以省略value -->
    <api-child :age="age" />
</template>
<script lang="ts">
import { defineComponent, reactive, toRef, ref } from "vue";
import apiChild from "./component/apiChild.vue";
export default defineComponent({
    components:{
        apiChild
    },
    setup() {
        const state = reactive({
            age: 5,
            money: 100,
        });
        // 把响应式数据state对象中的某个属性age变成了ref对象了
        const age = toRef(state, "age");
        // 把响应式对象中的某个属性使用ref进行包装,变成了一个ref对象(所以他俩 不会绑定，改变下面的，state里面的值 不会改变)
        const money = ref(state.money);
        console.log(age);//ref对象
        console.log(money);//ref对象
        //跟新数据
        function fun() {
            state.age += 2;
            // age.value = 3;
        }
        return { state, age, money, fun };
    },
});
</script>
```

2）子组件

```javascript
<template>
    <h3>子组件</h3>
    <p>age：{{age}}</p>
    <p>长度：{{length}}</p>
</template>
<script lang="ts">
import { defineComponent, computed, Ref, toRef } from "vue";
// 下面是一个hook函数
function useGetLength(age: Ref) {
    return computed(() => {
        return age.value.toString().length;
    });
}
export default defineComponent({
    props: {
        age: {
            type: Number,
            required: true, // 必须的(就是必须传这个)
        },
    },
    setup(props) {
        // 在子组件里获取传过来的长度
        const length = useGetLength(toRef(props, "age"));
        return { length };
    },
});
</script>
```

## 11、customRef

创建一个自定义的 ref，并对其依赖项跟踪和更新触发进行显式控制

```javascript
<template>
    <input type="text" v-model="text" />
    <p>{{ text }}</p>
</template>
<script lang="ts">
import { customRef, defineComponent, ref } from "vue";
// 自定义hook防抖的函数
// value传入的数据,将来数据的类型不确定,所以,用泛型,delay防抖的间隔时间.默认是200毫秒
function useDebouncedRef<T>(value: T, delay = 200) {
    // 准备一个存储定时器的id的变量
    let time: any;
    // 它返回的是ref(track, trigger 在官方文档里面)
    return customRef((track, trigger) => {
        return {
            // 返回数据的
            get() {
                // 告诉Vue追踪数据
                track();
                return value;
            },
            // 设置数据的
            set(newValue: T) {
                // 清理定时器
                clearTimeout(time);
                // 开启定时器
                time = setTimeout(() => {
                    value = newValue;
                    // 告诉Vue更新界面
                    trigger();
                }, delay);
            },
        };
    });
}
export default defineComponent({
    setup() {
        const text = useDebouncedRef("abc", 500);
        return {
            text,
        };
    },
});
</script>
```

## 12、判断数据是不是响应式

1）isRef: 检查一个值是否为一个 ref 对象

2）isReactive: 检查一个对象是否是由 `reactive` 创建的响应式代理

3）isReadonly: 检查一个对象是否是由 `readonly` 创建的只读代理

4）isProxy: 检查一个对象是否是由 `reactive` 或者 `readonly` 方法创建的代理

注：上面返回的是true或者false。

# 三、生命周期钩子

• beforeMount -> onBeforeMount

• mounted -> onMounted

• beforeUpdate -> onBeforeUpdate

• updated -> onUpdated

• beforeUnmount -> onBeforeUnmount

• unmounted -> onUnmounted

<font color=red>注：</font>

①在setup中没有创建组件的钩子函数（beforeCreate、created），其余的都有；

②执行时机是相同的；

③但是写在setup中的时候，前面多加了一个on，里面还是一个回调方法；

④一定要引入;

⑤v-if显示隐藏会触发组件里面的显示隐藏。

```javascript
<template>
    生命周期

</template>
<script>
//用哪个生命周期，就要在这引入
import { onMounted } from 'vue'
export default {
    setup(){
        /**
         * 跟Mounted是同时调用的，
         * 但是写在setup中的时候，前面多加了一个on
         * 里面还是一个回调方法
         * 同时一定要引入
        */ 
        onMounted(()=>{
            console.log("onMounted");
        })
    }
}
</script>
```

# 四、自定义hooks函数的使用

- 使用Vue3的组合API封装的可复用的功能函数
- 自定义hook的作用类似于vue2中的mixin（混入）技术
- 自定义Hook的优势: 很清楚复用功能代码的来源, 更清楚易懂

## 1、案例

### 1）收集用户鼠标点击的页面坐标

①在vue页面中

```javascript
<template>
    <h2>自定义hooks函数</h2>
    <p>x:{{x}} ，y:{{y}}</p>
</template>
<script>
import { defineComponent } from 'vue';
//1、引入
import 文件名 from "文件的路径/文件名.ts";
export default defineComponent({
    // 需求1:用户在页面中点击页面,把点击的位置的横纵坐标收集起来并展示出来
    setup() {
        // 2、在这使用结构赋值，进行使用
        const { x, y } = 文件名();
        return { x, y }
    },
})
</script>
```

②创建引入的文件（这块可以是ts也可以是js）

```typescript
import { onBeforeMount, onMounted, ref } from 'vue';
export default function () {
    const x = ref(-1);
    const y = ref(-1);
    /**
     * 点击的回调函数
     * @param event 他是鼠标的点击事件，所以他有一个MouseEvent类型
     */
    function clickHandler(event: MouseEvent) {
        x.value = event.pageX;
        y.value = event.pageY;
    }

    // 页面加载完毕，在进行操作，所以 就需要页面加载完毕的钩子
    onMounted(() => {
        //在这写了方法，需要在上面写回调函数
        window.addEventListener("click", clickHandler);
    })
    // 页面关闭，卸载函数
    onBeforeMount(() => {
        window.removeEventListener("click", clickHandler);
    })
    // 暴露出去，外面要用的
    return { x, y }
}
```

# 五、在组合API中provide和inject使用

从父向后代传递数据，父向后代传递的时候，不想层层传递就用它。

## 1、传统方式

1）根组件或者页面

```javascript
<template>
    Provide和Inject的使用
    <h1>跟</h1>
    {{title}}
    <input type="text" v-model="title">
    <TwoProvide />
</template>
<script>
import TwoProvide from "./component/TwoProvide.vue";
export default {
    components:{
        TwoProvide
    },
    data() {
        return {
            title:"这是跟组件或者页面提供的数据"
        }
    },
    // 1、注入(也就是 提供数据，但是 传统方式，无法响应数据，也就后代组件里面的值，不会改变)
    provide(){
        return {
            title:this.title
        }
    }
}
</script>
```

2）第二层组件

```javascript
<template>
    <hr />
    <p>第二层组件</p>
    <three-provide />
</template>
<script>
import ThreeProvide from "./ThreeProvide.vue";
export default {
    components:{
        ThreeProvide
    }
}
</script>
```

3）第三层组件

```javascript
<template>
    <hr />
    <p>第三层组件</p>
    {{title}}
</template>
<script>
export default {
    // 2、使用
    inject:["title"]
}
</script>
```

![image-20210215235628437](https://gitee.com/Green_chicken/picture/raw/master/%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/20210215235630.png)

## 2、setup中的使用

1）根组件或者页面

```java
<template>
    Provide和Inject的使用
    <h1>跟</h1>
    {{title}}
    <input type="text" v-model="title">
    <TwoProvide />
</template>
<script>
import { ref, provide } from "vue";
import TwoProvide from "./component/TwoProvide.vue";
export default {
    components: {
        TwoProvide
    },
    setup() {
        let title = ref("这是跟组件或者页面提供的数据");
        // 1、注入(也就是 提供数据，但是 传统方式，无法响应数据，也就后代组件里面的值，不会改变)
        provide("title", title);

        return { title };
    }
}
</script>
```

2)第二层组件

```javascript
<template>
    <hr />
    <p>第二层组件</p>
    <three-provide />
</template>
<script>
import ThreeProvide from "./ThreeProvide.vue";
export default {
    components:{
        ThreeProvide
    }
}
</script>
```

3)第三层组件

```javascript
<template>
    <hr />
    <p>第三层组件</p>
    {{title}}
    <input type="text" v-model="title">
</template>
<script>
import { inject } from 'vue'
export default {
    // 2、使用(接受)
    setup() {

        let title = inject("title");
        return { title }
    }
}
</script>
```

## 3、setup对象的方式如下

<font color=blue> 注：其他PI参考[地址](https://vue3js.cn/vue-composition-api)：https://vue3js.cn/vue-composition-api</font>

# 六、vue3中的新组件

## 1、Fragment(片断)

1）在Vue2中: 组件必须有一个根标签

2）在Vue3中: 组件可以没有根标签, 内部会将多个标签包含在一个Fragment虚拟元素中

3）好处: 减少标签层级, 减小内存占用

```vu
<template>
    <!-- 这个里面不需要跟标签（div） -->
</template>
```

## 2、Teleport(瞬移)

Teleport 提供了一种干净的方法, 让组件的html在父组件界面外的特定标签(很可能是body)下插入显示。

注：也就是可以通过，指定组件的显示位置。

1）父组件

```vu
<template>
    App
    <DialogBox />
    
</template>
<script lang="ts">
import { defineComponent } from 'vue'
import DialogBox from "组件路径/DialogBox.vue";
export default defineComponent({
    components:{
        DialogBox
    }
})
</script>
```

2）子组件

```javascript
<template>
    <button @click="fun">打开一个对话框</button>
    <!-- 下面这个 是对话框,使用teleport显示位置 -->
    <teleport to="body">
        <div v-if="modalOpen" class="modal">
            <div>
                内容
                <button @click="modalOpen = false">关闭</button>
            </div>
        </div>
    </teleport>
</template>
<script lang="ts">
import { defineComponent, ref } from "vue";
export default defineComponent({
    setup() {
        const modalOpen = ref(false);
        function fun() {
            modalOpen.value = true;
        }
        return { modalOpen, fun };
    },
});
</script>
<style scoped>
.modal {
    position: absolute;
    top: 0;
    right: 0;
    bottom: 0;
    left: 0;
    background-color: rgba(0, 0, 0, 0.5);
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
}

.modal div {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    background-color: white;
    width: 300px;
    height: 300px;
    padding: 5px;
}
</style>
```

## 3、Suspense(不确定的)

允许我们的应用程序在等待异步组件时渲染一些后备内容，可以让我们创建一个平滑的用户体验。

也就是在页面中要显示一个组件，但是这个组件要过一段时间进行显示，这个空白页就可以用它，可以在做骨架屏的时候使用。

1）父组件

```javascript
<template>
    <h1>父组件</h1>
    <suspense>
        <!-- 
            注： v-slot:fallback 是固定的写法，v-slot的简写是 #default
         -->
        <!-- 异步组件 -->
        <template #default>
            <AsyncComponent />
        </template>
        <!-- 在异步组件还没加载完成时，显示的内容 -->
        <template v-slot:fallback>
            <h3>Loading。。。</h3>
        </template>
    </suspense>
</template>
<script lang="ts">
import { defineComponent, defineAsyncComponent } from "vue";
// 组件的引入 分为动态引入和静态引入
// 1、vue2中的动态引入（可以理解为 异步的）,但是下面这种写法在vue3中不行
// const AsyncComponent = () => import("./component/AsyncComponent.vue");
// 2、vue3中的同台引入
const AsyncComponent = defineAsyncComponent(
    () => import("./component/AsyncComponent.vue")
);
export default defineComponent({
    components: {
        AsyncComponent,
    },
    setup() {},
});
</script>
```

2）子组件

```javascript
<template>
    <h2>异步组件</h2>
    {{msg}}
</template>
<script lang="ts">
import { defineComponent } from "vue";
export default defineComponent({
    setup() {
        // 在这模拟一个异步的操作(这里一般是数据请求),这块也可以使用async和await
        return new Promise((resolve, reject) => {
            setTimeout(() => {
                resolve({
                    msg: "你好！！！",
                });
            }, 2000);
        });
    },
    /**
    //async和await的数据请求如下
    async setup() {
        const request = await axios.get("路径");
        return {
            data:request
        }
    },
     */
});
</script>
```

# 六、结合路由

## 1、params路由传参

1）在路由文件里面定义规则（也就是确定传递的键名）

```typescript
//下面这个是不传参数的，这样可以解决错误提示(必须写在参数的前面)
{ name: "details", path: "details", component: () => import("路径") },
//下面这个 id就是键名
{ name: "details", path: "details/:id", component: () => import("路径") }
```

2）在跳转页面

```javascript
<template>
    路由api
    <div>
     <!-- 下面使用了二级路由 -->
        <div id="menu">
            <router-link v-for="item in list" :key="item.id" :to="'/router-api/details/' + item.id">{{item.title}}</router-link>
        </div>
        <div id="content">
            <router-view></router-view>
        </div>
    </div>
</template>
<script>
import { reactive } from 'vue'
export default {
    setup() {
        let list = reactive([
            { title: "列表一", id: 1 },
            { title: "列表二", id: 2 },
            { title: "列表三", id: 3 },
            { title: "列表四", id: 4 }
        ])
        return { list }
    }
}
</script>
<style scoped>
a{
    display: block;
    margin-top: 20px;
}
#menu {
    width: 30%;
    background: orchid;
    height: 300px;
    float: left;
}
#content {
    width: 70%;
    background: palegreen;
    height: 300px;
    float: right;
}
</style>
```

3)在接收页面

```javascript
<template>
    详情
    <p>{{$route.params.id}}</p>
    {{id}}
    <p>{{detailsId}}</p>
</template>
<script>
import { ref, watch } from 'vue';
import { useRoute, useRouter } from "vue-router";
export default {
    setup() {
        const route = useRoute();
        const router = useRouter();
        let detailsId = ref(route.params.id);
        watch(()=>route.params,(news)=>{
            detailsId.value = news.id;
        })

        return { detailsId }
    },
    computed: {
        // 原来的方法
        id() {
            return this.$route.params.id
        }
    }
}
</script>
```

## 2、query传参

1）跳转页面

```javascript
<router-link :to="{path:'/router-api/article',query:{name:'啊啊啊',age:22}}">跳转</router-link>
<button @click="$router.push({path:'/router-api/article',query:{name:'哈哈哈',age:222}})">跳转</button>
```

2）接收页面

```javascript
<template>
    文章
    <p>{{$route.query.name}}</p>

</template>
<script>
import { ref } from 'vue';
import { useRoute, useRouter } from "vue-router";
export default {
     setup() {
        const route = useRoute();
        const router = useRouter();
        //这里只执行一次，多个参数的时候，用监听
        console.log(route.query.name);
    },
}
</script>
```

注：导航守卫

onBeforeRouteLeave((to, from) => {｝）

onBeforeRouteUpdate(async (to, from) => {})
