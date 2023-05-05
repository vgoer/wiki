---
title: 03.Router6
description: Router6
published: 1
date: 2023-05-05T11:51:51.469Z
tags: react
editor: markdown
dateCreated: 2023-05-03T18:03:30.765Z
---

<center>RouterV6</center>





[toc]





## Router6

> 路由： 输入不同的地址，给出不同的响应 [Router](https://reactrouter.com/en/main)





### 1. 安装

> 博客[router](https://juejin.cn/post/7187199524903845946)

```shell
# index.js
npm install react-router-dom@6
```



### 2. 使用

> 包裹APP组件   和 vue路由模式一样
>
> `BrowserRouter` 常用
>
> `HashRouter` `#`加了 

```jsx
// BrowserRouter 常用的路由
import {BrowserRouter as Router} from 'react-router-dom'

<Router>
    <App />
</Router>
```

> 子组件中使用

```jsx
{/× 创建多个组件 ×/}
// Link 类似a标签  Route路由  Routes 路由组
import {Link,Route,Routes} from 'react-router-dom'


function App() {
  return (
    <div className="App">
        <nav>
            <Link to="/">Home</Link>
            <Link to="/about">About</Link>
            <Link to="/main">Main</Link>
        </nav>

        <Routes>
            <Route path='/' element={<Home /> } />
            <Route path='about' element={<About /> } />
            <Route path='main' element={<Main /> } >

                {/* index 默认到这个路由 */}
                <Route index element={<MainBook />} />

                <Route path='books' element={ <MainBook />} />
                <Route path='users' element={ <MainUser />} />
            </Route>


            // 404page
            <Route path='*' element={<h2>Error: notFourd</h2>} />
        </Routes>
    </div>
  );
}
```

> `main.jsx` 主组件中

```jsx
import {Link,Outlet} from 'react-router-dom'

export default function Main(){

    return (

        <div>
            <h2>hello main page</h2>

            <nav>
                <Link to="/main/users">User</Link>
                <Link to="/main/books">Books</Link>
            </nav>

            {/* 站位符 */}
            <Outlet />
        </div>
    )
}
```

> 前进后退 路由跳转

```jsx
import { useNavigate } from "react-router-dom"


// useNavigate 钩子函数
const navigate = useNavigate()

const goBack = () => {
    navigate(-1)
}
const goToUser = (name) => {

    navigate(`/main/users/${name}`,
    {
        // 在该路由下清空缓存，路由只记录访问一次
        replace:true
    }
    )
}
```

> 获取参数

```jsx
import {useParams,useSearchParams} from 'react-router-dom'

// 获取浏览器url变量
const params = useParams()  // :id
const [search] = useSearchParams() // ?id=1&name=abc

const id = search.get('id')
const name = search.get('name')
```

