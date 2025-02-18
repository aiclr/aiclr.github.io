+++
title = 'usr_32 The undo tree'
date = '2025-02-07'
draft = true
author = 'aiclr'
categories = ['vim']
tags = ['vim','help','user manual','editing effectively']
Summary='Vim provides multi-level undo.  If you undo a few changes and then make a new change you create a branch in the undo tree.  This text is about moving through the branches.'
+++

Vim provides multi-level undo.  If you undo a few changes and then make a new change you create a branch in the undo tree.  This text is about moving through the branches.

- [32.1    Undo up to a file write](#32.1-undo-up-to-a-file-write)
- [32.2    Numbering changes](#32.2-numbering-changes)
- [32.3    Jumping around the tree](#32.3-jumping-around-the-tree)
- [32.4    Time travelling](#32.4-time-travelling)

Next chapter: [usr_40](../../tuning_vim/usr_40)  Make new commands<br/>
Previous chapter: [usr_31](../usr_31)  Exploiting the GUI<br/>
Table of contents: [usr_toc](../../usr_toc)

___
___

## 32.2 Numbering changes

In section [02.5](../../getting_started/usr_02/#02.5-undo-and-redo) we only discussed one line of undo/redo. But it is also possible to branch off. This happens when you undo a few changes and then make a new change. The new changes become a branch in the undo tree.

Let's start with the text **one**. The first change to make is to append ** too**. And then move to the first **o** and change it into **w**. We then have two changes, numbered 1 and 2, and three states of the text:
```text
                one
                 |
              change 1
                 |
              one too
                 |
              change 2
                 |
              one two
```
If we now undo one change, back to **one too**, and change **one** to **me** we create a branch in the undo tree:
```text
                one
                 |
              change 1
                 |
              one too
              /     \
         change 2  change 3
            |         |
         one two    me too
```
You can noe use the `u`command to undo. If you do this twice you get to **one**. Use `CTRL-R` to redo, and you will go to **one too**. One more `CTRL-R` takes you to **me too**. Thus undo and redo go up and down in the tree, using the branch that was last used.

What matters here is the order in which the changes are made. Undo and redo are not considered changes in this context. After each change you have a new state of the text.

### Note

that only the changes are numbered, the text shown in the tree above has no identifier. They are mostly referred to by the number of the change above it. But sometimes by the number of one of the changes below it, especially when moving up in the tree, so that you know which change was just undone.

___
___

Next chapter: [usr_40](../../tuning_vim/usr_40)  Make new commands<br/>