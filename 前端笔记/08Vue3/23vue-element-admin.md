---
title: 23vue-element-admin
description: 
published: 1
date: 2023-06-09T10:14:59.812Z
tags: 
editor: markdown
dateCreated: 2023-05-11T10:30:52.008Z
---

<center>vue-element</center>





[toc]



### vue-element-admin

> 后台前端解决方案。 [git](https://github.com/PanJiaChen/vue-element-admin)
>
> 文档: [doc](https://panjiachen.gitee.io/vue-element-admin-site/zh/)





#### 1. 安装

> 很多配置官方文档都有说明

```shell
git clone https://github.com/PanJiaChen/vue-element-admin.git

# npm  i 报错 code:128 
1. 删除 node_modules 和 package-lock.json
2. npm cache clean --force 
3. git config --global http.sslverify "false"   # 关闭 ssl
4. npm i

# 查看依赖
npm ls
```

> `tui-editor插件`已经没使用了  [blog](https://blog.csdn.net/qq_43271844/article/details/125865607)



### 2. ESLint 

> 代码规范都是很重要的 [doc](https://panjiachen.gitee.io/vue-element-admin-site/zh/guide/advanced/eslint.html#%E9%85%8D%E7%BD%AE%E9%A1%B9)

```js
// 四个空格   .eslintrc.js 中，找到 indent，然后修改为 4 即可。

'indent': [2, 4, {
  'SwitchCase': 1
}],
    
npm run lint -- --fix
```

> eslint安装使用

```js
npm i -D eslint

npx eslint --init
// 并选择您喜欢的格式（YAML、JSON 或 JS）。完成后，将在项目根目录中生成一个名为 .eslintrc.* 的文件。

// 使用
npx eslint yourfile.js

{
  "scripts": {
    "lint": "eslint ."
  }
}

// 安装插件 ESLint插件
```

> ==vue和react==的配置 [blog](https://blog.csdn.net/GZZ__z/article/details/122006474)
>
> 下载这个插件：` Prettier ESLint`













