---
title: nvim
description: nvim编辑器
published: 1
date: 2023-06-09T10:10:18.945Z
tags: app, nvim
editor: markdown
dateCreated: 2023-04-19T15:29:43.316Z
---

<center>nvim</center>



[toc]



## Nvim

>nvim 编辑器 [nvim](https://github.com/neovim/neovim) [bili](https://www.bilibili.com/video/BV1Td4y1578E/?spm_id_from=333.1007.top_right_bar_window_history.content.click&vd_source=86b829d6caeffc65037786a84ec2cb17)
>
>配置： [zhihu](https://zhuanlan.zhihu.com/p/382092667) [github](https://github.com/nshen/learn-neovim-lua)

```shell
# win11 
C:\Users\用户名\AppData\Local # 用户名换成你的， 配置文件clone到 nvim下。就可以了
```



### 1. 安装

```shell
# arch 0.8以上
sudo pacman -S neovim 
# 其他发行版下载 
https://github.com/neovim/neovim/releases
```



### 2.配置

```shell
# 文件
mkdir -p ~/.config/nvim
# 核心文件
touch init.lua

# 模块化配置方案 不然和vim一样配置了
mkdir lua
mkdir lua/core  # 核心文件夹
touch lua/core/options.lua # 内容如下

# init.lua 引入配置 
-- options
require("core.options")
```

```lua
-- 选项配置
--

local opt = vim.opt

-- 行号
opt.number = true
opt.relativenumber = true

-- 缩进
opt.tabstop = 2
opt.shiftwidth = 2
opt.expandtab = true
opt.autoindent = true

-- 防止包裹
opt.wrap = false

-- 光标行
opt.cursorline = true

-- 启用鼠标
opt.mouse:append("a")

-- 系统剪切板
opt.clipboard:append("unnamedplus")


-- 默认新窗口右和下
opt.splitright = true
opt.splitbelow = true


-- 搜素区分大小写
opt.ignorecase = true
opt.smartcase = true

-- 外观
opt.termguicolors = true
opt.signcolumn = "yes"
```

> 改键

```shell
touch lua/core/keymaps.lua # 内容如下
# init.lua 引入配置 
require("core.keympas")
```

```lua
-- 改建

-- 主键设置为空格
vim.g.mapleader = " "

-- 设置变量
local keymap = vim.keymap


-- 修改退出 esc
keymap.set("i","jk","<ESC>")


-- 移动单行和多行
keymap.set("v","J",":m '>+1<CR>gv=gv")
keymap.set("v","K",":m '<-2<CR>gv=gv")


-- 窗口
keymap.set("n","<leader>sv","<C-w>v") -- 水平
keymap.set("n","<leader>sh","<C-w>s") -- 水平


-- 取消高亮
keymap.set("n","<leader>nh",":nohl<CR>")
```



### 3. 插件

> packer [packer](https://github.com/wbthomason/packer.nvim)  插件管理

```shell
cd ~/.config/nvim
mkdir -p lua/plugins
touch lua/plugins/plugins-setup.lua # 管理插件配置

-- packer  
require("plugins.plugins-setup")
```

```lua
-- from  https://github.com/wbthomason/packer.nvim#bootstrapping
local ensure_packer = function()
  local fn = vim.fn
  local install_path = fn.stdpath('data')..'/site/pack/packer/start/packer.nvim'
  if fn.empty(fn.glob(install_path)) > 0 then
    fn.system({'git', 'clone', '--depth', '1', 'https://github.com/wbthomason/packer.nvim', install_path})
    vim.cmd [[packadd packer.nvim]]
    return true
  end
  return false
end

local packer_bootstrap = ensure_packer()

return require('packer').startup(function(use)
  use 'wbthomason/packer.nvim'
  -- My plugins here
  -- use 'foo1/bar1.nvim'
  -- use 'foo2/bar2.nvim'
  use 'folke/tokyonight.nvim' -- 主题
  use {
    'nvim-lualine/lualine.nvim',  -- 状态栏
    requires = { 'kyazdani42/nvim-web-devicons', opt = true }  -- 状态栏图标
  }
        
              
  -- Automatically set up your configuration after cloning packer.nvim
  -- Put this at the end after all plugins
  if packer_bootstrap then
    require('packer').sync()
  end
end)
```

```shell
-- 主题  lua/core/options.lua
-- Lua /moon/Storm/Night/Day
vim.cmd[[colorscheme tokyonight-moon]]
```

> 状态栏：[lualine](https://github.com/nvim-lualine/lualine.nvim)

```shell
use {
  'nvim-lualine/lualine.nvim',
  requires = { 'kyazdani42/nvim-web-devicons', opt = true }
}
# 配置文件
nvim lua/core/plugins/lualine.lua
require('lualine').setup({
   options = {
      theme = 'tokyonight'
    }
})
```

> 文档树 [tree](https://github.com/nvim-tree/nvim-tree.lua)

```shell
use {
  'nvim-tree/nvim-tree.lua',
  requires = {
    'nvim-tree/nvim-web-devicons', -- optional, for file icons
  },
  tag = 'nightly' -- optional, updated every week. (see issue #1193)
}
# 配置文件
nvim lua/core/plugins/nvim-tree.lua
-- 默认不开起tree
vim.g.loaded_netrw = 1
vim.g.loaded_netrwPlugin = 1
require("nvim-tree").setup()

# 打开tree 键
nvim lua/core/keymaps.lua
-- nvim-tree
keymap.set("n","<leader>e",":NvimTreeToggle<CR>")
```

> nvim-tmux [tmux](https://github.com/christoomey/vim-tmux-navigator)

```shell
  use "christoomey/vim-tmux-navigator" -- 用ctl-hjkl来定位窗口
```

> 高亮代码 [treesitter](https://github.com/nvim-treesitter/nvim-treesitter) 

```shell
  use "nvim-treesitter/nvim-treesitter" -- 语法高亮
  use "p00f/nvim-ts-rainbow" -- 配合treesitter，不同括号颜色区分
  
# 配置
nvim lua/core/plugins/treesitter.lua
require'nvim-treesitter.configs'.setup {
  -- 添加不同语言
  ensure_installed = { "vim", "help", "bash", "c", "cpp", "javascript", "json", "lua", "python", "typescript", "tsx", "css", "rust", "markdown", "markdown_inline" }, -- one of "all" or a list of languages

  highlight = { enable = true },
  indent = { enable = true },

  -- 不同括号颜色区分
  rainbow = {
    enable = true,
    extended_mode = true,
    max_file_lines = nil,
  }
}
```

> lsp代码提示 [mason](https://github.com/williamboman/mason.nvim)

```shell
  use {
    "williamboman/mason.nvim",
    "williamboman/mason-lspconfig.nvim",  -- 这个相当于mason.nvim和lspconfig的桥梁
    "neovim/nvim-lspconfig"
  }
  -- 自动补全
  use "hrsh7th/nvim-cmp"
  use "hrsh7th/cmp-nvim-lsp"
  use "L3MON4D3/LuaSnip" -- snippets引擎，不装这个自动补全会出问题
  use "saadparwaiz1/cmp_luasnip"
  use "rafamadriz/friendly-snippets"
  use "hrsh7th/cmp-path" -- 文件路径
# 配置
nvim lua/core/plugins/lsp.lua
require("mason").setup({
  ui = {
      icons = {
          package_installed = "✓",
          package_pending = "➜",
          package_uninstalled = "✗"
      }
  }
})

require("mason-lspconfig").setup({
  -- 确保安装，根据需要填写
  ensure_installed = {
    "lua_ls",
  },
})

local capabilities = require('cmp_nvim_lsp').default_capabilities()

require("lspconfig").lua_ls.setup {
  capabilities = capabilities,
}

# 配置
nvim lua/core/plugins/cmp.lua
local cmp_status_ok, cmp = pcall(require, "cmp")
if not cmp_status_ok then
  return
end

local snip_status_ok, luasnip = pcall(require, "luasnip")
if not snip_status_ok then
  return
end

require("luasnip.loaders.from_vscode").lazy_load()

-- 下面会用到这个函数
local check_backspace = function()
  local col = vim.fn.col "." - 1
  return col == 0 or vim.fn.getline("."):sub(col, col):match "%s"
end


cmp.setup({
  snippet = {
    expand = function(args)
      require('luasnip').lsp_expand(args.body)
    end,
  },
  mapping = cmp.mapping.preset.insert({
    ['<C-b>'] = cmp.mapping.scroll_docs(-4),
    ['<C-f>'] = cmp.mapping.scroll_docs(4),
    ['<C-e>'] = cmp.mapping.abort(),  -- 取消补全，esc也可以退出
    ['<CR>'] = cmp.mapping.confirm({ select = true }),

    ["<Tab>"] = cmp.mapping(function(fallback)
      if cmp.visible() then
        cmp.select_next_item()
      elseif luasnip.expandable() then
        luasnip.expand()
      elseif luasnip.expand_or_jumpable() then
        luasnip.expand_or_jump()
      elseif check_backspace() then
        fallback()
      else
        fallback()
      end
    end, {
      "i",
      "s",
    }),

    ["<S-Tab>"] = cmp.mapping(function(fallback)
      if cmp.visible() then
        cmp.select_prev_item()
      elseif luasnip.jumpable(-1) then
        luasnip.jump(-1)
      else
        fallback()
      end
    end, {
      "i",
      "s",
    }),
  }),

  -- 这里重要
  sources = cmp.config.sources({
    { name = 'nvim_lsp' },
    { name = 'luasnip' },
    { name = 'path' },
  }, {
    { name = 'buffer' },
  })
})

# 安装其他语言提示
:Mason

```





### 5. neovim

> nvim的gui,rust写的。带有gup加速
>
> [github](https://github.com/neovide/neovide) 
