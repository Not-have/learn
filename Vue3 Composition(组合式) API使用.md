# 一、setup中API的使用

注：ref、Reactive都是深层的响应式。

## 1、Reactive

  ```vue
  <script lang='ts' setup>
  import { reactive } from 'vue';
  const obj = reactive({ count: 0 });
  obj.count++;
  console.log(obj);
  
  </script>
  <template>
      <div>
          API 的基础使用
      </div>
  </template>
  ```

<font color=red>注：</font>Reactive API必须传入的是一个对象或者数组类型。

## 2、ref

1）ref 会返回一个可变的响应式对象，该对象作为一个 响应式的引用 维护着它内部的值，这就是ref名称的来源；

2）它内部的值是在ref的 value 属性中被维护的；

<font color=red>注：</font> ① 在模板中引入ref的值时，Vue会自动帮助我们进行解包操作，所以我们并不需要在模板中通过 ref.value 的方式 来使用；

​         ② 在 setup 函数内部，它依然是一个 ref引用， 所以对其进行操作时，我们依然需要使用 ref.value的方式；在 setup 函数内部，它依然是一个 ref引用， 所以对其进行操作时，我们依然需要使用 ref.value的方式；

 ```vue
 <script lang='ts' setup>
 import { ref } from 'vue';
 const count = ref(0);
 console.log(count.value); // 0
 
 count.value = 1;
 console.log(count.value); // 1
 
 </script>
 <template>
     <div>
         API 的基础使用
     </div>
 </template>
 ```

3）使用ref获取DOM元素

```vue
<script lang='ts' setup>
import { ref, onMounted } from 'vue';
const el = ref<Element | undefined>();


onMounted(() => {
    console.log(el.value);
})

</script>
<template>
    <div ref="el">
        API 的基础使用
    </div>
</template>
```

## 3、readonly

readonly会返回原生对象的只读代理（也就是它依然是一个Proxy，这是一个proxy的set方法被劫持，并且不 能对其进行修改）

注：用readonly包裹响应式数据，发送给子组件，让其不能在子组件中，随意改变，只能在组件中，去改变响应的数据，如果非要在子组件中改变传过去的数据，他就会报错。

 ```vue
 <script lang='ts' setup>
 import { reactive, readonly } from 'vue';
 const original = reactive({ count: 0 });
 
 const copy = readonly(original);
 
 // 更改源属性会触发其依赖的侦听器
 original.count++;
 
 // 更改该只读副本将会失败，并会得到一个警告
 copy.count++; // warning!
 
 console.log(copy.count);
 
 </script>
 <template>
     <div ref="el">
         API 的基础使用
     </div>
 </template>
 ```

![image-20240224191544976](./images/image-20240224191544976.png)

## 4、Reactive其他API的使用

具体的，请参考文档： <https://cn.vuejs.org/api/reactivity-utilities.html>

1）isProxy

检查一个对象是否是由 [`reactive()`](https://cn.vuejs.org/api/reactivity-core.html#reactive)、[`readonly()`](https://cn.vuejs.org/api/reactivity-core.html#readonly)、[`shallowReactive()`](https://cn.vuejs.org/api/reactivity-advanced.html#shallowreactive) 或 [`shallowReadonly()`](https://cn.vuejs.org/api/reactivity-advanced.html#shallowreadonly) 创建的代理，返回 boolean；

2）isReactive

检查一个对象是否是由 [`reactive()`](https://cn.vuejs.org/api/reactivity-core.html#reactive) 或 [`shallowReactive()`](https://cn.vuejs.org/api/reactivity-advanced.html#shallowreactive) 创建的代理，返回 boolean；

3）isReadonly

通过 [`readonly()`](https://cn.vuejs.org/api/reactivity-core.html#readonly) 和 [`shallowReadonly()`](https://cn.vuejs.org/api/reactivity-advanced.html#shallowreadonly) 创建的代理都是只读的，因为他们是没有 `set` 函数的 [`computed()`](https://cn.vuejs.org/api/reactivity-core.html#computed) ref。

4）toRaw

返回 reactive 或 readonly 代理的原始对象（不建议保留对原始对象的持久引用。请谨慎使用）；

5）shallowReactive

创建一个响应式代理，它跟踪其自身 property 的响应性，但不执行嵌套对象的深层响应式转换 (深层还是原生对象)，只做浅层的响应式代理；

6）shallowReadonly

创建一个 proxy，使其自身的 property 为只读，但不执行嵌套对象的深度只读转换（深层还是可读、可写的）；

## 5、toRefs（将reactive 将对象中的所有属性都转成ref，并且是一个reactive的响应式的对象，才能作为转换）

```javascript
<template>
    <p>toRef的使用</p>
    <button @click="fun">点击</button>
    <p>{{ info.name }} —————— {{ info.age }}</p>
</template>

<script lang='ts'>
import { defineComponent, reactive, toRefs } from "vue";

export default defineComponent({
    name: "index",
    components: {},
    setup() {
        let info = reactive({
            name: "李四",
            age: 18,
        });
        // 直接修改他俩 需要 .value
        const { age, name } = toRefs(info);
        const fun = () => {
            // info.age++;
            age.value++;
        };
        return { info, fun };
    },
});
</script>
```

## 6、toRef（对其中一个属性进行转换，转成ref属性）

```javascript
<template>
    <p>toRef的使用</p>
    <button @click="fun">点击</button>
    <p>{{ name }} —————— {{ age }}</p>
</template>

<script>
import { reactive, toRef } from "vue";

export default {
    name: "index",
    components: {},
    setup() {
        let info = reactive({
            name: "李四",
            age: 18,
        });
        let { name } = info;
        // 只转一个(这里不能在做一个结构了)
        let age = toRef(info, "age");
        const fun = () => {
            age.value++;
        };
        return { name, age, fun };
    },
};
</script>
```

## 7、ref的其他API

具体的，请参考文档：<https://v3.cn.vuejs.org/api/refs-api.html#unref>

### 1）unref（判断是否为ref）

如果我们想要获取一个ref引用中的value，那么也可以通过unref方法

如果参数是一个 ref，则返回内部值，否则返回参数本身；

这是 val = isRef(val) ? val.value : val 的语法糖函数。

### 2）isRef（判断值是否是一个ref对象）

判断值是否是一个ref对象

### 3）shallowRef（创建一个浅层的ref对象）

```javascript
<template>
    <p>{{info}}</p>
    <button @click="changeInfo">点击</button>
</template>

<script>
import { ref, shallowRef } from '@vue/reactivity'

export default {
    name: 'index',
    components: {

    },
    setup() {
        const info = shallowRef({ nmae: "哈哈哈" })
        const changeInfo = () => {
            info.value.nmae = "呵呵呵"
        }
        return { info, changeInfo }
    }
}
</script>
```

### 4）triggerRef

手动触发和 shallowRef 相关联的副作用

```javascript
<template>
    <p>{{info}}</p>
    <button @click="changeInfo">点击</button>
</template>

<script>
import { ref, shallowRef, triggerRef } from '@vue/reactivity'

export default {
    name: 'index',
    components: {

    },
    setup() {
        const info = shallowRef({ nmae: "哈哈哈" })
        const changeInfo = () => {
            info.value.nmae = "呵呵呵"
            triggerRef(info)
        }
        return { info, changeInfo }
    }
}
</script>
```

## 8、customRef

创建一个自定义的ref，并对其依赖项跟踪和更新触发进行显示控制

<https://cn.vuejs.org/api/reactivity-advanced.html#customref>

```javascript
/**
 * 实现一个防抖的ref
 * 下面这个一般写在hook里面
 */
import { customRef } from "vue"
/**
 * 自定义ref
 * customRef 里面两个参数
 * track 跟踪（决定什么时候 收集依赖）
 * trigger 决定什么时候触发所有的依赖,进行更新
 * 返回一个带有 get 和 set 的对象
 */
export default function (value, delay = 200) {
    let time = null
    return customRef((track, trigger) => {
        return {
            get() {
                // 收集依赖
                track()
                return value
            },
            // 设置新的值
            set(newValue) {
                // 处理数据
                clearTimeout(time)
                time = setTimeout(() => {
                    value = newValue
                    trigger()
                }, delay)
            }
        }
    })
}
```

## 9、computed 计算属性

方式一：接收一个getter函数，并为 getter 函数返回的值，返回一个不变的 ref 对象；
方式二：接收一个具有 get 和 set 的对象，返回一个可变的（可读写）ref 对象。

```javascript
<template>
    <p>{{and}}</p>
    <button @click="fun">修改</button>
</template>
<script>
import { computed, ref } from "vue"
export default {
    name: 'index',
    setup() {
        const str1 = ref("lalall111")
        const str2 = ref("hahaha")
        /**
         * 方法一：
         * 传入一个getter函数
         * computed 返回的是一个ref对象
         */
        // const and = computed(() => str1.value + str2.value)
        /**
         * 方法二：
         * 传入一个对象，包含 etter/setter
         * computed 返回的是一个ref对象
         */
        const and = computed({
            get: () => str1.value + str2.value,
            set(newValue) {
                str1.value = newValue
            }
        })
        const fun = function () {
            and.value = "哈哈哈"
        }
        return { and, fun }
    }
}
</script>
```

## 10、watchEffect

### 1）watchEffect 自动响应式依赖

注：会立即执行一次，然后改变变量，这里就能监听到

```javascript
<template>
    <div>
        <p>{{name}}</p>
        <p> {{age}}</p>

        <p><button @click="changeName">点击</button><button @click="changeNameAge">点击</button></p>
    </div>
</template>

<script lang='ts'>
import { defineComponent, ref, watchEffect } from "vue";
export default defineComponent({
    name: "index",
    components: {},
    setup() {
        const name = ref("小明");
        const age = ref(22);
        const changeName = () => (name.value = "小花");
        const changeNameAge = () => age.value++;
        /**
         * 里面是响应式对象
         */
        watchEffect(() => {
            console.log("name", name.value);
            console.log("age", age.value);
        });
        return { name, age, changeName, changeNameAge };
    },
});
</script>
```

### 2）watchEffect的停止监听的使用

```javascript
<template>
    <div>
        <p>{{name}}</p>
        <p> {{age}}</p>

        <p><button @click="changeName">点击</button><button @click="changeNameAge">点击</button></p>
    </div>
</template>

<script lang='ts'>
import { defineComponent, ref, watchEffect } from "vue";
export default defineComponent({
    name: "index",
    components: {},
    setup() {
        const name = ref("小明");
        const age = ref(22);
        const changeName = () => (name.value = "小花");
        const changeNameAge = () => {
            age.value++;
            if (age.value > 25) {
                stop();
            }
        };
        /**
         * 里面是响应式对象
         * 返回一个函数，在外面调用，他就会停止监听
         */
        const stop = watchEffect(() => {
            console.log("name", name.value, "age", age.value);
        });
        return { name, age, changeName, changeNameAge };
    },
});
</script>
```

### 3）watchEffect清除副作用

注：在网络请求中，当数据更新了之后，应该使用新数据去请求，停掉之前的那个的时候用。

```javascript
<template>
    <div>
        <h2>{{name}}-{{age}}</h2>
        <button @click="changeName">修改name</button>
        <button @click="changeAge">修改age</button>
    </div>
</template>

<script>
import { ref, watchEffect } from 'vue';

export default {
    setup() {
        // watchEffect: 自动收集响应式的依赖
        const name = ref("why");
        const age = ref(18);

        const stop = watchEffect((onInvalidate) => {
            const timer = setTimeout(() => {
                console.log("网络请求成功~");
            }, 2000)

            // 根据name和age两个变量发送网络请求
            onInvalidate(() => {
                // 在这个函数中清除额外的副作用
                // request.cancel()
                clearTimeout(timer);
                console.log("onInvalidate");
            })
            console.log("name:", name.value, "age:", age.value);
        });

        const changeName = () => name.value = "kobe"
        const changeAge = () => {
            age.value++;
            if (age.value > 25) {
                stop();
            }
        }

        return {
            name,
            age,
            changeName,
            changeAge
        }
    }
}
</script>
```

### 4）watchEffect的执行时机

```javascript
<template>
    <h2 ref="text">哈哈哈</h2>
</template>

<script>
import { ref, watchEffect } from 'vue'

export default {
    name: 'index',
    components: {

    },
    setup() {
        let text = ref(null)
        watchEffect(() => {
            console.log(text.value)
        }, {
            /**
             * pre 默认值 在元素 挂载 或者 更新 之前执行
             * post 监听DOM时，可修改为这个值
             * sync 强制效果始终同步触发
             */
            flush: "post"
        })
        return { text }
    }
}
</script>
```

## 11、watch

### 1）监听单个数据源

① reactive的监听

```javascript
<template>
    <p>{{ info }}</p>
    <button @click="fun">点</button>
</template>

<script>
import { reactive, watch } from 'vue'

export default {
    name: 'index',
    setup() {
        const info = reactive({ name: "哈哈哈", age: 22 })
        /**
         * 第一种写法
         */
        watch(() => info.age, (newVal, oldVal) => {
            console.log(newVal, oldVal)
        })
        const fun = () => {
            info.age++
        }
        return { info, fun }
    }
}
</script>
```

② 对reactive对象的监听

```javascript
<template>
    <p>{{ info.age }}</p>
    <button @click="fun">点</button>
</template>

<script>
import { reactive, ref, watch } from 'vue'

export default {
    name: 'index',
    setup() {
        const info = reactive({ name: "哈哈哈", age: 22 })
        /**
         * 下面这样写 就可以获取成一个 普通的对象
         */
        watch(() => ({ ...info }), (newVal, oldVal) => {
            console.log(newVal, oldVal)
        })

        const fun = () => {
            info.age++
        }
        return { info, fun }
    }
}
</script>
```

③ ref的监听

```javascript
<template>
    <p>{{ num }}</p>
    <button @click="fun">点</button>
</template>

<script>
import { reactive, ref, watch } from 'vue'

export default {
    name: 'index',
    setup() {
        const num = ref(0)
        watch(num, (newVal, oldVal) => {
            console.log(newVal, oldVal)
        })

        const fun = () => {
            num.value++
        }
        return { num, fun }
    }
}
</script>
```

### 2）监听多个数据源

```javascript
<template>
    <p>{{ info.age }}</p>
    <p>{{ num }}</p>
    <button @click="fun">点</button>
</template>

<script>
import { reactive, ref, watch } from 'vue'

export default {
    name: 'index',
    setup() {

        let info = reactive({ name: "哈哈哈", age: 22 })
        let num = ref(0)
        // watch([info, num], (newVal, oldVal) => {
        //下面可以 挨个打印
        watch([info, num], ([newInfo, newNum], [oldInfo, oldNum]) => {
            console.log(newInfo, newNum, oldInfo, oldNum)
        })

        const fun = () => {
            info.age++
            num.value++
        }
        return { info, num, fun }
    }
}
</script>
```

### 3）watch的选项

```javascript
<template>
    <p>{{ info }}</p>
    <button @click="fun">点</button>
</template>

<script>
import { reactive, ref, watch } from 'vue'

export default {
    name: 'index',
    setup() {

        let info = reactive({ name: "哈哈哈", age: 22, child: { name: "呵呵" } })
        watch(info, (newVal, oldVal) => {
            console.log(newVal, oldVal)
        }, {
            deep: true, // 深度监听
            immediate: true // 立即执行（西药第一次 进来就打印）
        })

        const fun = () => {
            info.child.name = "啊啊啊啊啊"
        }
        return { info, fun }
    }
}
</script>
```

## 12、provide

注：修改数据的时候，请尽量做到单向数据流。

[文档](https://cn.vuejs.org/guide/components/provide-inject.html#provide)

![image-20240225011508511](./images/image-20240225011508511.png)

## 13、hooks的使用

### 1）封装一个修改title

```javascript
import { ref, watch } from 'vue'
export default function (title = "默认值") {
    const titleRef = ref(title)
    /**
     * watch 是惰性的
     */
    watch(titleRef, newValue => {
        document.title = newValue
    }, { immediate: true })
    return titleRef
}
```

使用：

```javascript
<template>
    <p>hooks 的使用</p>
    <p> <button @click="fun">修改Title</button> </p>
</template>

<script lang='ts'>
import { defineComponent } from "vue";
import userTitle from "./userTitle";
export default defineComponent({
    name: "index",
    components: {},
    setup() {
        function fun() {
            let title = userTitle("你好");
            setTimeout(() => {
                title.value = "测试";
            }, 3000);
        }
        return { fun };
    },
});
</script>
```

### 2）监听界面滚动位置

```javascript
import { ref } from 'vue'
export default function () {
    const scrollX = ref(0);
    const scrollY = ref(0);
    document.addEventListener("scroll", () => {
        scrollX.value = window.scrollX;
        scrollY.value = window.scrollY;
    });
    return { scrollX, scrollY }
}
```

### 3）监听鼠标位置

```javascript
import { ref } from 'vue'
export default function () {
    const mouseX = ref(0)
    const mouseY = ref(0)

    window.addEventListener("mousemove", event => {
        mouseX.value = event.pageX
        mouseY.value = event.pageY
    })
    return { mouseX, mouseY }
}
```

# 三、自定义指令

文档：<https://v3.cn.vuejs.org/guide/custom-directive.html>

自定义局部指令：组件中通过 directives 选项，只能在当前组件中使用；

自定义全局指令：app的 directive 方法，可以在任意组件中被使用；

<font color=red>注：对DOM进行一些底层操作的时候，会用到自定义指令。</font>

## 1、局部指令

```javascript
<template>
    <div>
        <input type="text" v-focus>
    </div>
</template>
<script>
export default {
    directives: {
        focus: {
            mounted(el) {
                el.focus()
            }
        }
    }
}
</script>
```

## 2、全局指令

1）在main.js中定义

```javascript
const app = createApp(App);
/**
 * 全局指令
 */
app.directive("focus", {
    mounted(el) {
        el.focus()
    }
})
app.mount('#app');
```

2）在任意组件中的使用

```javascript
<template>
    <div>
        <input type="text" v-focus>
    </div>
</template>
```

## 3、指令的生命周期

1）created：在绑定元素的 attribute 或事件监听器被应用之前调用

2）beforeMount：当指令第一次绑定到元素并且在挂载父组件之前调用

3）mounted：在绑定元素的父组件被挂载后调用

4）beforeUpdate：在更新包含组件的 VNode 之前调用

5）updated：在包含组件的 VNode 及其子组件的 VNode 更新后调用

6）beforeUnmount：在卸载绑定元素的父组件之前调用

7）unmounted：当指令与元素解除绑定且父组件已卸载时，只调用一次

注：有自定义指令，至少有一个自定义指令。

### 1）执行顺序

① 初次加载的时候，执行的生命周期：

created、beforeMount、mounted

② 修改参数后的

beforeUpdate、updated

③ 销毁后

beforeUnmount、unmounted

### 2）生命周期中的参数

el、binding、vnode、preVnode

## 4、动态指令参数

```javascript
<template>
    <div>
        <h1 v-if="counter < 2" @click="add" v-num.aa.bb="'哈哈哈'">{{counter}}</h1>
    </div>
</template>
<script>
import { ref } from '@vue/reactivity'
export default {
    directives: {
        num: {
            created(el, binding) {
                console.log("created");
                console.log(el, binding);
            },
            beforeMount() {
                console.log("beforeMount");
            },
            mounted() {
                console.log("mounted");
            },
            beforeUpdate() {
                console.log("beforeUpdate");
            },
            updated() {
                console.log("updated");
            },
            beforeUnmount() {
                console.log("beforeUnmount");
            },
            unmounted() {
                console.log("unmounted");
            }
        }
    },
    setup() {
        let counter = ref(0)
        const add = () => {
            counter.value++
        }
        return { add, counter }
    }
}
</script>
```

## 5、时间戳转换案例

1）在src目录下创建一个directives文件夹，来存放自定义指令，里面的文件如下：

① 出口文件(index.js)

```javascript
/**
 * 总出口
 */
import time from "./time.js"
/**
* app 是从main.js中传入，而他是createApp的返回值
*/
export default function (app) {
    time(app)
}
```

② 自定义指令文件（这个  可以是多个）,以时间戳转换文件为例

```javascript
/**
 * 定义的自定义指令
 * 转换时间戳为普通格式
 * @param {*} app 
 * 使用：v-conversion-time
 * 可以传入参数 v-conversion-time="哈哈哈"
 */
export default function time(app) {
    let format = "" // 在这定义的变量，如果你在下面对其，进行修改，那么你修改一次后，之前都会复用这个修改，这是由于闭包产生的，所以不建议在这进行定义变量
    app.directive("conversion-time", {
        /**
         * 可以在这设置当前时间格式（如：2022/01/01），也就是使用 binding 参数给这传值
         * binding 是参数
         * 注：1、同时created、mounted中的binding是同一个值；
         *    2、如果你在created里面定义变量（或者修改传入的参数），想在mounted里面使用，见下面方法
         * 初始化一般都放在created里，在别的地方使用，能更好一些
         */
        created(el, binding) {
            binding.text = "哈哈哈"
        },
        mounted(el, binding) {
            console.log(binding.text);
            const text = el.textContent
            console.log(text)
            const timespan = parseInt(text)
            // 同意格式
            if (timespan.length === 10) {
                timespan = timespan * 1000
            }
            el.textContent = dateFormat(timespan)
        }
    })
}

function dateFormat(time) {
    let date = new Date(time);
    let year = date.getFullYear();
    /* 在日期格式中，月份是从0开始的，因此要加0
     * 使用三元表达式在小于10的前面加0，以达到格式统一  如 09:11:05
     */
    let month = date.getMonth() + 1 < 10 ? "0" + (date.getMonth() + 1) : date.getMonth() + 1;
    let day = date.getDate() < 10 ? "0" + date.getDate() : date.getDate();
    let hours = date.getHours() < 10 ? "0" + date.getHours() : date.getHours();
    let minutes = date.getMinutes() < 10 ? "0" + date.getMinutes() : date.getMinutes();
    let seconds = date.getSeconds() < 10 ? "0" + date.getSeconds() : date.getSeconds();
    // 拼接
    return year + "-" + month + "-" + day + " " + hours + ":" + minutes + ":" + seconds;
}
```

2）main.js中引入

```javascript
import { createApp } from 'vue'
import App from './App.vue'
// @ts-ignore
import directives from "./directives/index" // 引入

const app = createApp(App);

directives(app)

app.mount('#app');
```

3）在组件中的使用

```javascript
<template>
    <p v-conversion-time>{{time}}</p>
</template>

<script>
import { ref } from '@vue/reactivity'
export default {
    name: 'Time',
    setup() {
        const time = ref()
        time.value = new Date().getTime()
        return { time }
    }
}
</script>
```

# 四、Teleport

[文档](https://cn.vuejs.org/guide/built-ins/teleport.html)

Vue 鼓励我们通过将 UI 和相关行为封装到组件中来构建 UI。我们可以将它们嵌套在另一个内部，以构建一个组成应用程序 UI 的树。

然而，有时组件模板的一部分逻辑上属于该组件，而从技术角度来看，最好将模板的这一部分移动到 DOM 中 Vue app 之外的其他位置。

一个常见的场景是创建一个包含全屏模式的组件。在大多数情况下，你希望模态框的逻辑存在于组件中，但是模态框的快速定位就很难通过 CSS 来解决，或者需要更改组件组合。

1、组件

```javascript
<template>
    <div>
        <teleport to="#box">
            <p>容移动到的目标元素</p>
        </teleport>
    </div>
</template>
```

2、要定义id为box的元素，我目前把他定义在public下的index.html里

```javascript
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
 <!-- 在这定义了一个box -->
    <div id="box"></div>
    <!-- built files will be auto injected -->
</body>

</html>
```

![image-20240225013124657](./images/image-20240225013124657.png)
