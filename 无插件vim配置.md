" 不开启与vi的兼容模式
set nocompatible

" 编码格式设置, 设置tab为两个空格, 换行时自动与上一行对齐
set tabstop=2
set shiftwidth=2
set expandtab
set softtabstop=2
set autoindent

" 按下F9后再crtl+shift+v复制文本不会打乱原有格式
set pastetoggle=<F9>

" vsplit默认新窗口在右边打开
set splitright

" 高亮搜索的字符
set hlsearch
" 忽略搜索大小写
set ignorecase
set incsearch

" 在屏幕前后预留5行
set scrolloff=5

set encoding=utf-8

" 在insert模式下按crtl+h/j/k/l移动光标
inoremap <c-h> <left>
inoremap <c-j> <down>
inoremap <c-k> <up>
inoremap <c-l> <right>

" 底部预留两行始终显示文件名和行数信息
set laststatus=2

" 打开文件时, 回到上次退出的位置
if has("autocmd")
  au BufReadPost * if line("'\"") > 1 && line("'\"") <= line("$") | exe "normal! g'\"" | endif
endif