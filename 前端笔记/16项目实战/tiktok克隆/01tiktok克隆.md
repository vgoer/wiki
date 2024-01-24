<center>tiktok克隆</center>





[toc]







## tiktok克隆

> tiktok: [bili](https://www.bilibili.com/video/BV14M411575a)







### 1. 安装项目

> 前端。 [nuxt中文文档](https://nuxt.com.cn/)



* nuxt

```shell
npm init vite@latest tiktok_clone_app  # 选择nuxt

cd tiktok_clone_app

pnpm i 
```

* tailwindcss

> tailwindcss: [blog](https://www.tailwindcss.cn/docs/guides/nuxtjs)

```shell
pnpm install -D tailwindcss postcss autoprefixer

npx tailwindcss init
```

> `nuxt.config.js`

```js
// https://nuxt.com/docs/api/configuration/nuxt-config
export default defineNuxtConfig({
  devtools: { enabled: true },
  postcss: {
    plugins: {
      tailwindcss: {},
      autoprefixer: {},
    },
  },
})
```

> `tailwind.config.js`

```js
/** @type {import('tailwindcss').Config} */
module.exports = {
  content: [
    "./components/**/*.{js,vue,ts}",
    "./layouts/**/*.vue",
    "./pages/**/*.vue",
    "./plugins/**/*.{js,ts}",
    "./app.vue",
    "./error.vue",
  ],
  theme: {
    extend: {},
  },
  plugins: [],
}
```

>  `./assets/css/main.css`  

```css
@tailwind base;
@tailwind components;
@tailwind utilities;
```

> `nuxt.config.js` 引入css

```js
// https://nuxt.com/docs/api/configuration/nuxt-config
export default defineNuxtConfig({
  devtools: { enabled: true },
  css: ['~/assets/css/main.css'],
  postcss: {
    plugins: {
      tailwindcss: {},
      autoprefixer: {},
    },
  },
})
```



* 格式化

> prettier

```shell
pnpm i prettier -D

#  创建  .prettierrc.cjs 

module.exports = {
  printWidth: 80, // 单行长度
  tabWidth: 4, // 缩进长度
  useTabs: false, // 使用空格代替tab缩进
  semi: true, // 句末使用分号
  singleQuote: true, // 使用单引号
  quoteProps: 'as-needed', // 仅在必需时为对象的key添加引号
  jsxSingleQuote: true, // jsx中使用单引号
  trailingComma: 'all', // 多行时尽可能打印尾随逗号
  bracketSpacing: true, // 在对象前后添加空格-eg: { foo: bar }
  jsxBracketSameLine: true, // 多属性html标签的‘>’折行放置
  arrowParens: 'always', // 箭头函数单参数时包裹圆括号
  requirePragma: false, // 无需顶部注释即可格式化
  insertPragma: false, // 在已被preitter格式化的文件顶部加上标注
  htmlWhitespaceSensitivity: 'ignore', // 对HTML全局空白不敏感
  endOfLine: 'auto', // 不检查结束行形式
  embeddedLanguageFormatting: 'auto', // 对引用代码进行格式化
};


# 创建 .prettierignore  忽略文件并添加以下内容：

# Ignore artifacts:
build
coverage
node_modules
.nuxt

# Ignore all HTML files:
*.html

# 使用格式化
npx prettier --write .
npx prettier --check . #检查 ，写入到 package 
```



* nuxt-icon

> icon: [icon](https://nuxt.com/modules/icon)

```shell
pnpm install --save-dev nuxt-icon
```

> Add it to the `modules` array in your `nuxt.config.ts`:

```js
import { defineNuxtConfig } from 'nuxt'

export default defineNuxtConfig({
  modules: ['nuxt-icon']
})
```

> 使用

```html
<Icon name="uil:github" color="black" />
```



* pinia/nuxt

```shell
pnpm i pinia -D
pnpm i @pinia/nuxt
```

> Add it to the `modules` array in your `nuxt.config.ts`:

```ts
   modules: ['@pinia/nuxt'],
```



* pinia-plugin-persistedstate

> 持久化 Pinia Store  [githubio](https://prazdevs.github.io/pinia-plugin-persistedstate/zh/frameworks/nuxt-3.html)

```shell
pnpm i -D @pinia-plugin-persistedstate/nuxt
```

> 将模块添加到 Nuxt 配置中 (`nuxt.config.ts`)：

```ts
export default defineNuxtConfig({
    pages:true,
    devtools: { enabled: true },
  modules: [
    '@pinia/nuxt',
    '@pinia-plugin-persistedstate/nuxt',
  ],
})
```

> 用法
>
> 创建 Store 时，将 `persist` 选项设置为 `true`。

```shell
import { defineStore } from 'pinia'

export const useStore = defineStore('main', {
  state: () => {
    return {
      someState: '你好 pinia',
    }
  },
  persist: true,
})
```



* axios

```shell
pnpm i -D axios
```











