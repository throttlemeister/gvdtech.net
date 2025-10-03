---
title: "Neovim"
date: 2025-10-03T14:58:56+02:00
draft: false
description: "Some bits and pieces on my Neovim configuration"
tags: [ computer, linux, Neovim ]
categories: [ linux, neovim, computers ]
---
# Some bits on my Neovim configuration and used plugins

## Introduction

Neovim has been my go-to editor for a while now, and over time I have collected some plugins and made some configuration changes that help me. Nothing crazy or spectacular, but works for me.

## Colorscheme configuration

First off, as you may have guessed if you have been here before, I use Catpuccin (Frappe flavor) as my default colorscheme for just about everything and everywhere if I can. Same for Neovim, however I have been experiencing some issues with Catpuccin and bufferline (one of the default lazyvim plugins) after upgrades. Figured out, manual config change fixes this.

File:
    ~/.config/nvim/lua/config/colorscheme.lua

Source:

```lua
return {
  "catppuccin/nvim",
  lazy = true,
  name = "catppuccin",
  opts = {
    integrations = {
      aerial = true,
      alpha = true,
      cmp = true,
      dashboard = true,
      flash = true,
      fzf = true,
      grug_far = true,
      gitsigns = true,
      headlines = true,
      illuminate = true,
      indent_blankline = { enabled = true },
      leap = true,
      lsp_trouble = true,
      mason = true,
      markdown = true,
      mini = true,
      native_lsp = {
        enabled = true,
        underlines = {
          errors = { "undercurl" },
          hints = { "undercurl" },
          warnings = { "undercurl" },
          information = { "undercurl" },
        },
      },
      navic = { enabled = true, custom_bg = "lualine" },
      neotest = true,
      neotree = true,
      noice = true,
      notify = true,
      semantic_tokens = true,
      snacks = true,
      telescope = true,
      treesitter = true,
      treesitter_context = true,
      which_key = true,
    },
  },
  specs = {
    {
      "akinsho/bufferline.nvim",
      optional = true,
      opts = function(_, opts)
        if (vim.g.colors_name or ""):find("catppuccin") then
          opts.highlights = require("catppuccin.groups.integrations.bufferline").get_theme()
        end
      end,
    },
  },
}
```

The fancy bit is actually in the last section, and how it calls bufferline.

## Plugins

I use a few plugins. Four to be exact, at the moment.

- render-markdown
- mkdnflow
- contextindent
- markdown-lint

The last one is actually a standard plugin inside lazyvim, but I have a modified configuration to 'fix' an issue with linter errors in Markdown when having more than 80 characters in a single line. I know.

### Render-Markdown

File:
    ~/.config/nvim/lua/plugins/render-markdown.lua

Source:

```lua
return {
  "MeanderingProgrammer/render-markdown.nvim",
  dependencies = { "mini.icons" },
  --- @module 'render-markdown'
  --- @type render.md.UserConfig
  opts = {
    ensure_installed = { "markdownlint-cli2" },
    code = {
      sign = true,
      width = "block",
      right_pad = 1,
    },
    heading = {
      enabled = true,
      render_modes = false,
      atx = true,
      setext = true,
      sign = true,
      icons = { "󰲡 ", "󰲣 ", "󰲥 ", "󰲧 ", "󰲩 ", "󰲫 " },
      position = "overlay",
      signs = { "󰫎 " },
      width = "full",
      left_margin = 0,
      left_pad = 0,
      right_pad = 0,
      min_width = 0,
      border = false,
      border_virtual = false,
      border_prefix = false,
      above = "▄",
      below = "▀",
      backgrounds = {
        "RenderMarkdownH1Bg",
        "RenderMarkdownH2Bg",
        "RenderMarkdownH3Bg",
        "RenderMarkdownH4Bg",
        "RenderMarkdownH5Bg",
        "RenderMarkdownH6Bg",
      },
      foregrounds = {
        "RenderMarkdownH1",
        "RenderMarkdownH2",
        "RenderMarkdownH3",
        "RenderMarkdownH4",
        "RenderMarkdownH5",
        "RenderMarkdownH6",
      },
      custom = {},
    },
    checkbox = {
      enabled = true,
      render_modes = false,
      bullet = false,
      right_pad = 1,
      unchecked = {
        icon = "󰄱 ",
        highlight = "RenderMarkdownUnchecked",
        scope_highlight = nil,
      },
      checked = {
        icon = "󰱒 ",
        highlight = "RenderMarkdownChecked",
        scope_highlight = "@markup.strikethrough",
      },
      custom = {
        todo = { raw = "[-]", rendered = "󰥔 ", highlight = "RenderMarkdownTodo", scope_highlight = nil },
      },
    },
    link = {
      enabled = true,
      render_modes = false,
      footnote = {
        enabled = true,
        superscript = true,
        prefix = "",
        suffix = "",
      },
      image = "󰥶 ",
      email = "󰀓 ",
      hyperlink = "󰌹 ",
      highlight = "RenderMarkdownLink",
      wiki = {
        icon = "󱗖 ",
        body = function()
          return nil
        end,
        highlight = "RenderMarkdownWikiLink",
        scope_highlight = nil,
      },
      custom = {
        web = { pattern = "^http", icon = "󰖟 " },
        discord = { pattern = "discord%.com", icon = "󰙯 " },
        github = { pattern = "github%.com", icon = "󰊤 " },
        gitlab = { pattern = "gitlab%.com", icon = "󰮠 " },
        google = { pattern = "google%.com", icon = "󰊭 " },
        neovim = { pattern = "neovim%.io", icon = " " },
        reddit = { pattern = "reddit%.com", icon = "󰑍 " },
        stackoverflow = { pattern = "stackoverflow%.com", icon = "󰓌 " },
        wikipedia = { pattern = "wikipedia%.org", icon = "󰖬 " },
        youtube = { pattern = "youtube%.com", icon = "󰗃 " },
      },
    },
    indent = {
      enabled = false,
      render_modes = false,
      per_level = 2,
      skip_level = 1,
      skip_heading = false,
      icon = "▎",
      priority = 0,
      highlight = "RenderMarkdownIndent",
    },
    pipe_table = {
      enabled = true,
      render_modes = false,
      preset = "round",
      cell = "padded",
      cell_offset = function()
        return 0
      end,
      padding = 1,
      min_width = 0,
      border = {
        "┌",
        "┬",
        "┐",
        "├",
        "┼",
        "┤",
        "└",
        "┴",
        "┘",
        "│",
        "─",
      },
      border_enabled = true,
      border_virtual = false,
      alignment_indicator = "━",
      head = "RenderMarkdownTableHead",
      row = "RenderMarkdownTableRow",
      filler = "RenderMarkdownTableFill",
      style = "full",
    },
    render_modes = { "n", "c", "t" },
  },
  ft = { "markdown", "norg", "rmd", "org", "codecompanion" },
  config = function(_, opts)
    require("render-markdown").setup(opts)
    Snacks.toggle({
      name = "Render Markdown",
      get = function()
        return require("render-markdown.state").enabled
      end,
      set = function(enabled)
        local m = require("render-markdown")
        if enabled then
          m.enable()
        else
          m.disable()
        end
      end,
    }):map("<leader>um")
  end,
}
```

Lots of configuration options here that can be set.

What's it do, you may ask? It renders markdown files inside neovim and while you are typing. Very convenient and gives you a pretty good idea of what a page is going to look like.

### mkdnflow

This plugin helps me to quickly organize and work with creating notes or related and linked files from Neovim. For now, I have not customized its configuration so it is very basic.

File:
    ~/.config/nvim/lua/plugins/mkdwnflow.lua

Source:

```lua
return {
  "jakewvincent/mkdnflow.nvim",
  config = function()
    require("mkdnflow").setup({
      -- Config goes here; leave blank for defaults
    })
  end,
}
```

### contextindent

Same as mkdnflow basically. This plugin makes sure indentation of code blocks in Markdown files are done properly according to the language of the code block.

File:
    ~/.config/nvim/lua/plugins/indent.lua

Source:

```lua
return {
 "wurli/contextindent.nvim",
 -- This is the only config option; you can use it to restrict the files
 -- which this plugin will affect (see :help autocommand-pattern).
 opts = { pattern = "*" },
 dependencies = { "nvim-treesitter/nvim-treesitter" },
}
```

### nvim-lint

And the last one, which is basically a fix, as I said. It points to a dot-file in my home directory to suppress a specific linter error that is not applicable in Markdown.

File:
    ~/.config/nvim/lua/plugins/markdown-lint.lua
    ~/.markdownlint-cli2.yaml

Source:

```lua
return {
  {
    "mfussenegger/nvim-lint",
    opts = {
      linters = {
        ["markdownlint-cli2"] = {
          args = { "--config", vim.fn.expand("$HOME/.markdownlint-cli2.yaml"), "--" },
        },
      },
    },
  },
}
```

And the error to suppress:

```yaml
config:
  MD013: false
```

## Conclusion

Neovim is a tool of endless oppertunity. I haven't even scratched the surface of what is possible and already it does more for me quicker and easier than vscode did before. Yes it does getting used to, especially if you are used to operating software with your mouse as your core navigation tool but it is learned quickly if you want.
