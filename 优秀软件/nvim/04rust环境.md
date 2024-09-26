<center>rust环境</center>









[toc]







### 1. Rust环境

> lazyvim 搭建rust环境

```shell
cargo new hello_rust


```





### 2. mason插件

> mason插件永久化。
>
> 约定大于配置，所以我们配置好文件，下此就不用在一个一个mason插件的找了

```shell
1. 新建plugins
nvim ~/.config/nvim/lua/plugins/mason.lua

2. 内容 想要的 插件 直接加就行

return {
  {
    "williamboman/mason-lspconfig.nvim",
    opts = {
      ensure_installed = {
        "rust_analyzer",
      },
    },
    config = function() end,
  },
}
```

