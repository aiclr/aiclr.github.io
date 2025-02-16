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
           w  w  w    3w
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

One of the most useful movement commands is 