+++
title = 'Vim'
date = '2024-05-23'
author = 'aiclr'
categories = ['vim']
tags = ['vim']
Summary = 'Vim is a terminal text editor. It is an extended version of vi with additional features, including syntax highlighting, a comprehensive help system, native scripting (Vim script), a visual mode for text selection, comparison of files (vimdiff(1)), and tools with restricted capabilities such as rview(1) and rvim(1).'
+++

- [help](../user_manual/getting_started/usr_02#028-finding-help)

___
___

## MODE

```text
:set showmode
```

- normal mode
- insert mode
- visual mode
    - visual line mode
    - visual block mode

**normal mode** `i` -> **insert mode** `<Esc>` -> **normal mode**
**normal mode** `v` -> **visual mode** `<Esc>` -> **normal mode**
**normal mode** `V` -> **visual line mode** `<Esc>` -> **normal mode**
**normal mode** `CTRL-V` -> **visual block mode** `<Esc>` -> **normal mode**
**normal mode** `R` -> **replace mode** `<Esc>` -> **normal mode**

| editing commands ||||
|---|---|---|---|
| a | append 拼接到光标位置 | i | insert 插入到光标位置 |
| A | 光标行尾拼接 | I | 光标行首插入 |
| o | 光标下方新增一行 | O | 光标上方新增一行 |
| s | 删除光标所在字符 | S | 删除光标所在行 |

___
___

## undo & redo

| undo & redo ||
|:---|:---|
| u      | undoes the last edit |
| CTRL-R | (redo) to reverse the preceding command.undoes the undo |
| U      | (undo line) undoes all the changes made on the last line that was edited |

___
___

## Getting out

- `ZZ` command writes the file and exits
- `:wq` write and quit
- `:w` write
- `:q` quit
- `:q!` fource quit without write

___
___

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
|`%`|括号`()[]{}`匹配跳转|
|***G***|跳转到末行|***nG***|跳转到n行|
|***gg***|跳转到首行|
|***H***|移动光标到当前页的第一行|
|***M***|移动光标到当前页的中间行|
|***L***|移动光标到当前页的最后一行|
||to scroll one line at a time||一次滚动一行|
|`CTRL-E`|scroll up|`CTRL-Y`|scroll down|
||To scroll a whole screen at a time||一次滚动一屏|
|`CTRL-F`|To scroll forward|`CTRL-B`|To scroll backward|
|`zz`| To see the context of the line with the cursor ||将光标所在行滚动到屏幕中间位置|
|`zt`|put the cursor line at the top|||
|`zb`|put the cursor line at the bottom|||
||simple searches|||
| `*` | 从上往下搜索光标所在单词 |`#`|从下往上搜索光标所在单词|
|`/include`| `n` 从上往下搜索 ***include***| `N` 从下往上搜索 ***include***|
|`3/include`|goes to the third match of ***include***|||
|`?include`| `n` 从下往上搜索 ***include***| `N` 从上往下搜索 ***include***|
|`4?include`|goes the the fouth match of ***include***|||
| `.*[]^%/\?~$` |特殊字符需要使用 `\` 转义|||
|`<\`| only matches at the begining of a word |`\>`| only matches the end of a word |
|`/the\>`| 搜索 ***the*** 结尾的单词  |`/<\the`|搜索 ***the*** 开头的单词|
|`/<\the\>`| 仅搜索 ***the*** |||
|`/the$`|以 the 开头的行|`/^the`|以 the 结尾的行|
|`/^the$`|仅有 the 的行，前后不能有空格等其他字符|||
|`c.m`| com、cam、cum等|`the\.`| 转义点 搜索 the. |
|named marks||||
|`ma`| marks the place under the cursor as mark ****a****|`:marks`|查看所有marks；最多设置26个marks a到z|
|***&#96;a***| 反引号移动到 ***a*** mark 所在位置|***'a***| 单引号 移动到 ***a*** mark所在行的开头|
|`:set ignorecase`| 搜索时忽略大小写 |`:set noignorecase`| 搜索时不忽略大小写 |
|`:set hlsearch`|高亮显示搜索结果|`:set nohlsearch`|关闭高亮|
|`:nohlsearch`|only remove the highlight||does not reset the option|

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

                                       +----------------+
                                       | some text      |
                                       | some text      |
                                       | some text      |
        +---------------+              | some text      |
        | some text     |  CTRL-U  --> |                |
        |               |              | 123456         |
        | 123456        |              +----------------+
        | 7890          |
        |               |              +----------------+
        | example       |  CTRL-D -->  | 7890           |
        +---------------+              |                |
                                       | example        |
                                       | example        |
                                       | example        |
                                       | example        |
                                       +----------------+

        +------------------+             +------------------+
        | earlier text     |             | earlier text     |
        | earlier text     |             | earlier text     |
        | earlier text     |             | earlier text     |
        | earlier text     |   zz  -->   | line with cursor |
        | earlier text     |             | later text       |
        | earlier text     |             | later text       |
        | line with cursor |             | later text       |
        +------------------+             +------------------+

        /the

        the solder holding one of the chips melted and the
        xxx                       xxx                  xxx

       /the$

        the solder holding one of the chips melted and the
                                                       xxx

       /^the

        the solder holding one of the chips melted and the
        xxx

        c.m

        We use a computer that became the cummin winter.
                 xxx             xxx      xxx

        ter.

        We use a computer that became the cummin winter.
                      xxxx                          xxxx

        ter\.

        We use a computer that became the cummin winter.
                                                    xxxx

             |  example text   ^             |
        33G  |  example text   |  CTRL-O     | CTRL-I
             |  example text   |             |
             V  line 33 text   ^             V
             |  example text   |             |
       /^The |  example text   |  CTRL-O     | CTRL-I
             V  There you are  |             V
                example text

        '       The cursor position before doing a jump
        "       The cursor position when last editing the file
        [       Start of the last change
        ]       End of the last change
```

## 编辑

|编辑||||
|:---|:---|:---|:---|
|`d`| delete 删除指令（剪切）|`x`=`dl`|`x` 是 `dl` 的快捷指令|
|`dw`|从光标位置开始删除一个单词（包括单词后面的空格，不包括光标前内容）|`2d4w`|执行两次，删除4个单词|
|`de`|从光标位置开始删除一个单词（不包括单词后面的空格，不包括光标前内容）|`d2e`|执行一次，删除两个单词|
|`d$`=`D`|从光标位置开始删除到行尾|`d^`|从光标位置开始删除到行首|
|`dh`=`X`|`dj`|`dk`|`dl`=`x`|
|`dd`|删除光标所在行|
|`df>`|`d` 删除到 `f>` 搜索 ***>***的位置|
|`c`| change 修改指令（开启 Insert mode）|
|`c2wbe`|`c` change 操作符|`2w` 移除两个单词，并开启Insert mode|`be` 输入内容|
|`cw`|同 `d`, 会开启 Insert mode|`2c2w`||
|`ce`|同 `d`, 会开启 Insert mode|`c2e`||
|`c$`|同 `d`, 会开启 Insert mode|`c^`|同 `d`, 会开启 Insert mode|
|`cc`=`S`|删除光标所在行并开启 Insert mode|
|`cis`|删除光标所在句子（不包括句子末尾的空格）|`cas`|删除光标所在句子（包括句子末尾的空格）|
|`ch`|`cj`|`ck`|`cl`=`s`|
|`r`| replace 替换指令||等待输入，替换光标位置的字符，不会切换到 Insert mode|
|`rT`|将光标位置替换为 ***T***|`clT<Esc>` or `sT<Esc>`|功能同 `rT`|
|`5rx`|从光标位置开始，将五个字符替换为 ***x***|
|`r<Enter>`|将光标位置替换为换行|`4r<Enter>`|将光标位置开始的四个字符，替换为换行|
|`.`|重复执行变更指令|
|`p`|paste 粘贴指令|`xp`|快速转换光标字符与其后字符的位置|
|`y`|yank 复制指令|
|`yw`|同 `d`|`y2w`|
|`ye`|同 `d`|`y2e`|
|`y$`|同 `d`|`y^`|同 `d`|
|`yy`|复制光标所在行|
|`yh`|`yj`|`yk`|`yl`|
|`"*yy`| 复制到 `*` 号粘贴板|`"*p`|从 `*` 粘贴板粘贴|
|`"+yy`| 复制到 `+` 号粘贴板|`"+p`|从 `+` 粘贴板粘贴|
|`*`|寄存器，与系统粘贴板交互|`+`|寄存器与系统粘贴板交互|

```text
        To err is human. To really foul up you need a computer.
                         ------------------>
                                 d4w

        To err is human. you need a computer.
###
        To err is human. you need a computer.
                        -------->
                           d2e

        To err is human. a computer.
###
        To err is human. a computer.
                       ------------>
                            d$

        To err is human
###
        To err is human
           ------->
             c2wbe<Esc>

        To be human
###
        c       the change operator
        2w      move two words (they are deleted and Insert mode started)
        be      insert this text
        <Esc>   back to Normal mode
###
        x  stands for  dl  (delete character under the cursor)
        X  stands for  dh  (delete character left of the cursor)
        D  stands for  d$  (delete to end of the line)
        C  stands for  c$  (change to end of the line)
        s  stands for  cl  (change one character)
        S  stands for  cc  (change a whole line)
###
        there is somerhing grong here
        rT           rt    rw

        There is something wrong here
###
        There is something wrong here
                           5rx

        There is something xxxxx here
###
                              To <B>generate</B> a table of <B>contents
        f<   find first <     --->
        df>  delete to >         -->
        f<   find next <           --------->
        .    repeat df>                     --->
        f<   find next <                       ------------->
        .    repeat df>                                     -->
###
        /four<Enter>    find the first string "four"
        cwfive<Esc>     change the word to "five"
        n               find the next "four"
        .               repeat the change to "five"
        n               find the next "four"
        .               repeat the change
                        etc.
###
        teh     th     the
         x       p
###
x       delete character under the cursor (short for "dl")
X       delete character before the cursor (short for "dh")
D       delete from cursor to end of line (short for "d$")
dw      delete from cursor to next start of word
db      delete from cursor to previous start of word
diw     delete word under the cursor (excluding white space)
daw     delete word under the cursor (including white space)
dG      delete until the end of the file
dgg     delete until the start of the file
```

##


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