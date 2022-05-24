- ## 插件设置
	- ```vim
	  call plug#begin("~/.vim/plugged")
	   " Plugin Section
	   Plug 'ryanoasis/vim-devicons'
	   Plug 'preservim/nerdcommenter'
	   Plug 'mhinz/vim-startify'
	   Plug 'kyazdani42/nvim-web-devicons' 
	   Plug 'kyazdani42/nvim-tree.lua'
	   Plug 'neoclide/coc.nvim', {'branch': 'release'}
	  call plug#end()
	  ```
- ## 插件命令
	- ```text
	  PlugInstall
	  PlugUpdate
	  PlugClean
	  PlugStatus
	  PlugDiff
	  PlugSnapshot
	  ```