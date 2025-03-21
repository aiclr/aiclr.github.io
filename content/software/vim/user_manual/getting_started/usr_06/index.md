+++
title = 'usr_06 Using syntax highlighting'
date = '2025-03-06'
author = 'aiclr'
categories = ['vim']
tags = ['vim','help','user manual','getting started']
Summary=':help usr_06'
+++

Black and white text is boring. With colors your file comes to life. This not only looks nice, it also speeds up your work. Change the colors used for the different sorts of text. Print your text, with the colors you see on the screen.

- [06.1    Switching it on](#061-switching-it-on)
- [06.2    No or wrong colors?](#062-no-or-wrong-colors)
- [06.3    Different colors](#063-different-colors)
- [06.4    With colors or without colors](#064-with-colors-or-without-colors)
- [06.5    Printing with colors](#065-printing-with-colors)
- [06.6    Further reading](#066-further-reading)

Next chapter: [usr_07.txt  Editing more than one file](../usr_07)<br>
Previous chapter: [usr_05.txt  Set your settings](../usr_05)<br>
Table of contents: [usr_toc.txt](../../usr_toc)

___
___

## 06.1 switching it on

It all starts with one simple command:
```text
        :syntax enable
```
That should work in most situations to get color in your files. Vim will automagically detect the type of file and load the right syntax highlighting. Suddenly comments are blue, keywords brown and string red. This makes it easy to overview the file. After a while you will find that black&white text slows you down!

If you always want to use syntax highlighting, put the `:syntax enable` command in your [vimrc](../../../starting#vimrc-exrc) file.

If you always want syntax highlighting only when the terminal supports colors, you can put this in your [vimrc](../../../starting#vimrc-exrc) file:
```text
        if &t_Co > 1
           syntax enable
        endif
```
If you want syntax highlighting only in the GUI version, put the `:syntax enable` command in your [gvimrc](../../../gui#gvimrc) file.

___
___

## 06.2 No or wrong colors?

There can be a numberof reasons why you don't see colors:<br>
- You terminal does not support colors.<br>
Vim will use bold, italic and underlined text, but this doesn't look very nice. You probably will want to try to get a terminal with colors. For Unix, I recommend the xterm from the XFree86 project: [xfree-xterm](../../../syntax#xfree-xterm).
- Your terminal does support colors, but Vim doesn't know this.<br>
Make sure your **$TERM** setting is correct. For example, when using an xterm that supports colors: `setenv TERM xterm-color` or (depending on your shell): `TERM=xterm-color; export TERM`<br>
The terminal name must match the terminal you are using. If it still doesn't work, have a look at [xterm-color](../../../syntax#xterm-color-color-xterm), which shows a few ways to make Vim display colors (not only for an xterm).
- The file type is not recognized.<br>
Vim doesn't know all file types, and sometimes it's near to impossible to tell what language a file uses. Try this command: `:set filetype`<br>
If the result is `filetype=` then the problem is indeed that Vim doesn't know what type of file this is. You can set the type manually: `:set filetype=fortran`<br>
To see which types are available, look in the directory **$VIMRUNTIME/syntax**. For the GUI you can use the Syntax menu. Setting the filetype can also be done with a [modeline](../../../options#modeline-vim-vi-ex-e520), so that the file will be highlighted each time you edit it. For example, this line can be used in a Makefile (put it near the start or end of the file): `# vim: syntax=make`<br>
You might know how to detect the file type yourself. Often the file name extension (after the dot) can be used.<br>
See [new-filetype](../../../filetype#new-filetype) for how to tell Vim to detect that file type.
- There is no highlighting for your file type.<br>
You could try using a similar file type by manually setting it as mentioned above. If that isn't good enough, you can write your own syntax file, see [mysyntaxfile](../../../syntax#mysyntaxfile).

Or the colors could be wrong:<br>
- The colored text is very hard to read.<br>
Vim guesses the background color that you are using. If it is black (or another dark color) it will use light colors for text. If it is white (or another light color) it will use dark colors for text. If Vim guessed wrong the text will be hard to read. To solve this, set the [background](../../../options#background-bg) option. For a dark background: `:set background=dark` And for a light background: `:set background=light`<br>
Make sure you put this _before_ the `:syntax enable` command, otherwise the colors will already have been set. You could do `:syntax reset` after setting `background` to make Vim set the default colors again.
- The colors are wrong when scrolling bottom to top.<br>
Vim doesn't read the whole file to parse the text. It starts parsing wherever you are viewing the file. That saves a lot of time, but sometimes the colors are wrong. A simple fix is hitting `CTRL-L`. Or scroll back a bit and then forward again.<br>
For a real fix, see [:syn-sync](../../../syntax#syn-sync-e403-e404). Some syntax files have a way to make it look further back, see the help for the specific syntax file. For example, [tex.vim](../../../syntax#tex) for the Tex syntax.

___
___

## 06.3 Different colors

### :syn-default-override

If you don't like the default colors, you can select another color scheme. In the GUI use the Edit/Color Scheme menu. You can also type the command:
```text
        :colorscheme evening
```
**evening** is the name of the color scheme. There are several others you might want to try out. Look in the directory ***$VIMRUNTIME/colors***.

When you found the color scheme that you like, add the `:colorscheme` command to your [vimrc](../../../starting#vimrc-exrc) file.

You could also write your own color scheme. This is how you do it:

- Select a color scheme that comes close. Copy this file to your own Vim directory. For Unix, this should work:

```text
        !mkdir ~/.vim/colors
        !cp $VIMRUNTIME/colors/morning.vim ~/.vim/colors/mine.vim
```
This is done from Vim, because it knows the value of `$VIMRUNTIME`.

- Edit the color scheme file.

||Edit the color scheme file. These entries are useful|
|:---|:---|
|term|attributes in a B&W terminal|
|cterm|attributes in a color terminal|
|ctermfg|foreground color in a color terminal|
|ctermbg|background color in a color terminal|
|gui|attributes in the GUI|
|guifg|foreground color in the GUI|
|guibg|background color in the GUI|

For example, to make comments green:
```text
        :highlight Comment ctermfg=green guifg=green
```
Attributes you can use for **cterm** and **gui** are **bold** and **underline**. If you want both, use **bold,underline**. For details see the [:highlight](../../../highlight-hi-e28-e411-e415) command.

- Tell Vim to always use your color scheme. Put this line in your [vimrc](../../../starting#vimrc-exrc):

```text
        colorscheme mine
```
If you want to see what the most often used color combinations look like, use this command:
```text
        :runtime syntax/colortest,vim
```
You will see text in various color combinations. You can check which ones are readable and look nice. These aren't the only colors available to you though. You can specify `#rrggbb` hex colors and you can define new names for hex colors in [v:colornames](../../../eval#vcolornames) like so:
```text
        let v:colornames['mine_red'] = '#aa0000'
```
If you are authoring a color scheme for others to use, it is important to define these colors only when they do not exist:
```text
        call extend(v:colornames, {'mine_red': '#aa0000'}, 'keep')
```
This allows users of the color scheme to override the precise definition of that color prior to loading your color scheme. For example, in a [.vimrc](../../../starting#viminit) file:
```text
        runtime colors/lists/css_colors.vim
        let v:colornames['your_red'] = v:colornames['css_red']
        colorscheme yourscheme
```
As a color scheme author, you should be able to reply on some color names for GUI colors. These are defined in ***colors/lists/default.vim***. All such files found on the [runtimepath](../../../options#runtimepath-rtp-vimfiles) are loaded each time the colorscheme command is run. A canonical list is provided by the vim distribution, which should include all X11 colors (previously defined in rgb.txt).

___
___

## 06.4 With colors or without colors

Displaying text in color takes a lot of effort. If you find the displaying too slow, you might want to disable syntax highlighting for a moment:
```text
        :syntax clear
```
When editing another file (or the same one) the colors will come back.

If you want to stop highlighting completely use:
```text
        :syntax off
```
This will completely disable syntax highlighting and remove it immediately for all buffers. See [:syntax-off](../../../syntax#syntax-off-syn-off) for more details.

### :syn-manual

If you want syntax highlighting only for specific files, use this:
```text
        :syntax manual
```
This will enable the syntax highlighting, but not switch it on automatically when starting to edit a buffer. To switch highlighting on for the current buffer, set the [syntax](../../../options#syntax-syn) option:
```text
        :set syntax=ON
```

___
___

## 06.5 Printing with colors

### syntax-printing

In the MS-Windows version you can print the current file with this command:
```text
        :hardcopy
```
You will get the usual printer dialog, where you can select the printer and a few settings. If you have a color printer, the paper output should look the same as what you see inside Vim. But when you use a dark background the colors will be adjusted to look good on white paper.

There are several options that change the way Vim prints:

- [printdevice](../../../options#printdevice-pdev)
- [printheader](../../../options#printheader-pheader)
- [printfont](../../../options#printfont-pfn)
- [printoptions](../../../options#printoptions-popt)

To print only a range of lines, use Visual mode to select the lines and then type the command:
```text
        v100j:hardcopy
```
`v` starts Visual mode. `100j` modes a hundred lines down, they will be highlighted. Then `:hardcopy` will print those lines. You can use other commands to move in Visual mode, of course.

This also works on Unix, if you have a PostScript printer. Otherwise, you will have to do a bit more work. You need to convert the text to HTML first, and then print it from a web browser.

Convert the current file to HTML with this command:
```text
        :TOhtml
```
In case that doesn't work:
```text
        :source $VIMRUNTIME/syntax/2html.vim
```
You will see it crunching away, this can take quite a while for a large file. Some time later another window shows the HTML code. Now write this somewhere (doesn't matter where, you throw it away later):
```text
       write main.c.html
```
Open this file in your favorite brower and print it from there. If all goes well, the output should look exactly as it does in Vim. See [2html.vim](../../../syntax#convert-to-html) for details. Don't forget to delete the HTML file when you are done with it.

Instead of printing, you could also put the HTML file on a web server, and let others look at the colored text.

___
___

## 06.6 Further reading

[usr_44.txt](../../tuning_vim/usr_44) Your own syntax highlighted.<br>
[syntax](../../../syntax#syntax-syntax-highlighting-coloring) All the details.

___
___

Next chapter: [usr_07.txt  Editing more than one file](../usr_07)<br>