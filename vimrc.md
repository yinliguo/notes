```
set nocompatible
set number
set relativenumber
set timeoutlen=0
set tabstop=4
set shiftwidth=4 softtabstop=4 expandtab
set hls
filetype plugin on
runtime macros/matchit.vim

nnoremap<silent> <C-l> :<C-u>nohlsearch<CR><C-l>`

syntax on
set background=dark

" https://github.com/morhetz/gruvbox
let g:gruvbox_contrast_dark='dark'
colorscheme gruvbox

" airline
let g:airline#extensions#tabline#enabled = 1
let g:airline#extensions#tabline#buffer_nr_show = 1

" https://github.com/junegunn/vim-plug
call plug#begin('~/.vim/plugged')
Plug 'morhetz/gruvbox'
Plug 'vim-airline/vim-airline'
Plug 'https://tpope.io/vim/surround.git'
call plug#end()
```