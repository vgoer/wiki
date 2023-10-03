---
title: 09Motion动画库
description: 
published: 1
date: 2023-06-27T10:48:31.377Z
tags: 
editor: markdown
dateCreated: 2023-06-27T10:48:29.899Z
---



<center>Motion</center>





[toc]





## Motion

> 而`Framer-motion`这个库，好用的React生态下的交互动效库。
>
> [中文文档](https://motion.framer.wiki/) [motion](https://www.framer.com/motion/)





### 1. 安装

```shell
npm i framer-motion

# 或者
pnpm i  framer-motion
```





### 2. 使用

> 简单使用

```tsx
import React from "react"
import { motion } from 'framer-motion'

const Motion: React.FC  = () => {
    return (
        <div className="box">
            <motion.div
                style={{
                    width:100,
                    height:100,  
                    backgroundColor:'white',
                    borderRadius:'20px'
                }}
                // 以下三个属性
                // 是motion组件提供的能力
                // 实现鼠标悬浮的效果
                whileHover={{
                    rotate:45,
                    scale:1.2
                }}
                // 让元素可以随意拖拽
                drag
                // 元素放手后会自动回到起始点
                dragSnapToOrigin
                >
            </motion.div>
        </div>
        
    )
}

export default Motion
```

> 搭配`styled-components`

```tsx
import React from "react"
import styled from 'styled-components'
import { motion } from 'framer-motion'


const MotionButton: React.FC = styled(motion.div)`
    width:100px;
    height:100px;
    border:none;
    cursor: pointer;
    background-color:#fff;
    border-radius:20px;
`

const Motion: React.FC  = () => {
    return (
        <div className="box">
            <MotionButton  WhileHover={{ rotate:45, scale:3 }} drag dragSnapToOrigin />
        </div>
        
    )
}

export default Motion
```

> 更多示例在文档哈

> 父子动画联动

```tsx
import React from "react"
import { motion } from 'framer-motion'

const list = {
    visible: { opacity: 1 },
    hidden: { opacity: 0 },
    
}
const item = {
    visible: { opacity: 1, x: 0 },
    hidden: { opacity: 0, x: -100 },
}

const Test :React.FC = () => {
    return (
        <motion.ul
            initial="hidden"
            animate="visible"
            variants={list}
            transition={
            {staggerChildren:0.5}
        }
        >
            <motion.li variants={item} />
            <motion.li variants={item} />
            <motion.li variants={item} />
        </motion.ul>
    )
}
export default Test
```

