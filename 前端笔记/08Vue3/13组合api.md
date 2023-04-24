---
title: 13.组合api
description: 组合api
published: 1
date: 2023-04-24T12:08:44.231Z
tags: vue3
editor: markdown
dateCreated: 2023-04-24T12:08:40.843Z
---

<center>组合api</center>



[toc]



## 组合api

> 以前学习的都是`选项api`
>
> 选项api: `option api` 数据和方法都是放在`data() `和 `methods()`下
>
>  组合api: `composition api`  数据方法可以写在一起，更方便维护

![img](https://img-blog.csdnimg.cn/img_convert/e8ad905d83aaec45451797517ef453aa.png)



> 优势: 逻辑分明 可维护性 高

```vue
<template>
    <h3>{{msg}}</h3>
    <button @click="setMsg">修改</button>
</template>

<script>
  // 选项api基本写法
  export default{
    // setup 内定义数据 组合api
    setup(){
      let msg = "hi vue"

      let setMsg = () => {
        console.log('aaa')
      }
      // 暴露数据和函数 
      return{
        msg,
        setMsg
      }
    },
  }
</script>
```



### 1. 组合api变量

> 目前我们的数据都不是==响应式的==
>
> > `ref `修改==字符==为响应式 ` reactive `修改==数组和对象等==为响应式 

```js
// ref
// 引入
import { ref } from 'vue'

// ref 对象
let msg = ref("hi html")

let setMsg = () => {
    // 字符的响应式  .value 才有
    console.log(msg)
    msg.value = "hi vue3"
}
```

```js
// reactive
let Cat = reactive({
    name:'小白',
    age:3,
    love:'猫娘'
})
```



### 2.组合api生命周期

> 常用的就四个钩子函数

```js
//beforeCreate 和 created 创建之前和创建之后没有了  直接写在setup(){} 下就行了
//其他的钩子函数 加on引入并使用

import { ref,onMounted } from 'vue'
setup(){
    let msg = ref('')
    let list = [1,2,3,'开始了']

    // 挂载后生命周期
    onMounted(() => {
        let i = 0
        let t = setInterval(() => {
            msg.value = list[i]
            i++
            if(i === 4){
                clearInterval(t)
            }
        }, 1000);
    })
```



### 3.组合api 计算属性

> ==多个地方调用==一个函数，可以使用计算属性把值缓存起来

```js
import { ref,computed } from 'vue'
export default{
  setup(){

    let msg = ref("水果价格")
	//  computed 计算属性
    let tatle = computed(() => {
      return 100
    })
    return{
      msg,
      tatle
    }
  }
}
```



### 4.组合api提取功能

> 如果一个功能要在网页使用几十次，你有什么办法么?

```js
// 组合api 提取功能  
// math.js 功能

// 抽离重复的功能
export default function(number){

    function add(){
        // ref 返回值是对象  .value才能修改
        number.value++
        console.log(number)
    }

    function sub(){
        if(number.value == 0){
            console.log("不能在减了")
            return false
        }
        number.value--

    }
    // 暴露对象 方法
    return {
        add,
        sub
    }
}
```

```vue
// 引入和使用
<template>
    <div>
      <button @click="sub">-</button>
      <span>{{num}}</span>
      <button @click="add">+</button>
    </div>  
</template>


<script>
import { reactive, ref } from 'vue'
// 引入
import math from './math.js'

  export default{
    setup(){
      let num = ref(0)
      // 解构赋值
      let {add,sub} = math(num)
      // 暴露
      return {
        num,
        add,
        sub
      }
    }
  }
</script>

```

> 使用组合api实现  ==水果列表==
>
> > so cool  哈哈

```vue
<template>

  <h1>水果列表商城</h1>

  <form @submit.prevent="addFrite">
    名称: <input type="text" v-model="name">
    <br>
    单价: <input type="text" v-model="price">
    <br>
    <button>添加</button>
  </form>
  <ul>
    <li v-for="item,index in friteList" key="index">
      水果名称: --- {{item.name}} ---
      单价: --- {{item.price}} ---
      数量: --- 
      <button @click="sub(index)">-</button>
      <span>{{item.count}}</span>
      <button @click="add(index)">+</button>
    </li>
     
  </ul>
  <p>总价为: {{totalPrice}}</p>
</template>

<script>
import { reactive, ref,computed } from 'vue'

export default{
  setup(){
    let name = ref('')
    let price = ref(1)
    
    let friteList = reactive([])

    //  添加列表
    let addFrite = ()=>{
      let List = {
        name : name.value,
        price : price.value,
        count: 1,
      }
      friteList.push(List)
    }
     
    // 添加数量
    let add = (i) => {
      friteList[i].count++
    }

    // 减少数量
    let sub = (i) => {
      friteList[i].count--
      console.log(friteList[i]);
      if(friteList[i].count === 0 && confirm('是否删除数据?')){
        friteList.splice(i,1)
      }
    }

    // 总价
    let totalPrice = computed(() => {
      let sum = 0;
      friteList.forEach((item) => {
        sum = (item.price * item.count)
      })
      return sum

    })


    return {
      name,
      price,
      addFrite,
      friteList,
      add,
      sub,
      totalPrice
    }
  }
}

</script>
```



