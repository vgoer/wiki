<center>Biome格式化工具</center>





[toc]









## Biome格式化工具

> Biome 是一个用 Rust 编写的高性能代码格式化和 lint 工具，主要用于 JavaScript/TypeScript 项目。 [biome](https://github.com/biomejs/biome) [dev](https://biomejs.dev/)
>
> 其主要特点：

```shell
速度极快（比 Prettier 快 20-30 倍）
零配置，开箱即用
统一的代码风格
支持增量格式化
内置 Linter
```









### 1.使用

```shell
1. 创建vue3项目
npm init vite@latest vue3_project

2. 安装依赖
pnpm install 

3. 安装 biome
pnpm add --save-dev --save-exact @biomejs/biome

4. 创建默认配置文件
pnpm biome init

5. 使用
# format
pnpm biome format --write <files>

# lint
pnpm biome lint --write <files>

# 两着都使用
pnpm biome check --write <files>
```

> 完成啦，默认配置也可以的。





### 2. 格式化配置

> 默认配置 `biome.json`

```shell
# 基础默认格式配置

"formatter": {
  "enabled": true,                 // 启用格式化功能
  "formatWithErrors": false,       // 有错误时不进行格式化
  "ignore": [],                    // 忽略的文件列表为空
  "attributePosition": "auto",     // HTML/JSX 属性位置自动调整
  "indentStyle": "tab",           // 使用 tab 作为缩进
  "indentWidth": 2,               // 缩进宽度为 2
  "lineEnding": "lf",             // 使用 LF（Unix 风格）换行符
  "lineWidth": 80                 // 每行最大宽度为 80 字符
}

# js格式配置
"javascript": {
  "formatter": {
    "arrowParentheses": "always",    // 箭头函数参数总是使用括号，如: (x) => x
    "bracketSameLine": false,        // 结束标签不与最后一行内容在同一行
    "bracketSpacing": true,          // 对象字面量括号内添加空格，如: { foo: bar }
    "jsxQuoteStyle": "double",       // JSX 属性使用双引号
    "quoteProperties": "asNeeded",   // 对象属性只在必要时使用引号
    "semicolons": "always",          // 始终添加分号
    "trailingCommas": "all"          // 总是添加尾随逗号
  }
}

# json 格式配置

"json": {
  "formatter": {
    "trailingCommas": "none"         // JSON 文件中不使用尾随逗号
  }
}
```

> 写入配置

```shell
pnpm biome format --write ./src
```

