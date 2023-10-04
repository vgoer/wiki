---
title: 21Electron
description: 
published: 1
date: 2023-06-09T10:10:33.609Z
tags: 
editor: markdown
dateCreated: 2023-05-11T10:46:13.034Z
---

<center>Electron</center>



[toc]





# Electron 

> 用前端语言==开发 桌面应用程序==



### 1. 环境

> 安装环境

```shell
1. 安装node  使用nvn
node -v 
npm -v

2. 初始化项目
npm init

# 3. 全局安装 
npm install -g electron 
3.1 一般局部  npm install --save-dev electron@18.3.5  # 指定版本

4. 添加脚本启动命令 script package.json
    "start": "electorn ."

5. 启动 
npm start
```

> 创建app 和 启动窗口

```js
// index.js
// 引入 electron 模块
const {app,BrowserWindow} = require('electron')

// ready 事件
app.on('ready' , function() {
    const mianApp = new  BrowserWindow({
        width:800,
        height:700
    })
    // 加载这个文件
    mianApp.loadFile('index.html')
})
```

```html
<!-- index.html -->
<!DOCTYPE html>
<html lang="en">
<head>
    <title>app</title>
</head>
<body>
    <h3>first app</h3>
</body>
</html>

npm start 就可以看到你的应用了
```



### 2. app 模块

> 核心模块 app

> 生命周期 

```js
ready 应用程序初始化完成

browser-window-created 窗口创建完成触发

before-quit  窗口关闭之前

will-quit  窗口关闭了  但程序还没关闭 即将关闭

quit  应用程序关闭触发
```

```js
// 引入 electron 模块
const {app,BrowserWindow} = require('electron')

app.on('browser-window-created', () => {
    console.log('browser-window-created')
})

app.on('quit', () => {
    console.log('quit')
})
app.on('will-quit', () => {
    console.log('will-quit')
})
app.on('before-quit', () => {
    console.log('before-quit')
})

// ready 事件
app.on('ready' , function() {

    console.log('ready');

    const mianApp = new  BrowserWindow({
        width:800,
        height:700
    })
    // 加载这个文件
    mianApp.loadFile('index.html')
})
// 注意看打印顺序
```

> 创建窗口不用放到 ready事件中

```js
// whenReady() 方法 返回值是promise对象
app.whenReady().then(() => {
    const mianApp = new  BrowserWindow({
        width:800,
        height:700
    })
    // 加载这个文件
    mianApp.loadFile('index.html')
})
```

> 禁止双开  限制打开程序

```js
requestSingleInstanceLock()  返回true和false

配合 second-instance 事件执行
```



### 3. BrowserWindow 模块

> BrowserWindow : 创建窗口大小位置等

```js
loadFile  用于加载本地文件， 也可以加载其他类型的文件(但无意义)

loadUrl   用于加载远程文件
```

```js
app.whenReady().then(() => {
    const mianApp = new  BrowserWindow({
        width:800,
        height:700
    })
    // 加载这个文件
    mianApp.loadURL('https://www.google.com')
})
```

> 设置边框样式

```js
const mainApp = new BrowserWindow({
    width:900,
    height:700,
    frame:false,        // 菜单标题和最大化最小化关闭按钮
    resizable: true,   // 鼠标改变窗口大小

    // 可以拖拉，但有限制
    maxWidth:1000,
    minWidth:600,
    maxHeight:1000,
    minHeight:600,
})
```

> 打开白屏加载很久

```js
app.on('ready' , function() {

    const mainApp = new BrowserWindow({
        width:900,
        height:700,
        
        show: false,   // 有白屏的话  设置窗口是否显示
    })
    mainApp.loadFile('index.html')

    // 调用ready-to-show 事件
    mainApp.once('ready-to-show' , function(){
        // 执行show方法显示
        mainApp.show()
    })
})
```

> 使用node的模块		

```js
show: false,   // 有白屏的话  设置窗口是否显示
    webPreferences:{
        nodeIntegration:true,   // 是否开启  node
        contextIsolation:false, // 是否开启上下文隔离
    }
```

> 移动窗口

```js
1. setBounds   后面会使用到
mainApp.setBounds({
    x:600,
    y:900
})

2. setPosition(x,y,animate)
mainApp.setPosition(100,200)
```

> 其他重要事件

```js
// show 方法
mainApp.on('show', function(){
    mainApp.maximize() // 最大化

    mainApp.minimize()  // 最小化
    mainApp.close()     // 关闭
}) 
```









