+++
title = 'usr_05 Set your settings'
date = '2025-02-07'
draft = true
author = 'aiclr'
categories = ['vim']
tags = ['vim','help','user manual','getting started']
Summary=':help usr_05'
+++

Vim can be tuned to work like you want it to. This chapter shows you how to make Vim start with options set to different values. Add pligins to extend Vim's capabilities. Or define your own macros.

[05.1    The vimrc file](#051-the-vimrc-file)
[05.2    The example vimrc file explained](#052-the-example-vimrc-file-explained)
[05.3    The defaults.vim file explained](#053-the-defaultvim-file-explained)
[05.4    Simple mappings](#054-simple-mappings)
[05.5    Adding a package](#055-adding-a-package)
[05.6    Adding a plugin](#056-adding-a-plugin)
[05.7    Adding a help file](#057-adding-a-help-file)
[05.8    The option window](#058-the-option-window)
[05.9    Often used options](#059-often-used-options)

Next chapter: [usr_06.txt  Using syntax highlighting](../usr_06)<br>
Previous chapter: [usr_04.txt  Making small changes](../usr_04)<br>
Table of contents: [usr_toc.txt](../../usr_toc)

___
___
## 05.1 The vimrc file

### vimrc intro

You probably got tired of typing commands that you use very often. To start Vim with all your favorite option settings and mappings, you write them in what is called the vimrc file. Vim executes the commands in this file when it starts up.

If you already have a vimrc file (e.g., when your sysadmin has one setup for you), you can edit it this way:
```text
        :edit $MYVIMRC
```
If you don't have a vimrc file yet, see [vimrc](../../../starting#vimrc-exrc) to find out where you can create a vimrc file. Also, the `:version` command mentions the name of the ***user vimrc file** Vim looks for.

For Unix and Macintosh this file is always used and is recommended:
```text
        ~/.vimrc
```
For MS-Windows you can use one of these:
```text
        $HOME/_vimrc
        $VIM/_vimrc
```
If you are creating the vimrc file for the first time, it is recommended to put this line at the top:
```text
        source $VIMRUNTIME/defaults.vim
```
This initializes Vim for new users (as opposed to traditional Vi users). See [defaults.vim](../../../starting#default.vim) for the details.

The vimrc file can contain all the commands that you type after a colon. The simplest ones are for setting options. For example, if you want Vim to always start with the `incsearch` option on, add this line your vimrc file:
```text
        set incsearch
```
For this new line to take effect you need to exit Vim and start it again. Later you will learn how to do this without exiting Vim.

This chapter only explains the most basic items. For more information on how to write a Vim script file: [usr_41.txt](../../tuning_vim/usr_41).

___
___

## 05.2 The example vimrc file explained

### vimrc_example.vim

In the first chapter was explained how the example vimrc (include in the Vim distribution) file can be used to make Vim startup in not-compatible mode (see [not-compatible](../usr_01#not-compatible)). The file can be found here:
```text
        $VIMRUNTIME/vimrc_example.vim
```
In this section we will explain the various commands used in this file. This will give you hints about how to set up your own preferences. Not everything will be explained though. Use the `:help` command to find out more.
```text
        " Get the defaults that most users want.
        source $VIMRUNTIME/defaults.vim
```
This loads the ***defaults.vim*** file in the $VIMRUNTIME directory. This sets up Vim for how most users like it. If you are one of the few that don't, then comment out this line. The commands are explained below:<br>
[defaults.vim-explained](#defaultsvim-explained)
```text
        if has("vms")
          set nobackup
        else
          set backup
          if has('persistent_undo')
            set undofile
          endif
        endif
```
This tells Vim to keep a backup copy of a file

___
___

## 05.3 The default.vim file explained
## 05.4 Simple mappings
## 05.5 Adding a package
## 05.6 Adding a plugin
## 05.7 Adding a help file
## 05.8 The option window
## 05.9 Often used options

___
___

Next chapter: [usr_06.txt  Using syntax highlighting](../usr_06)