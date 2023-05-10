<center>vueuse</center>



[toc]





## VueUse

> VueUse 是一个基于 [Composition API](https://v3.vuejs.org/guide/composition-api-introduction.html) 的工具集 [vueuse ](https://vueuse.org/)





### 1. 安装

```shell
npm i @vueuse/core
```





### 2. 使用

> 很强大方便的功能

* 获取鼠标

```js
import {useMouse} from '@vueuse/core'
const {x,y} = useMouse()


<div>pos: {{x}}, {{y}}</div>
```

* 本地存储

```js
// LocalStorage
import {useLocalStorage} from '@vueuse/core'

const store = useLocalStorage(
    'key',{
        name:'aaaa',
        kkk:'akey',
    }
)
console.log(store.value)
```

* base64编码

> 支持纯文本、缓冲区、文件、画布、对象、地图、集合和图像。

```js
import { ref,watchEffect } from 'vue'
import { useBase64 } from '@vueuse/core'
const text = ref('aaa')

const { base64 } = useBase64(text)
watchEffect(() => {
  console.log(base64.value)
})
```

* 蓝牙

```js
import { useBluetooth } from '@vueuse/core'

const {
  isSupported,
  isConnected,
  device,
  requestDevice,
  server,
} = useBluetooth({
  acceptAllDevices: true,
})

<template>
  <button @click="requestDevice()">
    Request Bluetooth Device
  </button>
</template>
```

