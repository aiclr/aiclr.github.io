+++
title = 'usr_03 Moving around'
date = '2025-02-07'
draft = true
author = 'aiclr'
categories = ['vim']
tags = ['vim','help','user manual','getting started']
Summary=':help usr_03'
+++

Before you can insert or delete text the cursor has to be moved to the right place. Vim has a large number of commands to position the cursor. This chapter shows you how to use the most important ones. You can find a list of these commands below [Q_lr](../../../quickref#q_lr-left-right-motions).

- [03.1    Word movement](#031-word-movement)
- [03.2    Moving to the start or end of a line](#032-moving-to-the-start-or-end-of-a-line)
- [03.3    Moving to a character](#033-moving-to-a-character)
- [03.4    Matching a paren](#034-matching-a-paren)
- [03.5    Moving to a specific line](#035-moving-to-a-specific-line)
- [03.6    Telling where you are](#036-telling-where-you-are)
- [03.7    Scrolling around](#037-scrolling-around)
- [03.8    Simple searches](#038-simple-searches)
- [03.9    Simple search patterns](#039-simple-search-patterns)
- [03.10   Using marks](#0310-using-marks)


Next chapter: [usr_04.txt](../usr_04)  Making small changes
Previous chapter: [usr_02.txt](../usr_02)  The first steps in Vim
Table of contents: [usr_toc.txt](../../usr_toc)

___
___

## 03.1 Word movement

To move the cursor forward one word, use the ***w*** command. Like most Vim commands, you can use a numeric prefix to move past multiple words. For example, ***3w*** moves three words. This figure shows how it works (starting at the position marked with ***x***):
```text
        This is a line with example text
          x-->-->->----------------->
             w  w w    3w
```
Notice that ***w*** woves to the start of the next word if it already is at the start of a word.<br/>
The ***b*** command moves backword to the start of the previous word:
```text
        This is a line with example text
        <----<--<-<---------<--x
           b   b b    2b      b
```
There is also the ***e*** command that moves to the next end of a word and ***ge***, which moves to the previous end of a word:
```text
        This is a line with example text
           <----<----x---->------------>
           2ge   ge     e       2e
```
If you are at the last word of a line, the ***w*** command will take you to the first word in the next line. Thus you can use this to move through a paragraph, much faster than using ***l***. ***b*** does the same in the other direction.

A word ends at a non-word character, such as a ***.***, ***-*** or ***)***. To change what Vim considers to be a word, see the ***iskeyword*** option. If you try this out in the help directory, ***iskeyword*** needs to be reset for the examples to work:
```text
        :set iskeyword&
```
It is also possible to move by white-space separated WORDs. This is not a word in the normal sense, that's why the uppercase is used. The commands for moving by WORDs are also uppercase, as this figure shows:
```text
               ge      b          w                             e
               <-     <-         --->                          --->
        This is-a line, with special/separated/words (and some more).
           <----- <-----         -------------------->         ----->
             gE      B                   W                       E
```
With this mix of lowercase and uppercase commands, you can quickly move forward and backward through a paragraph.

___
___

## 03.2 Moving to the start or end of a line

The ***$*** command moves the cursor to the end of a line. If your keyboard has an `<End>` key it will do the same thing.

The ***^*** command moves to the first non-blank character of the line. The ***0*** command (zero) moves to the very first character of the line, and the `<Home>` key does the same thing. In a picture (***.*** indicates a space):
```text
                  ^
             <-----------x
        .....This is a line with example text
        <----------------x   x-------------->
                0                  $
```
(the ***.....*** indicates blanks here)

The `$` command takes a count, like most movement commands. But moving to the end of the line several times doesn't make sense. Therefore it causes the editor to move to the end of another line. For example, `1$` moves you to the end of the first line (the one you're on), `2$` to the end of the next line, and so on.<br/>
The `0` command doesn't take a count argument, because the `0` would be part of the count. Unexpectedly, using a count with `^` doesn't have any effect.

___
___

## 03.3 Moving to a character

One of the most useful movement commands is the single-character search command. The command `fx` searches forward in the line for the single character **x**. Hint: `f` stands for ***Find***.<br/>
For example, you are at the beginning of the following line. Suppose you want to go to the ***h*** of human. Just execute the command `fh` and the cursor will be positioned over the ***h***:
```text
        To err is human.  To really foul up you need a computer.
        ---------->--------------->
            fh           fy
```
This also shows that the command `fy` moves to the end of the word really.<br/>
You can specify a count; therefore, you can go to the ***l*** of ***foul*** with `3fl`:
```text
        To err is human.  To really foul up you need a computer.
                  --------------------->
                           3fl
```
The `F` command searches to the left:
```text
        To err is human.  To really foul up you need a computer.
                  <---------------------
                            Fh
```
The `tx` command works like the `fx` command, except it stops one character before the searched character. Hint: `t` stands for ***To***. The backward version of this command is `Tx`.
```text
        To err is human.  To really foul up you need a computer.
                   <------------  ------------->
                        Th              tn
```
These four commands can be repeated with `;`. `,` repeats in the other direction. The cursor is never moved to another line. Not even when the sentence continues.

Sometimes you will start a search, only to realize that you have typed the wrong command. You type `f` to search backward, for example, only to realize that you really meant `F`. To about a search, press `<Esc>`. So `f<Esc>` is an aborted forward search and doesn't do anything. Note: `<Esc>` cancels most operations, not just searches.

___
___

## 03.4 Matching a parenthesis

When writing a program you often end up with nested () constructs. Then the `%` command is very handy: It moves to the matching paren. If the cursor is on a ***(*** it will move to the matching ***)***. If it's on a ***)*** it will move to the matching ***(***.
```text
                            %
                         <----->
                if (a == (b * c) / d)
                   <---------------->
                            %
```
This also works for ***[]*** and ***{}*** pairs. (This can be defined with the `matchpairs` option.)

When the cursor is not on a useful character, `%` will search forward to find one. Thus if the cursor is at the start of the line of the previous example, `%` will search forward and find the first `(`. Then it moves to its match:
```text
                if (a == (b * c) / d)
                ---+---------------->
                           %
```

___
___

## 03.5 Moving to a specific line

If you are a C or C++ programmer, you are familiar with error messages such as the following:
```text
        prog.c:33: j   undeclared (first use in this function)
```
This tells you that you might want to fix something on line 33. So how do you find line 33? One way is to do `9999k` to go to the top of the file and `32j` to go down thirty-two lines. It is not a good way, but it works, A much better way of doing things is to use the `G` command. With a count, this command positions you at the given line number. For example, `33G` puts you on line 33. (For a better way of going through a compiler's error list, see [usr_30.txt](../../editing_effectively/usr_30), for information on the `:make` command.)<br/>
With no argument, `G` positions you at the end of the file. A quick way to go to the start of a file use `gg`. `1G` will do the same, but is a tiny bit more typing.
```text
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
```
Another way to move to a line is using the  `%` command with a count. For example `50%` moves you to halfway the file. `90%` goes to near the end.

The previous assumes that you want to move to a line in the file, no matter if it's currently visible or not. What if you want to move to one of the lines you can see? This figure shows the three commands you can use:
```text
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
HintsL `H` stands for Home, `M` for Middle and `L` for Last. Alternatively, `H` for high, `M` for Middle and `L` for low.

___
___

## 03.6 Telling where you are

To see where you are in a file, there are three ways:

1. Use the `CTRL-G` command. You get a message like this (assuming the `ruler` option is off):
```text
       "usr_03.txt" line 233 of 650 --35%-- col 45-52
```
This shows the same of the file you are editing, the line number where the cursor is, the total number of lines, the percentage of the way through the file and the column of the cursor.<br>
Sometimes you will see a split column number. For example, ***col 2-9***. This indicates that the cursor is positioned on the second character, but because character one is a tab, occupying eight spaces worth of columns, the screen column is ***9***.

2. Set the `number` option. This will display a line number in front of every line:
```text
        :set number
        :set nu
```
To switch this off again:
```text
        :set nonumber
        :set nonu
```
Since `number` is a boolean option, prepending `no` to its names has the effect of switching it off. A boolean option has only these two values, it is either on or off.<br>
Vim has many options. Besides the boolean ones there are options with a numerical value and string options. You will see examples of this where they are used.

3. Set the `ruler` option. This will display the cursor position in the lower right corner of the Vim window:
```text
        :set ruler
```
Using the `ruler` option has the advantage that it doesn't take much room, thus there is more space for your text.

___
___

## 03.7 Scrolling around

