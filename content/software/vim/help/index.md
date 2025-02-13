+++
title = 'Help'
date = '2025-02-07'
author = 'aiclr'
categories = ['vim']
tags = ['vim','help']
Summary = ':help Vim help docs'
+++

|help|`:help`|帮助|
|---|---|---|
|Move around| Use the cursor keys,or `h` to go left,`j` to go down,`k` to go up,`l` to go right.|移动光标|
|Close this window|Use `:q<Enter>`|关闭帮助窗口|
|Get out of Vim|Use `:qa!<Enter>`(careful, all change are lost!)|退出vim|
|Jump to a subject|Position the cursor on a tag (e.g. bars) and hit `CTRL-]`.|跳到 bars 节|
|Jump to a subject with the mouse|`:set mouse=a` to enablethe mouse (in xterm or GUN). Double-click the left mouse button on a tag, e.g. bars.|使用鼠标左键双击跳转到bars节|
|Jump back|Type `CTRL-O`. Repeat to go further back.|返回上一节|

## Get specific help

It is possible to go directly to whatever you want help on,by giving an argument to the :help command.Prepend something to specify the context: help-context. See [help-summary]() for more contexts and an explanation.

|WHAT|PREPEND|EXAMPLE|
|---|---|---|
|Normal mode command| |`:help x`|
|Visual mode command| `v_`|`:help v_u`|
|Insert mode command|`i_`|`:help i_<Esc>`|
|Command-line command|`:`|`:help :quit`|
|Command-line editing|`c_`|`:help c_<Del>`|
|Vim command argument|`-`|`:help -r`|
|Option|`'`|`:help 'textwidth'`|
|Regular expression|`/`|`:help /[`|

## Search for help

Type `:help word`, then hit `CTRL-D` to see matching help entries for "word". Or use `:helpgrep word`.[:helpgrep]()

## Getting started

Do the Vim tutor, a 30-minute interactive course for the basic commands,see [vimtutor]().Read the [user manual](../user_manual) form start to end:[usr_01](../user_manual/getting_started/usr_01).
