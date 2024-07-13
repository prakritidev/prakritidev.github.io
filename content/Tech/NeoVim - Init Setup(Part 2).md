---
title: '"NeoVim - Init Setup(Part 2)"'
draft: 
tags:
  - neovim
  - installation
---
# Install Packer Manager (Or Lazy if you want)

```
git clone --depth 1 https://github.com/wbthomason/packer.nvim ~/.local/share/nvim/site/pack/packer/start/packer.nvim
```

#### Create dir for neovim

```
mkdir -p ~/.config/nvim
touch ~/.config/nvim/init.lua
mkdir -p ~/.config/nvim/lua

```

# init.lua (include basic settings and load the Lua configurations.)

```
-- init.lua [This file include basic settings and load the Lua configurations.]
require('settings')
require('keymappings')
require('plugins')
require('plugin_config')
```

execute command  ```<ESC>:wq```
This will write the text to the file and exit from neovim. 

Run command: ```nvim``` This will give you an error, this is because setting anf keymappings dont exist yet. 

# settings.lua

```
-- ~/.config/nvim/lua/settings.lua
-- Basic Neovim settings
vim.o.number = true
vim.o.relativenumber = true
vim.o.expandtab = true
vim.o.shiftwidth = 4
vim.o.tabstop = 4
vim.o.smartindent = true
vim.o.wrap = false
vim.o.swapfile = false
vim.o.backup = false
vim.o.undodir = os.getenv("HOME") .. "/.vim/undodir"
vim.o.undofile = true
vim.o.hlsearch = false
vim.o.incsearch = true
vim.o.termguicolors = true
vim.o.scrolloff = 8
vim.o.signcolumn = "yes"
vim.o.cmdheight = 2
```

# keymappings.lua

```
-- ~/.config/nvim/lua/keymappings.lua
-- Custom key mappings
vim.g.mapleader = " "
local map = vim.api.nvim_set_keymap
local opts = { noremap = true, silent = true }

-- Example mappings
map('n', '<leader>e', ':NvimTreeToggle<CR>', opts)
map('n', '<leader>ff', ':Telescope find_files<CR>', opts)
map('n', '<leader>fg', ':Telescope live_grep<CR>', opts)
```

# plugins.lua

```
-- ~/.config/nvim/lua/plugins.lua
-- Load packer.nvim
vim.cmd [[packadd packer.nvim]]

return require('packer').startup(function(use)
  -- Packer can manage itself
  use 'wbthomason/packer.nvim'
  
  -- UI Enhancements
  use 'morhetz/gruvbox'
  use 'hoob3rt/lualine.nvim'
  use 'kyazdani42/nvim-web-devicons'
  use 'akinsho/nvim-bufferline.lua'
  use 'nvim-telescope/telescope.nvim'
  use 'nvim-treesitter/nvim-treesitter'

  -- File Explorer
  use 'kyazdani42/nvim-tree.lua'

  -- Git Integration
  use 'lewis6991/gitsigns.nvim'

  -- LSP and Autocompletion
  use 'neovim/nvim-lspconfig'
  use 'hrsh7th/nvim-compe'
  
  -- Multi-line cursor support
  use 'mg979/vim-visual-multi'
  
  -- Your other plugins here
end)
```

# plugin_config.lua

```
-- ~/.config/nvim/lua/plugin_config.lua
-- Plugin-specific configurations

-- Treesitter configuration
require'nvim-treesitter.configs'.setup {
  ensure_installed = "maintained",
  highlight = {
    enable = true,
  },
}

-- Lualine configuration
require('lualine').setup {
  options = {
    theme = 'gruvbox'
  }
}

-- NvimTree configuration
require'nvim-tree'.setup {}

-- Gitsigns configuration
require'gitsigns'.setup {}

-- Telescope configuration
require('telescope').setup {}

-- LSP and completion configuration
local nvim_lsp = require('lspconfig')
local on_attach = function(client, bufnr)
  local buf_set_keymap = vim.api.nvim_buf_set_keymap
  local buf_set_option = vim.api.nvim_buf_set_option
  buf_set_option(bufnr, 'omnifunc', 'v:lua.vim.lsp.omnifunc')

  local opts = { noremap=true, silent=true }

  -- Key mappings
  buf_set_keymap(bufnr, 'n', 'gD', '<Cmd>lua vim.lsp.buf.declaration()<CR>', opts)
  buf_set_keymap(bufnr, 'n', 'gd', '<Cmd>lua vim.lsp.buf.definition()<CR>', opts)
  buf_set_keymap(bufnr, 'n', 'K', '<Cmd>lua vim.lsp.buf.hover()<CR>', opts)
  buf_set_keymap(bufnr, 'n', 'gi', '<cmd>lua vim.lsp.buf.implementation()<CR>', opts)
  buf_set_keymap(bufnr, 'n', '<C-k>', '<cmd>lua vim.lsp.buf.signature_help()<CR>', opts)
  buf_set_keymap(bufnr, 'n', '<space>wa', '<cmd>lua vim.lsp.buf.add_workspace_folder()<CR>', opts)
  buf_set_keymap(bufnr, 'n', '<space>wr', '<cmd>lua vim.lsp.buf.remove_workspace_folder()<CR>', opts)
  buf_set_keymap(bufnr, 'n', '<space>wl', '<cmd>lua print(vim.inspect(vim.lsp.buf.list_workspace_folders()))<CR>', opts)
  buf_set_keymap(bufnr, 'n', '<space>D', '<cmd>lua vim.lsp.buf.type_definition()<CR>', opts)
  buf_set_keymap(bufnr, 'n', '<space>rn', '<cmd>lua vim.lsp.buf.rename()<CR>', opts)
  buf_set_keymap(bufnr, 'n', 'gr', '<cmd>lua vim.lsp.buf.references()<CR>', opts)
  buf_set_keymap(bufnr, 'n', '<space>e', '<cmd>lua vim.lsp.diagnostic.show_line_diagnostics()<CR>', opts)
  buf_set_keymap(bufnr, 'n', '[d', '<cmd>lua vim.lsp.diagnostic.goto_prev()<CR>', opts)
  buf_set_keymap(bufnr, 'n', ']d', '<cmd>lua vim.lsp.diagnostic.goto_next()<CR>', opts)
  buf_set_keymap(bufnr, 'n', '<space>q', '<cmd>lua vim.lsp.diagnostic.set_loclist()<CR>', opts)
end

-- Enable the following language servers
local servers = { 'jdtls', 'gopls' }
for _, lsp in ipairs(servers) do
  nvim_lsp[lsp].setup {
    on_attach = on_attach,
  }
end

-- Nvim-compe configuration
require'compe'.setup {
  enabled = true;
  autocomplete = true;
  debug = false;
  min_length = 1;
  preselect = 'enable';
  throttle_time = 80;
  source_timeout = 200;
  resolve_timeout = 800;
  incomplete_delay = 400;
  max_abbr_width = 100;
  max_kind_width = 100;
  max_menu_width = 100;
  documentation = true;

  source = {
    path = true;
    buffer = true;
    calc = true;
    nvim_lsp = true;
    nvim_lua = true;
    vsnip = true;
  };
}
```

This is a basic setup for neovim. We'll add some more support in nevim and keep it light weight. 
1. We don't need file explorer, etc etc. 