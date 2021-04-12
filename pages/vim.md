---
layout: page
title: "VIM"
subheadline: "VIM"
meta_title: "VIM"
subheadline: "VIM"
teaser: ""
permalink: "/vim/"
---
用了很多的编辑器，还是vim顺手。

```shell
syntax on
set ts=4
set expandtab

set nocompatible 
filetype off 


let g:Tlist_Ctags_Cmd='/usr/local/Cellar/ctags/5.8_1/bin/ctags'

let g:tagbar_ctags_bin = 'ctags' 
let g:tagbar_autofocus = 1 
let Tlist_Show_One_File = 1 
let Tlist_Exit_OnlyWindow = 1 
let Tlist_Use_Right_Window = 1


let g:winManagerWindowLayout='FileExplorer'
nmap wm :WMToggle<cr>


nmap <silent> <C-Up> :wincmd k<CR>
nmap <silent> <C-Down> :wincmd j<CR>
nmap <silent> <C-Left> :wincmd h<CR>
nmap <silent> <C-Right> :wincmd l<CR>


map <C-n> :NERDTreeToggle<CR> 
map <F3> :TagbarToggle<CR>


let g:mkdp_path_to_chrome="open -a Google\\ Chrome"
let g:mkdp_auto_close=0
nmap <F7> <Plug>MarkdownPreview
nmap <F8> <Plug>StopMarkdownPreview



let g:ackprg = 'ag --vimgrep'


set mouse=a  

set rtp+=~/.vim/bundle/Vundle.vim 
call vundle#begin() 
Plugin 'https://github.com/kien/ctrlp.vim.git' 
Plugin 'https://github.com/scrooloose/nerdtree.git' 
Plugin 'https://github.com/aperezdc/vim-template.git' 
Plugin 'vim-scripts/winmanager'
Plugin 'iamcco/markdown-preview.vim'


Plugin 'godlygeek/tabular'
let g:detorte_theme_mode = 'light'

let g:tagbar_type_markdown = {
    \ 'ctagstype': 'markdown',
    \ 'ctagsbin' : '/Users/shengyang/.vim/markdown2ctags/markdown2ctags.py',
    \ 'ctagsargs' : '-f - --sort=yes --sro=»',
    \ 'kinds' : [
        \ 's:sections',
        \ 'i:images'
    \ ],
    \ 'sro' : '»',
    \ 'kind2scope' : {
        \ 's' : 'section',
    \ },
    \ 'sort': 0,
\ }



Bundle 'majutsushi/tagbar'
Bundle 'fholgado/minibufexpl.vim'
Bundle 'gabrielelana/vim-markdown'




Plugin 'itchyny/lightline.vim'
Plugin 'yuttie/hydrangea-vim'
"call dein#add('yuttie/hydrangea-vim')
let g:lightline = {
      \ 'colorscheme': 'hydrangea',
      \ 'component': {
      \   'readonly': '%{&readonly?"":""}',
      \ },
      \ 'separator':    { 'left': '', 'right': '' },
      \ 'subseparator': { 'left': '', 'right': '' },
      \ }
set laststatus=2



colorscheme molokai
let g:molokai_original = 1



call vundle#end() 
filetype plugin indent on

```

[markdown2ctgs](https://github.com/jszakmeister/markdown2ctags)

```shell
export TERM=xterm-256color
```

配置装态栏的颜色等信息

```
let g:lightline = {
      \ 'component_function': {
      \   'filename': 'LightlineFilename',
      \ },
      \ }

function! LightlineFilename()
  return &filetype ==# 'vimfiler' ? vimfiler#get_status_string() :
        \ &filetype ==# 'unite' ? unite#get_status_string() :
        \ &filetype ==# 'vimshell' ? vimshell#get_status_string() :
        \ expand('%:t') !=# '' ? expand('%:t') : '[No Name]'
endfunction

let g:unite_force_overwrite_statusline = 0
let g:vimfiler_force_overwrite_statusline = 0
let g:vimshell_force_overwrite_statusline = 0
```

```
colorscheme molokai
colorscheme hydrangea 
```

最后还是选择了hydrangea这个配色方法。


```
``VIM GIT插件
Plugin 'tpope/vim-fugitive'
```


### QQ群：781666521

 
