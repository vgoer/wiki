<center>nuxt基础配置</center>







[toc]







## nuxt基础配置

> 配置文件：`nuxt.config.ts`





### 1. 运行时的全局变量

```ts
  // 运行时全局变量
  runtimeConfig: {
    // 客户端获取不到 1 undefined
    count: 1,
    
    // 客户端和服务端都可以获取
    public: {
      baseUrl: "localhost:8080",
    }

  }

```

> 使用

```ts
// 获取
const config = useRuntimeConfig()
console.log(config.count)
console.log(config.public.baseUrl)
```





### 2. 引入样式。

```shell
# 安装 sass
pnpm install -D sass
```

> 添加scss

```scss
// assets/css/base.scss

$myColor:geeen;

h3{
  color: red;
}
```

````ts
// 配置文件

  // 初始化样式
  // css:[
  //   "~/assets/css/base.scss"
  // ],

  // 引入变量css 需要在vite配置
  vite:{
    css:{
      preprocessorOptions:{
        scss:{
          additionalData:'@use "~/assets/css/base.scss" as *;'
        }
      }
    }
  },
````





### 3. 引入elementui

```ts
// 安装

pnpm install -D @element-plus/nuxt

// nuxt.config.ts
export default defineNuxtConfig({
  modules: ['@element-plus/nuxt'],
})
```





### 4. 区分服务端和客户端

```ts
if (import.meta.server){
  console.log("server...")
}else{
  console.log("cilen...")
}
```
