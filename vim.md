## vim
```
set number
set relativenumber
set cursorline
set timeoutlen=0
set tabstop=4
set shiftwidth=4

syntax on
set background=dark
" https://github.com/morhetz/gruvbox
let g:gruvbox_contrast_dark='dark'
color gruvbox

" airline
let g:airline#extensions#tabline#enabled = 1
let g:airline#extensions#tabline#buffer_nr_show = 1

" fzf
nnoremap <silent> <C-p> :Files<CR>

" https://github.com/junegunn/vim-plug
call plug#begin('~/.vim/plugged')
Plug 'vim-airline/vim-airline'
Plug 'scrooloose/nerdtree'
" https://github.com/junegunn/fzf#using-git
Plug '~/.fzf'
Plug 'junegunn/fzf.vim'
call plug#end()
```
