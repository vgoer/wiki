<center>Markdown插件</center>









[toc]





### 1. Markdown插件

> (neo)vim 的 markdown 预览插件 [github](https://github.com/iamcco/markdown-preview.nvim)





### 2. 安装

> 参考这个文章 [markdown](https://www.lazyvim.org/extras/lang/markdown#markdown-previewnvim)

```shell
1. 新建plugins
nvim ~/.config/nvim/lua/plugins/markdown-preview.lua

2. 内容 想要的主题直接加就行

-- markdown 预览插件
return {
  "iamcco/markdown-preview.nvim",
  cmd = { "MarkdownPreviewToggle", "MarkdownPreview", "MarkdownPreviewStop" },
  ft = { "markdown" },
  build = function()
    vim.fn["mkdp#util#install"]()
  end,
}
```

> bug: [打不开](https://www.lazyvim.org/extras/lang/markdown#markdown-previewnvim)  [node找不到](https://github.com/iamcco/markdown-preview.nvim/issues/695)

```shell
" Start the preview
:MarkdownPreview

" Stop the preview"
:MarkdownPreviewStop
```

> `keymaps.lua`配置

```shell
-- 打开mardown
map({ "n", "i" }, "<C-m>", "<cmd>MarkdownPreview<cr>", { desc = "打开markdown", remap = true })
```

