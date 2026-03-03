## Getting Started

Read this from start to end to learn the essential commands.

### usr_01 About the manuals 

`:help usr_01`

This chapter introduces the manials available with Vim. Read this to know the conditions under which the commands are explained.

- [01.1 Two manuals](#011-two-manuals)
- [01.2 Vim installed](#012-vim-installed)
- [01.3 Using the Vim tutor](#013-using-the-vim-tutor)
- [01.4 Copyright](#014-copyright)

{{#include usr_01.md}}

### usr_02 The first steps in Vim

`:help usr_02`

This chapter provides just enough information to edit a file with Vim. Not well or fast, but you can edit. Take some time to practice with these commands, they form the base for what follows.

- [02.1 Running Vim for the First Time](#021-running-vim-for-the-first-time)
- [02.2 Inserting text](#022-inserting-text)
- [02.3 Moving around](#023-moving-around)
- [02.4 Deleting characters](#024-deleting-characters)
- [02.5 Undo and Redo](#025-undo-and-redo)
- [02.6 Other editing commands](#026-other-editing-commands)
- [02.7 Getting out](#027-getting-out)
- [02.8 Finding help](#028-finding-help)

{{#include usr_02.md}}

### usr_03 Moving around

`:help usr_03`

Before you can insert or delete text the cursor has to be moved to the right place. Vim has a large number of commands to position the cursor. This chapter shows you how to use the most important ones. You can find a list of these commands below [Q_lr](../../quickref.md#q_lr-left-right-motions).

- [03.1    Word movement](#031-word-movement)
- [03.2    Moving to the start or end of a line](#032-moving-to-the-start-or-end-of-a-line)
- [03.3    Moving to a character](#033-moving-to-a-character)
- [03.4    Matching a paren](#034-matching-a-parenthesis)
- [03.5    Moving to a specific line](#035-moving-to-a-specific-line)
- [03.6    Telling where you are](#036-telling-where-you-are)
- [03.7    Scrolling around](#037-scrolling-around)
- [03.8    Simple searches](#038-simple-searches)
- [03.9    Simple search patterns](#039-simple-search-patterns)
- [03.10   Using marks](#0310-using-marks)

{{#include usr_03.md}}

### usr_04 Making small changes

`:help usr_04`

This chapter shows you several ways of making corrections and moving text around. It teaches you the three basic ways to change text: operator-motion,Visual mode and text objects.

- [04.1    Operators and motions](#041-operators-and-motions)
- [04.2    Changing text](#042-changing-text)
- [04.3    Repeating a change](#043-repeating-a-change)
- [04.4    Visual mode](#044-visual-mode)
- [04.5    Moving text](#045-moving-text)
- [04.6    Copying text](#046-copying-text)
- [04.7    Using the clipboard](#047-using-the-clipboard)
- [04.8    Text objects](#048-text-objects)
- [04.9    Replace mode](#049-replace-mode)
- [04.10   Conclusion](#0410-conclusion)

{{#include usr_04.md}}

### usr_05 Set your settings

`:help usr_05`

Vim can be tuned to work like you want it to. This chapter shows you how to make Vim start with options set to different values. Add pligins to extend Vim's capabilities. Or define your own macros.

- [05.1    The vimrc file](#051-the-vimrc-file)
- [05.2    The example vimrc file explained](#052-the-example-vimrc-file-explained)
- [05.3    The defaults.vim file explained](#053-the-defaultvim-file-explained)
- [05.4    Simple mappings](#054-simple-mappings)
- [05.5    Adding a package](#055-adding-a-package)
- [05.6    Adding a plugin](#056-adding-a-plugin)
- [05.7    Adding a help file](#057-adding-a-help-file)
- [05.8    The option window](#058-the-option-window)
- [05.9    Often used options](#059-often-used-options)

{{#include usr_05.md}}

### usr_06 Using syntax highlighting

`:help usr_06`

Black and white text is boring. With colors your file comes to life. This not only looks nice, it also speeds up your work. Change the colors used for the different sorts of text. Print your text, with the colors you see on the screen.

- [06.1    Switching it on](#061-switching-it-on)
- [06.2    No or wrong colors?](#062-no-or-wrong-colors)
- [06.3    Different colors](#063-different-colors)
- [06.4    With colors or without colors](#064-with-colors-or-without-colors)
- [06.5    Printing with colors](#065-printing-with-colors)
- [06.6    Further reading](#066-further-reading)

{{#include usr_06.md}}

### usr_07 Editing more than one file

`help :usr_07`

No matter how many files you have, you can edit them without leaving Vim. Define a list of files to work on and jump from one to the other. Copy text from one file and put it in another one.

- [07.1    Edit another file](#071-edit-another-file)
- [07.2    A list of files](#072-a-list-of-files)
- [07.3    Jumping from file to file](#073-jumping-from-file-to-file)
- [07.4    Backup files](#074-backup-files)
- [07.5    Copy text between files](#075-copy-text-between-files)
- [07.6    Viewing a file](#076-viewing-a-file)
- [07.7    Changing the file name](#077-changing-the-file-name)

{{#include usr_07.md}}

### usr_08 Splitting windows

`:help usr_08`

Display two different files above each other. Or view two locations in the file at the same time. See the difference between two files by putting them side by side. All this is possible with split windows.

- [08.1    Split a window](#081-split-a-window)
- [08.2    Split a window on another file](#082-split-a-window-on-another-file)
- [08.3    Window size](#083-window-size)
- [08.4    Vertical splits](#084-vertical-splits)
- [08.5    Moving windows](#085-moving-windows)
- [08.6    Commands for all windows](#086-commands-for-all-windows)
- [08.7    Viewing differences with vimdiff](#087-viewing-differences-with-vimdiff)
- [08.8    Various](#088-various)
- [08.9    Tab pages](#089-tab-pages)

{{#include usr_08.md}}

### usr_09 Using the GUI

`:help usr_09`

{{#include usr_09.md}}

### usr_10 Making big changes  

`:help usr_10`

{{#include usr_10.md}}

### usr_11 Recovering from a crash

`:help usr_11`

Did your computer crash? And you just spent hours editing? Don't panic! Vim stores enough information to be able to restore most of your work. This chapter shows you how to get your work back and explains how the swap file is used.

- [11.1    Basic recovery](#111-basic-recovery)
- [11.2    Where is the swap file?](#112-where-is-the-swap-file)
- [11.3    Crashed or not?](#113-crashed-or-not)
- [11.4    Further reading](#114-further-reading)

{{#include usr_11.md}}

### usr_12 Clever tricks

`:help usr_12`

{{#include usr_12.md}}
