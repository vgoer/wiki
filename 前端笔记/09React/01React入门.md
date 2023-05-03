---
title: 01.React入门
description: React入门
published: 1
date: 2023-05-03T11:44:47.130Z
tags: react
editor: markdown
dateCreated: 2023-05-03T11:44:47.130Z
---

<center>React</center>





[toc]







## React

> 用于构建 Web 和原生交互界面的库 [react](https://react.dev/)  [cn](https://react.docschina.org/)
>
> 传统页面：文档式的，内容割裂，无法组件化和复用
>
> React库：虚拟Dom,JSX语法，动态交互。 组件化，复用性强





### 1. 安装

```shell
# 1.js
npx create-react-app my-app

# 2.ts 语法
npx create-react-app my-app --template typescript

# 启动
cd my-app
npm start
```



### 2. jsx

> `jsx语法`： js里面可以直接写html语法(函数返回值可以是html)
>
> `className`,`backgroundColor`都是驼峰命名的

```jsx
// js
function myBlock(){
    return "hello react!"
}

// jsx  传参数
function myBlock(name,color){
    return (
        <div className='box' style={{color: color,fontSize:'40px',backgroundColor:'blue'}}>
            hello {name}
        </div>
    )
}

// app内调用
function App() {
  return (
    <div className="App">
        {myBlock('goer','red')}
    </div>
  );
}
```





### 2.组件通信

> 父组件传子组件`props`

```jsx
function myBlock(props){
    return (
        <div style={{color: props.color,fontSize:'40px',backgroundColor:'blue'}}>
            hello {props.name}
        </div>
    )
}
// app
function App() {
  return (
    <div className="App">
        {myBlock({
            name:'vgoer',
            color:'red'
        })}
    </div>
  );
}
```

> 解构

```jsx
function myBlock({name,color,fontSize}){
    return (
        <div style={{color,fontSize,backgroundColor:'blue'}}>
            hello {name}
        </div>
    )
}
function App() {
  return (
    <div className="App">
        {myBlock({
            name:'goer',
            color:'red',
            fontSize:'40px'
        })}
    </div>
  );
}
```

> 变身为函数式组件 ==大写==

```jsx
function MyBlock({name,color,fontSize}){
    return (
        <div style={{color,fontSize,border:`1px solid ${color}`}}>
            hello {name}
        </div>
    )
}
function App() {
  return (
    <div className="App">
        <MyBlock name='goer' color='red' fontSize='40px' />
        <MyBlock name='python' color='blue' fontSize='20px' />
        <MyBlock name='javascript' color='green' fontSize='10px' />
    </div>
  );
}
```



### 3.子组件

> `children` 子组件，子标签。 和vue里面的插槽一样

```jsx
function MyBlock({name,color,fontSize,children}){
    return (
        <div style={{color,fontSize,border:`1px solid ${color}`}}>
            hello {name}
            <div>
                {children}
            </div>
        </div>
    )
}
// 使用
<MyBlock name='python' color='blue' fontSize='20px'>
    <p>hello children</p>
</MyBlock>
```





### 4. 事件

> `onClick` on开头的事件

```jsx
function MyBlock({name,color,fontSize,children}){

    const onBtnClick = () => {
        alert(`${name} clicked`)
    }

    return (
        <div style={{color,fontSize,border:`1px solid ${color}`}}>
            hello {name}
            <div>
                {children}
            </div>
            <button onClick={onBtnClick}>click</button>
        </div>
    )
}
```



### 5.state状态

> `state`  修改状态后，重新渲染页面  `useState钩子`

```jsx

function MyBlock({name,color,fontSize,children}){

    // useState 钩子函数，返回一个数组，第一个值变量，第二个值为设置该变量的方法
    const [label,setLabel] = useState('click')
    const [blockName,setblockName] = useState(name)

    const onBtnClick = () => {
        alert(`${name} clicked`)
        setLabel('CLICKED')
        setblockName(`${name} CLICKED`)
    }
    return (
        <div style={{color,fontSize,border:`1px solid ${color}`}}>
            hello {name} -- {blockName}
            <div>
                {children}
            </div>
            <button onClick={onBtnClick}>{label}</button>
        </div>
    )
}
```



### 6.生命周期

> `useEffect钩子`真实DOM加载完执行 和vue`Mounted`一样
>
> 监听值的变化也是用它

```jsx
useEffect(() => {

    alert('Mounted!!!')

    // 给一个返回值就是组件注销执行
    return (() => {
        alert('exit!!!')
    })
},[])

// 监听值
useEffect(() => {

    alert('lable chage')
},[label])
```

