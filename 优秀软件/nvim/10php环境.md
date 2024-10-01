<center>PHP环境</center>









[toc]





### 1. 搭建php环境

> 安装提示LSP插件





* 使用 `Intelephense` 作为 PHP 语言服务器   (二选一)

```shell
# 1.npm 安装

npm install -g intelephense

# 2. 添加对php支持
-- mason提示插件
return {
  {
    "williamboman/mason-lspconfig.nvim",
    opts = {
      ensure_installed = {
        --
        --  LSP
        --
        -- rust 提示插件
        "rust_analyzer",

        -- html 提示插件
        "html",

        -- css 提示插件
        "cssls",

        -- js、ts 提示插件
        "ts_ls",

        -- eslint 插件
        "eslint",

        -- php
        "intelephense",
      },
    },
    config = function() end,
  },
}
```

* 使用 `phpactor` 作为 PHP 语言服务器  (二选一)

```shell
# 1. composer 安装
composer global require phpactor/phpactor

# 2. 添加对php支持

        -- php
        "phpactor",
```





### 2. 其他插件

```lua
1. 新建plugins
nvim ~/.config/nvim/lua/plugins/php.lua

2. 内容 

-- 代码片段插件
return {
  -- PHP Syntax Highlighting and PHP Support
  {
    "stephpy/vim-php-cs-fixer",
    event = "BufWritePre",
    ft = { "php" },
    config = function()
      vim.cmd [[autocmd BufWritePre *.php execute ':call PhpCsFixerFixFile()']]
    end,
  },
  -- PHP Syntax
  {
    "StanAngeloff/php.vim",
    ft = { "php" },
  },
}
```

