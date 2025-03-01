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



