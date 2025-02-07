+++
title = 'usr_02 The first steps in Vim'
date = '2025-02-07'
draft = true
author = 'aiclr'
categories = ['vim']
tags = ['vim','help','user manual','getting started']
Summary='This chapter provides just enough information to edit a file with Vim. Notwell or fast, but you can edit. Take some time to practice with these commands, they form the base for what follows.'
+++

Next chapter: [usr_03](../usr_03)  Moving around
<br/>
Previous chapter: [usr_01](../usr_01)  About the manuals

##

This chapter provides just enough information to edit a file with Vim. Notwell or fast, but you can edit. Take some time to practice with these commands, they form the base for what follows.

## Running Vim for the First Time

To start Vim, enter this command: `gvim file.txt`. In UNIX you can type this at any command prompt. If you are running Mocrosoft Windows, open a Command Prompt and enter the command. In either case, Vim starts editing a file called file.txt. Because this is a new file, you get a blank window. This is what your screen will look like:

        +---------------------------------------+
        |#                                      |
        |~                                      |
        |~                                      |
        |~                                      |
        |~                                      |
        |"file.txt" [New file]                  |
        +---------------------------------------+
                ('#' is the cursor position.)
The tilde(~) lines indicate lines not in the file. In other words, when Vim runs out of file to display, it displays tilde lines. At the bottom of the screen, a message line indicates the file is named file.txt and shows that you are creating a new file. The message information is temporary and other information overwrites it.

### THE VIM COMMAND

The gvim command causes the editor to create a new window for editing. If you use this command:
```text
vim file.txt
```
the editing occurs inside your command window. In other words, if you are running inside an xterm, the editor uses your xterm window. If you are using as MS-Windows command prompt window, the editing occurs inside this window. The text in the window will look the same for both versions, but with gvim you have extra features, like a menu bar. More about that later.

## Inserting text

The Vim editor is a modal editor. That means that the editor behaves differently, depending on which mode you are in. The two basic modes are called **Normal mode** and **Insert mode**. In Normal mode the characters you type are commands. In Insert mode the characters are inserted as text.

Since you have just started Vim it will be in Normal mode. To start Insert mode you type the `i` command(i for Insert). Then you can enter the text. It will be inserted into the file. Do not worry if you make mistakes; you can correct then later. To enter the following programmer's limerick, this is what you type:
```text
iA very intelligent turtle
Found programming UNIX a hurdle
```
After typing **turtle** you press the `<Enter>`key to start a new line. Finally you press the `<Esc>` key to stop Insert mode and go back to Normal mode. You now have two lines of text in your Vim window:

        +---------------------------------------+
        |A very intelligent turtle              |
        |Found programming UNIX a hurdle        |
        |~                                      |
        |~                                      |
        |                                       |
        +---------------------------------------+

### WHAT IS THE MODE?

To be able to see what mode you are in, type this command:
```text
:set showmode
```
You will notice that when typing the colon Vim moves the cursor to the last line of the window. That's where you type colon commands (commands that start with a colon). Finish this command by pressing the `<Enter>` key (all commands that start with a colon are finished this way).

Now, if you type the `i` command Vim will display **--INSERT--** at the bottom of the window. This indicates you are in Insert mode.

        +---------------------------------------+
        |A very intelligent turtle              |
        |Found programming UNIX a hurdle        |
        |~                                      |
        |~                                      |
        |-- INSERT --                           |
        +---------------------------------------+

If you press `<Esc>` to go back to Normal mode the last line will be made blank.

### GETTING OUT OF TROUBLE



##

Next chapter: [usr_03](../usr_03)  Moving around