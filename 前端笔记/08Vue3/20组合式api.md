---
title: 20组合式api
description: 
published: 1
date: 2023-07-13T01:00:54.798Z
tags: 
editor: markdown
dateCreated: 2023-05-11T10:53:33.223Z
---

<center>Vue3 Setup</center>



[toc]





## Vue3 Setup

> `setup`语法糖，代码更优雅，更简介





### 1. 子父组件通信

> 组件通信

* `defineProps`父传子

```vue
<template>
    <!-- 父组件 -->
    <h2>test page</h2>
    <sunTest :info="msg"></sunTest>

</template>
<script setup>
import { ref } from 'vue';
import sunTest from './sunTest.vue';
const msg = ref('hello vue')

</script>
```

```vue
<template>
    <!-- 子组件 -->
    <h2>{{ info }}</h2>
    
</template>
<script setup>
const props = defineProps({
    info:String,
})
</script>
```

* `defineEmits `子传父

```vue
<template>
    <!-- 子组件 -->
    <button @click="clickChild">点击我</button>
</template>
<script setup>

// 使用defineEmits创建名称，接受一个数组
const emit = defineEmits(['clickChild','outherClick'])

const clickChild = () => {
    let obj = {name:'goer',age:10}
    emit('clickChild',obj)
}
</script>
```

```vue
<template>
    <!-- 父组件 -->
    <h2>test page</h2>

    <!-- clickChild是子组件绑定的事件，click是父组件接受方式 -->
    <sunTest @clickChild="clickEven"></sunTest>
</template>
<script setup>
import { ref } from 'vue';
import sunTest from './sunTest.vue';
const clickEven = (obj) => {
    console.log(obj)
}
</script>
```

* `defineExpose`父组件获取子组件导出的属性和方法

```vue
<template>
    <!-- 子组件 -->
    <h2>我是子组件</h2>
</template>
<script setup>
import { reactive } from 'vue';


let obj = reactive({name:'goer', age:18,love:'code'})

const resultAdd = (a,b) => {
    return a+b;
}
defineExpose({obj,resultAdd})
</script>
```

```vue
<template>
    <!-- 父组件 -->
    <h2>test page</h2>

    <!-- clickChild是子组件绑定的事件，click是父组件接受方式 -->
    <sunTest ref="sunRef"></sunTest>

    <button @click="getSunData">获取数据</button>
</template>
<script setup>
import { ref } from 'vue';
import sunTest from './sunTest.vue';

const sunRef = ref()
const getSunData = () => {

    console.log(sunRef.value.obj)
    console.log(sunRef.value.resultAdd(10,10))
}
</script>
```



### 2. 计算属性

> 计算属性需要时才更新

> `*只有getter的计算属性*`

```vue
<template>
    <!-- 子组件 -->
    <h2>我是子组件</h2>

    <p>{{ message }}</p>
    <p>{{ message }}</p>
    <p>{{ message }}</p>
</template>
<script setup>
import { computed, ref } from 'vue';

const msg =ref('vue3')
// 组件中使用了三次 实际才调用一次
const message = computed(() => {
    console.log(msg.value)
    return msg.value = 'hi vue3'
})
</script>
```

> `*有getter与setter的计算属性*`

```js
// 可计算的属性
import { computed, ref } from 'vue';
const firstName = ref('Goer')
const lastName = ref('Ma')
const fullName = computed({

    //getter
    get(){
        return firstName.value + " " + lastName.value;
    },
    //setter
    set(newValue){
        [firstName.value,lastName] = newValue.split(" ")
    }
})
console.log(fullName.value)
console.log(firstName.value,lastName.value)
```



### 3.异步组件

> `suspense`等义务逻辑完成后渲染到页面
>
> default 插槽里的节点展示**异步请求完成后，显示的模板内容；**
> fallback 插槽里的节点则展示**异步请求加载中，显示的内容**

```js
<template>
    <h2>我是异步组件</h2>
    <div v-for="item in list" :key="item.name"> {{ item.name }} </div>
</template>
<script setup>

const getList = () => {
    return new Promise((resolve, reject) => {
      setTimeout(() => {
        let res = [{name: '张三', age: 11}, {name: '李四', age: 22}]
        resolve(res)
      }, 3000)
    })
  }
  // <script setup> 中可以直接使用 await， 但函数声明仍需async await搭配使用
  let list= await getList()
</script>
  
// 父组件
<template>
    <!-- 父组件 -->
    <h2>test page</h2>
    <Suspense>
        <template #default>
            <AsyncCop></AsyncCop>
        </template>
        <template #fallback>
            Loading...
        </template>
    </Suspense>
</template>
<script setup>
import { defineAsyncComponent } from 'vue';
// 通过defineAsyncComponent加载异步配合import 函数模式便可以分包
const AsyncCop = defineAsyncComponent(() => import('./sunTest.vue'))
</script>
```

```js
// watch
const state = reactive({
    num: 0,
})
watch(
    // 监听多层对象
    () => state.num,
    (newValue,oldValue) => {
        console.log(newValue)
        console.log(oldValue)
    }
)
```


### 4. 瞬移组件

> `teleport`Teleport 是一种能够将我们的模板渲染至指定DOM节点

```vue
<teleport to="body">
    <div>瞬移到body中</div>
</teleport>
```



### 5. 自定义指令

> Vue 还允许你注册自定义的指令 (Custom Directives)。

```js
// 全局注册指令
// 使 v-focus 在所有组件中都可用
app.directive('focus', {
   mounted: (el) => el.focus()
})

// 使用
<input type="text" v-focus>
```



### 6.依赖注入	

> 子孙组件获取父组件数据、方法

```vue
// 父组件
<script setup>
import { provide,ref } from 'vue';
let msg = ref('hello')
// 注入名   和     值
provide('message',msg)

</script>

// 子孙组件
<template>
    <p>{{ message }}</p>
    <button @click="Add">Add</button>
</template>
<script setup>
import { inject } from 'vue';
const message = inject('message')

const Add = () => {
    message.value = 'yes'
}
</script>
```



### 7. 插槽

> 组件接收模版的内容

```vue
// 实例
<button class="fancy-btn">
  <slot></slot> <!-- 插槽出口 -->
</button>

// 使用
<FancyButton>
  Click me! <!-- 插槽内容 -->
</FancyButton>
```

> `v-solt`简写为`#`

* 具名插槽

```vue
// 组件
<template>
    <button class="fancy">
        <slot name="left"></slot>
        <slot></slot>
        <slot name="right"></slot>
    </button>
</template>

// 使用
<template>
    <h2>test page</h2>
    <btn v-slot:left>
        <div>左边</div>
    </btn>
    <btn #default>
        <div>中间</div>
    </btn>
    <btn #right>
        <div>右边数据</div>
    </btn>
</template>
<script setup>
import btn from './btn.vue'
</script>
```

* 作用域插槽

> 子组件绑定参数，传递父组件slot使用

```vue
<template>
    <button class="fancy">
        <slot :data="list"></slot>
    </button>
</template>
<script setup>
const list = [1,2,3,4]
</script>

// 获取
<btn #default ="{data}">
    <div>中间</div>
    <p>{{ data }}</p>
</btn>
```



### 8.组合式函数

> 也叫钩子函数`hook`
>
> 组合式函数约定用驼峰命名法命名，并以“use”作为开头。

```js
// mouse.js
// mouse.js
import { ref, onMounted, onUnmounted } from 'vue'

// 按照惯例，组合式函数名以“use”开头
export function useMouse() {
  // 被组合式函数封装和管理的状态
  const x = ref(0)
  const y = ref(0)

  // 组合式函数可以随时更改其状态。
  function update(event) {
    x.value = event.pageX
    y.value = event.pageY
  }

  // 一个组合式函数也可以挂靠在所属组件的生命周期上
  // 来启动和卸载副作用
  onMounted(() => window.addEventListener('mousemove', update))
  onUnmounted(() => window.removeEventListener('mousemove', update))

  // 通过返回值暴露所管理的状态
  return { x, y }
}

// 使用
<template>
    <p>at: {{ x }}, {{ y }}</p>
</template>
<script setup>
import {useMouse} from './mouse'

const {x,y} = useMouse()
</script>
```





### 9. 监听

> 监听 `watch` 每次响应式状态发生变化时触发回调函数：

```js
const x = ref(0)
const y = ref(0)

// 单个 ref
watch(x, (newX) => {
  console.log(`x is ${newX}`)
})

// getter 函数
watch(
  () => x.value + y.value,
  (sum) => {
    console.log(`sum of x + y is: ${sum}`)
  }
)

// 多个来源组成的数组
watch([x, () => y.value], ([newX, newY]) => {
  console.log(`x is ${newX} and y is ${newY}`)
})

// 监听对象属性时
const state = reactive({
    num: 0,
})
watch(
    // 监听多层对象
    () => state.num,
    (newValue,oldValue) => {
        console.log(newValue)
        console.log(oldValue)
    }
)
```

> `watchEffect`：自动追踪所有能访问到的响应式属性。代码更加简洁

```js
// watch
const todoId = ref(1)
const data = ref(null)

watch(todoId, async () => {
  const response = await fetch(
    `https://jsonplaceholder.typicode.com/todos/${todoId.value}`
  )
  data.value = await response.json()
}, { immediate: true })

// watchEffect
watchEffect(async () => {
  const response = await fetch(
    `https://jsonplaceholder.typicode.com/todos/${todoId.value}`
  )
  data.value = await response.json()
})
```

















