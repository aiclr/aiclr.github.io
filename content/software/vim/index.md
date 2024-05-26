+++
title = 'Vim'
date = '2024-05-23'
author = 'aiclr'
categories = ['software']
tags = ['vim']
Summary = 'Vim is a terminal text editor. It is an extended version of vi with additional features, including syntax highlighting, a comprehensive help system, native scripting (Vim script), a visual mode for text selection, comparison of files (vimdiff(1)), and tools with restricted capabilities such as rview(1) and rvim(1).'
+++

|vim||||
|:---|:---|:---|:---|
|insert model||||
|a|光标后|i|光标前|
|A|光标行尾|I|光标行首|
|o|光标下方新增一行|O|光标上方新增一行|
|s|删除光标所在字符|S|删除光标所在行|
|-|
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
|jump|
|***%***|括号匹配跳转|
|***$***|跳转到行尾|***^***|跳转到行首|
|***G***|跳转到末行|***gg***|跳转到首行|
|***H***|移动光标到当前页的第一行|***L***|移动光标到当前页的最后一行|
|***M***|移动光标到当前页的中间行|
|***e***|移动光标到单词末尾|***b***|移动光标到单词开头|
|***w***|移动光标到下一个单词开头|
| * |移动光标到上一个insert位置|
|-|
|split|
|***:sp***|水平分割窗口|***:vsp***|垂直分割窗口|
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
|***Ctrl w +***|当前窗口高度+1|***Ctrl w -***|当前窗口高度-1|

## set

|:set||||||
|:---|:---|:---|:---|:---|:---|
|***:set ff=unix***|unix格式 行结尾 ***\n***|***:set ff=dos***|dos格式 行结尾 ***\r\n*** |***:set ff***|查看行结尾格式|
|***:set nu*** or ***:set number***|显示行号|***:set nonu*** or ***:set nonumber***|隐藏行号|
|***:set wrap***|折行|***:set nowrap***|取消折行|
|***:set hlsearch***|高亮显示搜索结果|***:set nohlsearch***|取消高亮显示搜索结果|
|***:set autoread***|文件更新自动读取新文件|***:set noautoread***|取消文件更新自动读取新文件|

## .vimrc

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