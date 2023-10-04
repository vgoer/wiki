---
title: React项目
description: 
published: 1
date: 2023-06-09T10:15:28.237Z
tags: 
editor: markdown
dateCreated: 2023-05-22T10:38:49.587Z
---

<center>项目之React篇</center>





[toc]







## React

> React项目搭建



### 1. 安装

```shell
# react官方脚手架
npx create-react-app reactproject --template typescript

# vite
npm init vite@latest my-app
```

```shell
# 安装依赖
npm i/pnpm i

# ts爆红 找不到path等问题
npm install @types/node -D

# 运行项目
npm run dev 
```



### 2. 代码规范

> 代码规范===重要== [blog](https://juejin.cn/post/7123612981895626760)

> 一般 ESLint 用于检测代码风格代码规范，Prettier 用于对代码进行格式化。

#### ESLint

```shell
npm init @eslint/config
```

#### Prettier

```shell
npm i  prettier -D
```

> 然后再根目录创建 `.prettierrc.js` 配置文件

```js
module.exports = {
    printWidth: 80,
    tabWidth: 2,
    useTabs: false,
    singleQuote: true,
    semi: false,
    trailingComma: "none",
    bracketSpacing: true
}

// 四个空格   .eslintrc.js 中，找到 indent，然后修改为 4 即可。
'indent': [2, 4, {
  'SwitchCase': 1
}],
    
npm run lint -- --fix
```

#### ESLint+Prettier

```shell
npm i eslint-config-prettier eslint-plugin-prettier -D
```

> 现在更改 Eslint 的配置文件 `.eslintrc.cjs` 在里面加入 Prettier 相关配置。

```diff
module.exports = {
    "env": {
        "browser": true,
        "es2021": true
    },
    "extends": [
        "eslint:recommended",
        "plugin:react/recommended",
        "plugin:@typescript-eslint/recommended",
+       "plugin:prettier/recommended"
+       'plugin:react/jsx-runtime'
    ],
    "parser": "@typescript-eslint/parser",
    "parserOptions": {
        "ecmaFeatures": {
            "jsx": true
        },
        "ecmaVersion": "latest",
        "sourceType": "module"
    },
    "plugins": [
        "react",
        "@typescript-eslint",
+       "prettier"
    ],
    "rules": {
+       "prettier/prettier": "error",
+       "arrow-body-style": "off",
+       "prefer-arrow-callback": "off"
    }
}
```

> 接下来在 `package.json` 的 `script` 中添加命令。

```js
{
    "script": {
        "lint": "eslint --ext .js,.jsx,.ts,.tsx --fix --quiet ./"
    }
}
```

> vite中引入ESLint

```shell
npm i vite-plugin-eslint -D
```

> `vite.config.ts`配置

```js
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'
import viteEslint from 'vite-plugin-eslint'

// https://vitejs.dev/config/
export default defineConfig({
  plugins: [
    react(),
    viteEslint({
      failOnError: false
    })
  ]
})
```

> 接下来就可以变开发变提示了



#### Husky + lint-staged

> 对 git commit进行代码校验  具体参考博客呀

```shell
npm i husky -D
```





### 3.多环境

> 配置多个环境  `react脚手架可以获取` `console.log(process.env)`

```shell
# 这边就配置 开发环境和生产环境了
# 根目录新建 开发环境 
.env 
VITE_NAME=app
VITE_APP_PROXY_URL=http://127.0.0.1/api
```

> 生产环境

```shell
.env.production
VITE_NAME=proname

GENERATE_SOURCEMAP=false

VITE_APP_PROXY_URL=https://aa.com/api
```

> 使用

```shell
`react脚手架可以获取` `console.log(process.env)`  # 脱离了node的环境
console.log(import.meta.env)
console.log(import.meta.env.VITE_APP_PROXY_URL)
```





### 4.vite配置

> 一些好用的vite配置  `vue和react公用的`

```js
// 1. 目录代替
  resolve:{
    alias:{
      '@':path.resolve('./src')   // @代替src
    }
  },
// 为了不让ts报错 tsconfig.json配置
    "baseUrl": "./src",
    "paths": {
      "@/*":["*"]
    },
      
// 2. 允许跨域
  server:{
    host:true,  // 内网
    cors:true,  //为开发服务器配置 CORS , 默认启用并允许任何源
    open:true,  //服务启动时自动在浏览器中打开应用
    port:'8888',
      proxy: {
          "/api": {
            target: "https://yourBaseUrl",
            changeOrigin: true,
            cookieDomainRewrite: "",
            secure: false,
          },
        },
      },
  },
```

> 最终解决跨域后端来代理。 可以参考[blog ](https://blog.csdn.net/qq_37656005/article/details/129056780) [nginx](https://blog.csdn.net/xiao_bai_9527/article/details/120022750)





### 5. antd按需引入

> antd [antd](https://ant.design/)

```shell
npm install antd --save
# 按需引入
npm install less less-loader vite-plugin-style-import -D
```

```js
// viteconfig.json
import { createStyleImportPlugin, AntdResolve } from 'vite-plugin-style-import'
plugins:[
 react()
 createStyleImportPlugin({
    resolves: [AntdResolve()]
 })
]
```

