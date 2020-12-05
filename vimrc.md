linux下配置:<br>
```shell

" 不开启与vi的兼容模式
set nocompatible

" 编码格式设置, 设置tab为两个空格, 换行时自动与上一行对齐
set tabstop=2
set shiftwidth=2
set expandtab
set softtabstop=2
set autoindent

" 按下F9后再ctrl+shift+v复制文本不会打乱原有格式
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

```

windows下配置(gvim):<br>
```shell

" 不开启与vi的兼容模式
set nocompatible

" 设置文件编码格式
set encoding=utf-8
" 解决菜单乱码
source $VIMRUNTIME/delmenu.vim
source $VIMRUNTIME/menu.vim

" 去掉工具栏和菜单栏
set guioptions-=m
set guioptions-=T

" 关闭swap文件
set noswapfile
" 不生成 file~ 备份文件
set nobackup
" 不生成 file.un~ 文件
set noundofile

" 关闭bell提示音
set vb t_vb=
" 关闭屏闪
au GuiEnter * set t_vb=

" 设置打开窗口大小
set lines=50 columns=150

" 设置字体和大小
set guifont=Consolas:h12

" 配色方案(在/vim82/colors可以看到所有支持的配色方案)
colorscheme desert

" 打开语法高亮
syntax enable
" 开启文件类型侦测
syntax on

" 设置normal/visual/command/insert模式下光标样式为block
set guicursor=n-v-c-i:block
" 设置所有模式停止光标闪烁
set guicursor=a:blinkon0

" 编码格式设置, 设置tab为两个空格, 换行时自动与上一行对齐
set tabstop=2
" 设定 << 和 >> 命令移动时的宽度为 2
set shiftwidth=2
" 使用空格代替制表符
set expandtab
" 使得按退格键时可以一次删掉 2 个空格
set softtabstop=2
" 继承前一行的缩进方式, 特别适用于多行注释
set autoindent

" 按下F9后再ctrl+shift+v复制文本不会打乱原有格式
set pastetoggle=<F9>

" vsplit默认新窗口在右边打开
set splitright

" 高亮搜索的字符
set hlsearch
" 忽略搜索大小写
set ignorecase
" 打开显示查找匹配过程
set incsearch
set mouse=
" 在屏幕前后预留5行
set scrolloff=5

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

```

