<center>AstronVim</center>









[toc]







## AstronVim

> AstronVim 的配置。
>
> 安装：看我的多个nvim内里哈。









### 1. 社区插件

> 社区插件： [github](https://github.com/AstroNvim/astrocommunity)
>
> 直接复制，然后就使用了。

```lua
-- ccc.lua

return {
  "uga-rosa/ccc.nvim",
  event = { "User AstroFile", "InsertEnter" },
  cmd = { "CccPick", "CccConvert", "CccHighlighterEnable", "CccHighlighterDisable", "CccHighlighterToggle" },
  specs = {
    { "NvChad/nvim-colorizer.lua", optional = true, enabled = false },
  },
  dependencies = {
    {
      "AstroNvim/astrocore",
      opts = {
        mappings = {
          n = {
            ["<Leader>uC"] = { "<Cmd>CccHighlighterToggle<CR>", desc = "Toggle colorizer" },
            ["<Leader>zc"] = { "<Cmd>CccConvert<CR>", desc = "Convert color" },
            ["<Leader>zp"] = { "<Cmd>CccPick<CR>", desc = "Pick Color" },
          },
        },
      },
    },
  },
  opts = {
    highlighter = {
      auto_enable = true,
      lsp = true,
    },
  },
  config = function(_, opts)
    require("ccc").setup(opts)
    if opts.highlighter and opts.highlighter.auto_enable then vim.cmd.CccHighlighterEnable() end
  end,
}
```

