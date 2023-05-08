---
title: 17rrewb
description: 
published: 1
date: 2023-05-08T16:16:10.133Z
tags: 
editor: markdown
dateCreated: 2023-05-08T16:16:08.536Z
---

<center>rrweb</center>



[toc]





## Rrweb

> 视频回溯功能 [rrweb](https://github.com/rrweb-io/rrweb)

```shell
npm install --save rrweb		
```

```js
// 使用
import { ref,reactive, onMounted} from 'vue';
import {record}  from 'rrweb'
import 'rrweb-player/dist/style.css'
import rrwebPlayer from 'rrweb-player'

const myref = ref(null)
const events = []
// 开启视频
const start = () => {
    new record({
        emit(event){
            console.log(event)
            events.push(event)
        }
    })
}

// 结束
const end = () => {
    new rrwebPlayer({
        target: myref.value,
        props:{
            events
        }
    })
}
```

