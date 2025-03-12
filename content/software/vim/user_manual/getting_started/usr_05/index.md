+++
title = 'usr_05 Set your settings'
date = '2025-02-07'
author = 'aiclr'
categories = ['vim']
tags = ['vim','help','user manual','getting started']
Summary=':help usr_05'
+++

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
This tells Vim to keep a backup copy of a file when overwriting it. But not on the VMS system, since it keeps old versions of files already. The backup file will have the same name as the original file with `~` added. See [07.4](../usr_07#074-backup-files)

This also sets the `undofile` option, if available. This will store the multi-level undo information in a file. The result is that when you change a file, exit Vim, and then edit the file again, you can undo the changes made previously. It's a very powerful and useful feature, at the cost of storing a file. For more information see [undo-persistence](../../../undo#undo-persitence).

The **if** command is very useful to set options<bt>
only when some condition is met. More about that in [use_41.txt](../../tuning_vim/usr_41).
```text
        if &t_Co > 2 || has("gui_running")
          set hlsearch
        endif
```
This switches on the `hlsearch` option, telling Vim to highlight matches with the last used search pattern.
```text
        augroup vimrcEx
          au!
          autocmd FileType text setlocal textwidth=78
        augroup END
```
This makes Vim break text to avoid lines getting longer than 78 characters. But only for files that have been detected to be plain text. There are actually two parts here. `autocmd FileType text` is an autocommand. This defines that when the file type is set to `text` the following command is automatically executed. `setlocal textwidth=78` sets the `textwidth` option to 78, but only locally in one file.

The wrapper with `augroup vimrcEx` and `augroup END` makes it possible to delete the autocommand with the `au!` command. See [:augroup](../../../autocmd#aug)
```text
        if has('syntax') && has('eval')
          packadd! matchit
        endif
```
This loads the `matchit` plugin if the required features are available. It makes the `%` command more powerful. This is explained at [matchit-install](#add-package-matchit-install)

___
___

## 05.3 The default.vim file explained

### defaults.vim-explained

The [defaults.vim](../../../starting#defaults.vim-e1187) file is loaded when the user has no vimrc file. When you create a new vimrc file, add this line near the top to keep using it:
```text
        source $VIMRUNTIME/defaults.vim
```
Or use the vimrc_example.vim file, as explained above.

The following explains what defaults.vim is doing.
```text
        if exists('skip_defaults_vim')
          finish
        endif
```
Loading defaults.vim can be disabled with this command:
```text
        let skip_defaults_vim = 1
```
This has to be done in the system vimrc file. See [system-vimrc](../../../starting#system-vimrc). If you have a user vimrc this is not needed, since defaults.vim will not be loaded automatically.
```text
        set nocompatible
```
As mentioned in the first chapter, these manuals explain Vim working in an improved way, thus not completely Vi compatible. Setting the `compatible` option off, thus `nocompatible` takes care of this.
```text
        set backspace=indent,eol,start
```
This specifies where in Insert mode the `<BS>` is allowed to delete the character in front of the cursor. The three items, separated bt commas, tell Vim to delete the white space at the start of the line, a line break and the character before where Insert mode started. See [backspace](../../../options#backspace).
```text
        set history=200
```
Keep 200 commands and 200 search patterns in the history. Use another number if you want to remember fewer or more lines. See [history](../../../options#history).
```text
        set ruler
```
Always display the current cursor position in the lower right corner of the Vim window. See [ruler](../../../options#ruler).
```text
       set showcmd
```
Display an incomplete command in the lower right corner of the Vim window, left of the ruler. For example, when you type `2f`, Vim is waiting for you to type the character to find and `2f` is displayed. When you press `w` next, the `2fw` command is executed and the displayed `2f` is removed.
```text
        +-------------------------------------------------+
        |text in the Vim window                           |
        |~                                                |
        |~                                                |
        |-- VISUAL --                   2f     43,8   17% |
        +-------------------------------------------------+
         ^^^^^^^^^^^                  ^^^^^^^^ ^^^^^^^^^^
          'showmode'                 'showcmd'  'ruler'
```
```text
        set wildmenu
```
Display completion matches in a status line. That is when you type `<Tab>` and there is more than one match. See [wildmenu](../../../options#wildmenu).
```text
        set ttimeout
        set ttimeoutlen=100
```
This makes typing Esc take effect more quickly. Normally Vim waits a second to see if the Esc is the start of an escape sequence. If you have a very slow remote connection, increase the number. See [ttimeout](../../../options#ttimeout).
```text
        set display=truncate
```
Show @@@ in the last line if it is truncted, instead of hiding the whole line. See [dispaly](../../../options#display).
```text
        set incsearch
```
Display the match for a search pattern when halfway typing it. See [incsearch](../../../options#incsearch)
```text
        set nrformats-=octal
```
Do not recognize numbers starting with a zero as octal. See [nrformats](../../../options#nrformats).
```text
        map Q gq
```
This defines a key mapping. More about that in the next section. This defines the `Q` command to do formatting with the `gq` operator. This is how it worked before Vim 5.0. Otherwise the `Q` command starts Ex mode, but you will not need it.
```text
        inoremap <C-U> <C-G>u<C-U>
```
`CTRL-U` in insert mode deletes all entered text in the current line. Use `CTRL-G` u to first break undo, so that you can undo `CTRL-U` after inserting a line break. Revert with `:iunmap <C-U>`.
```text
        if has('mouse')
          set mouse=a
        endif
```
Enable using the mouse if avaiable. See [mouse](../../../options#mouse).
```text
        vnoremap _g y:exe "grep /" .. escape(@", '\\/') .. "/ *.c *.h"<CR>
```
This mapping yanks the visually selected text and searches for it in ***C*** files. You can see that a mapping can be used to do quite complicated things. Still, it is just a sequence of commands that are executed like you typed them.
```text
        syntax on
```
Enable highlighting files in color. See [syntax](../../../options#syntax).

### vimrc-filetype

```text
        filetype plugin indent on
```
This switches on three very clever mechanisms:
1. Filetype detecition.<br>
Whenever you start editing a file, Vim will try to figure out what kind of file this is. When you edit ***main.c***, Vim will see the `.c` extension and recognize this as a ***c*** filetype. When you edit a file that starts with `#!/bin/sh`, Vim will recognize it as a ***sh*** filetype.<br>
The filetype detection is used for syntax highlighting and the other two items below.<br>
See [filetype](../../../filetype#filetypes-file-types).
2. Using filetype plugin files<br>
Many different filetypes are edited with different options. For example, when you edit a ***c*** file, it's very useful to set the [cindent](../../../options#cindent-cin-nocindent-nocin) option to automatically indent the lines. These commonly useful option settings are included with Vim in filetype plugins. You can also add your own, see [write-filetype-plugin](../../writing_vim_script/usr_51#write-filetype-plugin-ftplugin).
3. Using indent files<br>
When editing programs, the indent of a line can often be computed automatically. Vim comes with these indent rules for a number of filetypes. See [:filetype-indent-on](../../../filetype#filetype-indent-on) and [indentexpr](../../../options#indentexpr-inde).

### restore-cursor last-position-jump

```text
    autocmd BufReadPost *
      \ if line("'\"") >= 1 && line("'\"") <= line("$") && &ft !~# 'commit'
      \ |   exe "normal! g`\""
      \ | endif
```
Another autocommand. This time it is used after reading any file. The complicated stuff after it checks if the `'"` mark is defined, and jumps to it if so. The backslash at the start of a line is used to continue the command from the previous line. That avoids a line getting very long.<br>
See [line-continuation](../../../repeat#line-continuation). This only works in a Vim script file, not when typing commands at the command-line.
```text
        command DiffOrig vert new | set bt=nofile | r ++edit # | 0d_ | diffthis
                  \ | wincmd p | diffthis
```
This adds the `:DiffOrig` command. Use this in a modified buffer to see the differences with the file it was loaded from. See [diff](../../../diff) and [:DiffOrig](../../../diff#difforig-diff-original-file).
```text
        set nolangremap
```
Prevent that the langmap option applies to characters that result from a mapping. If set (default), this may break plugins (but it's backward compatible). See [langremap](../../../options#langremap-lrm-nolangremap-nolrm).

___
___

## 05.4 Simple mappings

A mapping enables you to bind a set of Vim commands to a single key. Suppose, for example, that you need to surround certain words with curly braces. In other words, you need to change a word such as `amount` into `{amount}`. With the `:map` command, you can tell Vim that the `F5` key does this job. The command is as follows:
```text
        :map <F5> i{<Esc>ea}<Esc>
```
Note:<br>
When entering this command, you must enter `<F5>` by typing four characters. Similarly, `<Esc>` is not entered by pressing the `<Esc>` key, but by typing five characters. Watch out for this difference when reading the manual!

Let's break this down:
```text
    <F5>        The F5 function key.  This is the trigger key that causes the
                command to be executed as the key is pressed.

    i{<Esc>     Insert the { character.  The <Esc> key ends Insert mode.

    e           Move to the end of the word.

    a}<Esc>     Append the } to the word.
```
After you execute the `:map` command, all you have to do to put `{}` around a word is to put the cursor on the first character and press `F5`.

In this example, the tirgger is a single key; it can be any string. But when you use an existing Vim command, that command will no longer be available.You better avoid that.<br>
One key that can be used with mappings is the backslash. Since you probably want to define more than one mapping, add another character. You could map `\p` to add parentheses around a word, and `\c` to add curly braces, for example:
```text
        :map \p i(<Esc>ea)<Esc>
        :map \c i{<Esc>ea}<Esc>
```
You need to type `\` and the `p` quickly after another, so that Vim knows they belong together.

The `:map` command (with no arguments) lists your current mappings. At least the ones for Normal mode. More about mappings in section [40.1](../../tuning_vim/usr_40#401-key-mapping).

___
___

## 05.5 Adding a package

### add-package matchit-install

A package is a set of files that you can add to Vim. There are two kinds of packages: optional and automatically loaded on startup.

The Vim distribution comes with a few packages that you can optionally use. For example, the matchit plugin. This plugin makes the `%` command jump to matching HTML tags, if/else/endif in Vim scripts, etc. Very useful, although it's not backwards compatible (that's why it is not enabled by default).

To start using the matchit plugin, add one line to your vimrc file:
```text
        packadd! matchit
```
That's all! After restarting Vim you can find help about this plugin:
```text
        :help matchit
```
This works, because when `:packadd` loaded the plugin it also added the package directory in `runtimepath`, so that the help file can be found.

You can find packages on the Internet in various places. It usually comes as an archive or as a repository. For an archive you can follow these steps:
```text
        1. create the package directory:
                mkdir -p ~/.vim/pack/fancy
           "fancy" can be any name of your liking.  Use one that describes the package.
        2. unpack the archive in that directory.  This assumes the top directory in the archive is "start":
                cd ~/.vim/pack/fancy
                unzip /tmp/fancy.zip
           If the archive layout is different make sure that you end up with a path like this:
                ~/.vim/pack/fancy/start/fancytext/plugin/fancy.vim
           Here "fancytext" is the name of the package, it can be anything else.
        Note:
           MS-Windows path like this ~\vimfiles\pack\fancy\start\fancytext\plugin\fancy.vim
```

More information about packages can be found here: [packages](../../../repeat#packages)

___
___

## 05.6 Adding a plugin

### add-plugin plugin

Vim's functionally can be extended by adding plugins. A plugin is nothing more than a Vim script file that is loaded automatically when Vim starts. You can add a plugin very easily by dropping it in your plugin directory.
{not available when Vim was compiled without the **+eval** feature}

There are two types of plugins:
- global plugin: Used for all kinds of files
- filetype plugin: Only used for a specific type of file

The global plugins will be discussed first, then the filetype ones [add-filetype-plugin](#add-filetype-plugin-ftplugins)

### GLOBAL PLUGINS

#### standard-plugin

When you start Vim, it will automatically load a number of global plugins. You don't have to do anything for this. They add functionality that most people will want to use, but which was implemented as a Vim script instead of being compiled into Vim. You can find them listed in the help index [standard-plugins-list](../../../help#standard-plugin-list). Also see [load-plugins](../../../starting#load-plugins).

#### add-global-plugin

You can add a global plugin to add functionality that will always be  present when you use Vim. There are only two steps for adding a global plugin:
1. Get a copy of the plugin.
2. Drop it in the right directory.

### GETTING A GLOBAL PLUGIN

Where can you find plugins?
- Some are always loaded, you can see them in the directory ***$VIMRUNTIME/plugin***.
- Some come with Vim. You can find them in the directory ***$VIMRUNTIME/macros*** and its sub-directories and under ***$VIM/vimfiles/pack/dist/opt/***.
- Download from the net. There is a large collection on [http://www.vim.org](http://www.vim.org).
- They are sometimes posted in a Vim [maillist](../../../intro#mail-list-maillist).
- You could write one yourself, see [write-plugin](../../writing_vim_script/usr_51#write-plugin).

Some plugins come as a vimball archive, see [vimball](../../../pi_vimball#vba-vimball-vimball-contents).<br>
Some plugins can be updated automatically, see [getscript](../../../pi_getscript#glvs-contents-glvs-getscript-getlatestvimscripts).

### USING A GLOBAL PLUGIN

First read the text in the plugin itself to check for any special conditions. Then copy the file to your plugin directory:

|system|plugin directory|
|:---|:---|
|Unix|~/.vim/plugin|
|PC|$HOME/vimfiles/plugin or $VIM/vimfiles/plugin|
|Amiga|s:vimfiles/plugin|
|Macintosh|$VIM:vimfiles:plugin|
|Mac OS X|~/.vim/plugin/|

Example for Unix (assuming you didn't have a plugin directory yet):
```text
        mkdir ~/.vim
        mkdir ~/.vim/plugin
        cp /tmp/yourplugin.vim ~/.vim/plugin
```
That's all! Now you can use the commands defined in this plugin.

Instead of putting plugins directly into the ***plugin/*** directory, you may better organize them by putting them into subdirectories under ***plugin/***. As an example, consider using `~/.vim/plugin/perl/*.vim` for all your Perl plugins.

### FILETYPE PLUGINS

#### add-filetype-plugin ftplugins

The Vim distribution comes with a set of plugins for different filetypes that you can start using with this command:
```text
        :filetype plugin on
```
That's all! See [vimrc-filetype](#vimrc-filetype).

If you are missing a plugin for a filetype you are using, or you found a better one, you can add it. There are two steps for adding a filetype plugin:
1. Get a copy of the plugin.
2. Drop it in the right directory.

### GETTING A FILETYPE PLUGIN

You can find them in the same places as the global plugins. Watch out if the type of file is mentioned, then you know if the plugin is a global or a filetype one. The scripts in `$VIMRUNTIME/macros` are global ones, the filetype plugins are in `$VIMRUNTIME/ftplugin`.

### USING A FILETYPE PLUGIN

#### ftplugin-name

You can add a filetype plugin by dropping it in the right directory. The name of this directory is in the same directory mentioned above for global plugins, but the last part is `ftplugin`. Suppose you have found a plugin for the ***stuff*** filetype, and you are on Unix. Then you can move this file to the ftplugin directory:
```shell
mv thefile ~/.vim/ftplugin/stuff.vim
```
If that file already exists you already have a plugin for ***stuff***. You might want to check if the existing plugin doesn't conflict with the one you are adding. If it's OK, you can give the new one another name:
```shell
mv thefile ~/.vim/ftplugin/stuff_too.vim
```
The underscore is used to separate the name of the filetype from the rest, which can be anything. If you use `otherstuff.vim` it wouldn't work, it would be loaded for the `otherstuff` filetype.

On MS-DOS like filesystems you cannot use long filenames. You would run into trouble if you add a second plugin and the filetype has more than six characters. You can use an extra directory to get around this:
```shell
mkdir $VIM/vimfiles/ftplugin/fortran
copy thefile $VIM/vimfiles/ftplugin/fortran/too.vim
```
The generic names for the filetype plugins are:
```text
        ftplugin/<filetype>.vim
        ftplugin/<filetype>_<name>.vim
        ftplugin/<filetype>/<name>.vim
```
Here `<name>` can be any name that you prefer.<br>
Examples for the ***stuff*** filetype on Unix:
```text
        ~/.vim/ftplugin/stuff.vim
        ~/.vim/ftplugin/stuff_def.vim
        ~/.vim/ftplugin/stuff/header.vim
```
The `<filetype>` part is the name of the filetype the plugin is to be used for. Only files of this filetype will use the settings from the plugin. The `<name>` part of the plugin file doesn't matter, you can use it to have several plugins for the same filetype. Note that it must end in `.vim`.

Further reading:
- [filetype-plugins](../../../filetype#filetype-plugins)       Documentation for the filetype plugins and information about how to avoid that mappings cause problems.
- [load-plugins](../../../starting#load-plugins)       When the global plugins are loaded during startup.
- [ftplugin-overrule](../../../filetype#ftplugin-overrule)       Overruling the settings from a global plugin.
- [write-plugin](../../writing_vim_script/usr_51#write-plugin)       How to write a plugin script.
- [plugin-details](../../../filetype#plugin-details)       For more information about using plugins or when your plugin doesn't work.
- [new-filetype](../../../filetype#new-filetype)       How to detect a new file type.

___
___

## 05.7 Adding a help file

### add-local-help

If you are lucky, the plugin you installed also comes with a help file. We will explain how to install the help file, so that you can easily find help for your new plugin.<br>
Let us use the `doit.vim` plugin as an example. This plugin comes with documentation: `doit.txt`. Let's first copy the plugin to the right directory. This time we will do it from inside Vim. (You may skip some of the `mkdir` commands if you already have the directory.)
```text
        :!mkdir ~/.vim
        :!mkdir ~/.vim/plugin
        :!cp /tmp/doit.vim ~/.vim/plugin
```
The `cp` command is for Unix, on MS-Windows you can use `copy`.

Now create a `doc` directory in one of the directories in [runtimepath](../../../options#runtimepath-rtp-vimfiles).
```text
        :!mkdir ~/.vim/doc
```
Copy the help file to the `doc` directory.
```text
        :!cp /tmp/doit.txt ~/.vim/doc
```
Now comes the trick, which allows you to jump to the subjects in the new help file: Generate the local tags file with the `:helptags` command.
```text
        :helptags ~/.vim/doc
```
Now you can use the
```text
        :help doit
```
command to find help for `doit` in the help file you just added. You can see an entry for the local help file when you do:
```text
        :help local-additions
```
The title lines from the local help files are automagically added to this section. There you can see which local help files have bean added and jump to them through the tag.

For writing a local help file, see [write-local-help](../../writing_vim_script/usr_51#write-local-help).

___
___

## 05.8 The option window

If you are looking for an option that does what you want, you can search in the help files here: [options](../../../options). Another way is by using this command:
```text
        :options
```
This opens a new window, with a list of options with a one-line explanation. The options are grouped by subject. Move the cursor to a subject and press `<Enter>` to jump there. Press `<Enter>` again to jump back. Or use `CTRL-O`.

You can change the value of an option. For example, move to the `displaying text` subject. Then move the cursor down to this line:
```text
        set wrap        nowrap
```
When you hit `<Enter>`, the line will change to:
```text
        set nowrap      wrap
```
The option has now been switched off.

Just above this line is a short description of the `wrap` option. Move the cursor one line up to place it in this line. Now hit `<Enter>` and you jump to the full help on the `wrap` option.

For options that take a number or string argument you can edit the value. Then press `<Enter>` to apply the new value. For example, move the cursor a few lines up to this line:
```text
        set so=0
```
Position the cursor on the zero with `$`. Change it into a five with `r5`. Then press `<Enter>` to apply the new value. When you now move the cursor around you will notice that the text starts scrolling before you reach the border. This is what the `scrolloff` option does, it specifies an offset from the window border where scrolling starts.

___
___

## 05.9 Often used options

There are an awful lot of options. Most of them you will hardly ever use. Some of the more useful ones will be mentioned here. Don't forget you can find more help on these options with the `:help` command, with single quotes before and after the option name. For example:
```text
        :help 'wrap'
```
In case you have messed up an option value, you can set it back to the default by putting an ampersand (&) after the option name. Example:
```text
        :set iskeyword&
```

### NOT WRAPPING LINES

Vim normally wraps long lines, so that you can see all of the text. Sometimes it's better to let the text continue right of the window. Then you need to scroll the text left-right to see all of a long line. Switch wrapping off with this command:
```text
        :set nowrap
```
Vim will automatically scroll the text when you move to text that is not displayed. To see a context of ten characters, do this:
```text
        :set sidescroll=10
```
This doesn't change the text in the file, only the way it is displayed.

### WRAPPING MOVEMENT COMMANDS

Most commands for moving around will stop moving at the start and end of a line. You can change that whit the `whichwrap` option. This sets it to the default value:
```text
        :set whichwrap=b,s
```
This allows the `<BS>` key, when used in the first position of a line, to move the cursor to the end of the previous line. And the `<Space>` key moves from the end of a line to the start of the next one.

To allow the cursor keys `<Left>` and `<Right>` to also wrap, use this command:
```text
        :set whichwrap=b,s,<,>
```
This is still only for Normal mode. To let `<Left>` and `<Right>` do this in Insert mode as well:
```text
        :set whichwrap=b,s,<,>,[,]
```
There are a few other flags that can be added, see [whichwrap](../../../options#whichwrap-ww)

### VIEWING TABS

When there are tabs in a file, you cannot see where they are. To make them visible:
```text
        :set list
```
Now every tab is displayed as ^I. And a $ is displayed at the end of each line, so that you can spot trailing spaces that would otherwise go unnoticed.<br>
A disadvantage is that this looks ugly when there are mant Tabs in a file. If you have a color terminal, or are using the GUI, Vim can show the spaces and tabs as highlighted characters. Use the `listchars` option:
```text
        :set listchars=tab:>-,trail:-
```
Now every tab will be displayed as `>---` (with more or less `-`) and trailing white space as `-`. Looks a lot better, doesn't it?

### KEYWORDS

The `iskeyword` option specifies which characters can appear in a word:
```text
        :set iskeyword
          iskeyword=@,48-57,_,192-255
```
The `@` stands for all alphabetic letters. `48-57` stands for ASCII characters 48 to 57, which are the numbers 0 to 9. `192-255` are the printable latin characters.<br>
Sometimes you will want to include a dash in keywords, so that commands like `w` consider `upper-case` to be one word. You can do it like this:
```text
        :set iskeyword+=-
        :set iskeyword
          iskeyword=@,48-57,_,192-255,-
```
If you look at the new value, you will see that Vim has added a comma for you.<br>
To remove a character use `-=`. For example, to remove the underscore:
```text
        :set iskeyword-=_
        :set iskeyword
          iskeyword=@,48-57,192-255,-
```
This time a comma is automatically deleted.

### ROOM FOR MESSAGES

When Vim starts there is one line at the bottom that is used for messages. When a message is long, it is either truncated, thus you can only see part of it, or the text scrolls and you have to press `<Enter>` to continue.<br>
You can set the `cmdheight` option to the number of lines used for messages. Example:
```text
        :set cmdheight=3
```
This does mean there is less room to edit text, thus it's a compromise.

___
___

Next chapter: [usr_06.txt  Using syntax highlighting](../usr_06)