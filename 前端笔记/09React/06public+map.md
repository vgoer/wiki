---
title: 06.public+map
description: public+map
published: 1
date: 2023-05-29T04:15:32.200Z
tags: react
editor: markdown
dateCreated: 2023-05-05T17:22:21.254Z
---

<center>public+map</center>





[toc]





### 1. public

> public目录文件
>
> 默认访问的就是该目录

```jsx
// 一般放 image 图片  mock 假数据等文件
/mock/data.json

{
"short_name": "React App",
"name": "Create React App Sample",
"icons": [
  {
    "src": "favicon.ico",
    "sizes": "64x64 32x32 24x24 16x16",
    "type": "image/x-icon"
  },
  {
    "src": "logo192.png",
    "type": "image/png",
    "sizes": "192x192"
  },
  {
    "src": "logo512.png",
    "type": "image/png",
    "sizes": "512x512"
  }
],
}
  

// 读取json
```

> 获取mock数据

```tsx
function App() {
    const [data, setData] = React.useState();

    const initData = () => {
        fetch('/mock/data.json')
        .then((response) => {

            return response.json()
        })
        .then((json) => {
            setData(json)
        })
    }

    // 钩子函数
    React.useEffect(() => {

        initData()
    },[])

  return (
    <div className="App">
        // 存在渲染到页面
        {!!data && 
        <>
            {data.name}
        </>
        }
    </div>
  );
}
```



### 2. Map file

> react默认打包带有map关系映射表 ==打包文件大，泄漏源码了==

```jsx
// 制造一个bug
const data = {}
const getBug =() => {
    console.log(data.name.length)
}

<MyButton color='red' onClick={getBug}>aaaa</MyButton>
```

> 模拟生产环境

```shell
# 开启环境的工具
npm i browser-sync

# 添加命令 开启web服务
	"execute": "browser-sync start --server \"build\" --files \"*\"",
```

> 报错居然能找到我们的源代码，而且代码下有`xxx.map`文件 这个就是map关系映射表





### 3.1如何取消呢

> 1.添加脚本命令

```json
// 取消这个
    "linuxbuild":"GENERATE_SOURCEMAP=false react-scripts build",
    "winbuild":"set \"GENERATE_SOURCEMAP=false\" && react-scripts build",
```

> 2. `.env`里面设置

```shell
# 像vue一样，添加 开发模式和打包模式配置文件
.env    .env.production


# 打包 配置文件里面设置
.env.production

GENERATE_SOURCEMAP=false
```

