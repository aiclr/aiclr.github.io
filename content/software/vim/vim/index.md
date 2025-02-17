+++
title = 'Vim'
date = '2024-05-23'
author = 'aiclr'
categories = ['vim']
tags = ['vim']
Summary = 'Vim is a terminal text editor. It is an extended version of vi with additional features, including syntax highlighting, a comprehensive help system, native scripting (Vim script), a visual mode for text selection, comparison of files (vimdiff(1)), and tools with restricted capabilities such as rview(1) and rvim(1).'
+++

- [help](../user_manual/getting_started/usr_02#028-finding-help)

## MODE

```text
:set showmode
```

- normal mode
- insert mode
- visual mode

**normal mode** `i` -> **insert mode** `<Esc>` -> **normal mode**
**normal mode** `v` -> **visual mode** `<Esc>` -> **normal mode**

|editing commands||||
|---|---|---|---|
|a|append 拼接到光标位置|i|insert 插入到光标位置|
|A|光标行尾拼接|I|光标行首插入|
|o|光标下方新增一行|O|光标上方新增一行|
|s|删除光标所在字符|S|删除光标所在行|

## Getting out

- `ZZ` command writes the file and exits
- `:wq` write and quit
- `:w` write
- `:q` quit
- `:q!` fource quit without write

## Moving around

|moving||||
|:---|:---|:---|:---|
|`w`|to move the cursor forward to the start of the next word|`b`|to move the cursor backward to the start of the previous word|
|`e`|to move the cursor forward to the end of the next word|`ge`|to move the cursor backward to the end of the previous word|
|`0`|to move the cursor to the very first character of the line|`$`|to move the cursor to the end of a line|
|`^`|to move the cursor to the first non-blank character of the line|
||one of the most useful movement commands||single-character search command|
|`fx`|searches forward in the line for the single character x|`Fx`|the backward version of `fx` command|
|`tx`|works like the `fx` command, except it stops one character before the searched character.|`Tx`| the backward version of `tx` command|
|`%`|括号匹配跳转|
|***G***|跳转到末行|***nG***|跳转到n行|
|***gg***|跳转到首行|
|***H***|移动光标到当前页的第一行|
|***M***|移动光标到当前页的中间行|
|***L***|移动光标到当前页的最后一行|
| * |移动光标到上一个insert位置|

```text
        This is a line with example text
          x-->-->->----------------->
             w  w w    3w

        This is a line with example text
        <----<--<-<---------<--x
           b   b b    2b      b

        This is a line with example text
           <----<----x---->------------>
           2ge   ge     e       2e

                  ^
             <-----------x
        .....This is a line with example text
        <----------------x   x-------------->
                0                  $

        To err is human.  To really foul up you need a computer.
        ---------->--------------->
            fh           fy

        To err is human.  To really foul up you need a computer.
                  --------------------->
                           3fl

        To err is human.  To really foul up you need a computer.
                  <---------------------
                            Fh

        To err is human.  To really foul up you need a computer.
                   <------------  ------------->
                        Th              tn

                            %
                         <----->
                if (a == (b * c) / d)
                   <---------------->
                            %

            |   first line of a file   ^
            |   text text text text    |
            |   text text text text    |  gg
        7G  |   text text text text    |
            |   text text text text
            |   text text text text
            V   text text text text    |
                text text text text    |  G
                text text text text    |
                last line of a file    V

                        +---------------------------+
                H -->   | text sample text          |
                        | sample text               |
                        | text sample text          |
                        | sample text               |
                M -->   | text sample text          |
                        | sample text               |
                        | text sample text          |
                        | sample text               |
                L -->   | text sample text          |
                        +---------------------------+
```


|vim||||
|:---|:---|:---|:---|
|按边界*操作|
|yi*|复制边界内内容|yi"|复制双引号内的内容|
|ya*|复制边界以及内容|ya{|复制双引号及其内的内容|
|di*|删除边界内内容|di"|删除双引号内的内容|
|da*|删除边界以及内容|da{|删除双引号及其内的内容|
|ci*|删除边界内内容，并切换为i模式|ci"|-|
|ca*|删除边界以及内容，并切换为i模式|ca{|-|
|-|
|copy & cut|
|ggyG|复制全部|gg"+yG|复制全部到系统粘贴板|
|:7,10y|复制[7,10]行|不支持指定粘贴板|
|:7,10 copy 12 |相当于先复制[7,10]行，然后在第12行按p|:7,10 co 12 |相当于先复制[7,10]行，然后在第12行按p|
|:7,10d|剪切[7,10]行|不支持指定粘贴板|
|:7,10 move 12 |相当于先剪切[7,10]行，然后在第12行按p|:7,10 m 12 |相当于先剪切[7,10]行，然后在第12行按p|
|yy|复制行|yy、y1y|复制光标所在行|
|yw|复制单词|yw、y1w|复制光标及其后的1个单词|
|dd|剪切行|dd、d1d|剪切光标所在行|
|dw|剪切单词|dw、d1w|剪切光标及其后的1个单词|
|p|在当前行下方粘贴内容|P|在当前行上一行粘贴内容|
|-|
|***:reg***|查看所有粘贴板中内容|
|***"+y***|将内容复制到系统粘贴板|
|***"+p***|粘贴系统粘贴板中的内容|
|-|
|***:s/old/new***|将当前行第一个***old***替换成***new***|
|***:s/old/new/g***|当前行所有old替换成new|
|***:%s/old/new***|所有行第一个***old***替换成***new***|
|***:%s/old/new/g***|所有old替换成new|
|-|
|undo & redo|
|***u***|撤销一步|
|***ctrl r***|取消撤销|
|-|
|visual model|
|v|切换到visual model|
|ggvG|全选|
|vjjd|v切换到visual model，jj选择从光标所在行开始的往下两行，d剪切|
|vjjy|v切换到visual model，jj选择从光标所在行开始的往下两行，y复制|
|-|
|visual block mode|
|`Ctrl v`|切换到visual block model|
|y|block yink|将选择的块，复制到粘贴板|
|p|block paste|粘贴粘贴板中的内容|
|x|block delete|删除选择的块|
|c|block insert|输入后 使用`ESC`完成insert|
|-|
|split|
|***:sp***|水平分割窗口|***:vsp***|垂直分割窗口|
|***Ctrl+w+s***|水平分割窗口|***Ctrl+w+v***|垂直分割窗口|
|***:sp xxx.txt***|水平分割窗口并在新窗口打开xxx.txt|***:vsp xxx.txt***|垂直分割窗口并在新窗口打开xxx.txt|
|-|
|***vim -On***|***-O***垂直分割、***-n***分割窗口数|***vim -on***|***-o***水平分割、***-n***分割窗口数|
|***vim -O2 file1 file2***| 垂直分割窗口打开file1 file2|***vim -o2 file1 file2***| 水平分割窗口打开file1 file2|
|-|
|***:only***|仅保留当前分屏|***:hide***|关闭当前分屏|
|***Ctrl w c***|关闭当前窗口|***Ctrl w q***|关闭当前窗口，如果只剩最后一个窗口，则退出vim|
|-|
|***Ctrl w, h、j、k、l***|切换窗口|***Ctrl w, H、J、K、L***|移动窗口到最左下上右|
|***Ctrl w r***|互换窗口|
|***:resize 30***|调整当前窗口高度|
|***Ctrl w =***|所以窗口统一高度|
|***Ctrl w 1+***|当前窗口高度+1|***Ctrl w -***|当前窗口高度-1|
|***Ctrl f***|向前翻一页|***Ctrl b***|向后翻一页|
|***Ctrl u***|向前翻半页|***Ctrl d***|向后翻半页|

## set

|:set||||||
|:---|:---|:---|:---|:---|:---|
|***:set ff=unix***|unix格式 行结尾 ***\n***|***:set ff=dos***|dos格式 行结尾 ***\r\n*** |***:set ff***|查看行结尾格式|
|***:set nu*** or ***:set number***|显示行号|***:set nonu*** or ***:set nonumber***|隐藏行号|
|***:set wrap***|折行|***:set nowrap***|取消折行|
|***:set hlsearch***|高亮显示搜索结果|***:set nohlsearch***|取消高亮显示搜索结果|
|***:set autoread***|文件更新自动读取新文件|***:set noautoread***|取消文件更新自动读取新文件|

## .vimrc

### ~/.vimrc

```vimrc
set nu
set nocompatible
set laststatus=2
set statusline=%F%m%r%h%w%=(%{&ff}/%Y)\ (line\ %l\/%L,\ col\ %c)
set linespace=3
set cursorline
set cursorcolumn

highlight CursorLine term=reverse,bold cterm=reverse,bold ctermfg=DarkGray ctermbg=black guibg=Grey90
highlight CursorColumn term=reverse,bold cterm=reverse,bold ctermfg=DarkGray ctermbg=black guibg=Grey90

syntax enable
filetype plugin indent on
let g:rustfmt_autosave = 1
```

### ~/_vimrc

```vimrc
" 将当前行向上移动一行
inoremap <M-k> <Esc>kddpk
nnoremap <M-k> kddpk
" 将当前行向下移动一行
inoremap <M-j> <Esc>ddp
nnoremap <M-j> ddp

set ff=unix
" 去掉有关vi一致性模式，避免以前版本的bug和局限
set nocompatible
" indent :set indent :set ai等自动缩进，想使用backspace将字段缩进的删除，必须设置这个选项。否则不响应
" eol 如果 insert 模式再行开头想使用backspace合并两行，需要设置eol
" start 想要删除此次插入前的输入，需要设置start
set backspace=indent,eol,start
set nu
set guifont=JetBrains_Mono_SemiBold:h12
set laststatus=2  "显示状态栏
set statusline=%F%m%r%h%w%=(%{&ff}/%Y)\ (line\ %l\/%L,\ col\ %c)
"上面的状态栏展示信息比较多，，可以如上所示进行集合性配置，如果懒得一一理解，可直接复制进配置文件，因为所有配置对于提升你编程效率都有帮助。当然如果你不嫌麻烦，也可以以下面所示形式单独配置（注意去掉前面”号）
"set statusline+=%{&ff}  "显示文件格式类型
"set statusline+=%h  "帮助文件标识
"set statusline+=%m  "可编辑文件标识
"set statusline+=%r  "只读文件标识
"set statusline+=%y  "文件类型
"set statusline+=%c  "光标所在列数
"set statusline+=%l/%L  "光标所在行数/总行数
"set statusline+=\ %P  "光标所在位置占总文件百分比

" Show a few lines of context around the cursor. Note that this makes the text
" scroll if you mouse-click near the start or end of the window.
set scrolloff=5
set linespace=3
" Don't use Ex mode, use Q for formatting.
map Q gq

" rust.vim start "
syntax enable
filetype plugin indent on
" let current_compiler = 'rustc'
let g:rustfmt_autosave = 1
let g:rustfmt_fail_silently = 0
" let g:syntastic_rust_checkers = ['cargo']
" rust.vim end"
```

## plugin

### rust.vim

- [https://github.com/rust-lang/rust.vim](https://github.com/rust-lang/rust.vim)
- 插件位置：***~\.vim\pack\plugins\start***

## 注意

vim的`~/.vimrc`配置项，屏蔽掉下面这句话：
```shell
set fileencodings=utf-8,gb2312,gbk,gb18030,ucs-bom
```
再用 vim 打开`jpeg`文件，显示`ffd8 ffc0 0011 0804`和 `ffd9 0a`，显示正确\
vim 为了支持识别和显示中文，规定了 vim 的 `fileencodings`\
当vim打开文件时，会使用规定的编码格式对数据进行解析\
jpeg的文件头`FFD8`、尾`FFD9` 不是任何一个中文的编码，vim找不到对应的中文字，就显示为`？？`，即：`3f3f`