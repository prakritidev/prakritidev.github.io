---
title: '"NeoVim - Build from source" (Part 1)'
draft: 
tags:
  - neovim
  - sourcebuild
  - installation
---
# Build NeoVim from source 

remove if you have installed it already

```
brew uninstall neovim
rm -rf ~/.config/nvim ~/.local/share/nvim ~/.cache/nvim
```

Install deps: 

```
brew install ninja libtool automake cmake pkg-config gettext curl
```

Clone Repo:

```
git clone https://github.com/neovim/neovim.git
cd neovim
```

Build steps:

```
make CMAKE_BUILD_TYPE=Release
sudo make install
```

Verify:
```
nvim -V1 -v

Output:
NVIM v0.11.0-dev-405+g8d8b8af2d
Build type: Release
LuaJIT 2.1.1720049189

   system vimrc file: "$VIM/sysinit.vim"
  fall-back for $VIM: "/usr/local/share/nvim"

Run :checkhealth for more info

```


