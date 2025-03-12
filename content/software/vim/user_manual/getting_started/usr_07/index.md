+++
title = 'usr_07 Editing more than one file'
date = '2025-03-07'
author = 'aiclr'
categories = ['vim']
tags = ['vim','help','user manual','getting started']
Summary='help :usr_07'
+++

No matter how many files you have, you can edit them without leaving Vim. Define a list of files to work on and jump from one to the other. Copy text from one file and put it in another one.

- [07.1    Edit another file](#071-edit-another-file)
- [07.2    A list of files](#072-a-list-of-files)
- [07.3    Jumping from file to file](#073-jumping-from-file-to-file)
- [07.4    Backup files](#074-backup-files)
- [07.5    Copy text between files](#075-copy-text-between-files)
- [07.6    Viewing a file](#076-viewing-a-file)
- [07.7    Changing the file name](#077-changing-the-file-name)

Next chapter: [usr_08.txt](../usr_08)  Splitting windows<br>
Previous chapter: [usr_06.txt](../usr_06)  Using syntax highlighting<br>
Table of contents: [usr_toc.txt](../../usr_toc)

___
___

## 07.1 Edit another file

So far you had to start Vim for every file you wanted to edit. There is a simple way. To start editing another file, use this command:
```text
        :edit foo.txt
```
You can use any file name instead of ***foo.txt***. Vim will close the current file and open the new one. If the current file has unsaved changes, however, Vim displays an error message and does not open the new file:
```text
        E37: No write since last change (use ! to override)
```
Note:<br>
Vim puts an error ID at the start of each error message. If you do not understand the message or what caused it, look in the help system for this ID. In this case:
```text
        :help E37
```
At this point, you have a number of alternatives. You can write the file using this command:
```text
        :write
```
Or you can force Vim to discard your changes and edit the new file, using the force (!) character:
```text
        :edit! foo.txt
```
If you want to edit another file, but not write the changes in the current file yet, you can make it hidden:
```text
        :hide edit foo.txt
```
The text with changes is still there, but you can't see it. This is further explained in section [22.4](../../editing_effectively/usr_22#224-the-buffer-lis): The buffer list.

___
___

## 07.2 A list of files

You can start Vim to edit a sequence of files. For example:
```shell
        vim one.c two.c three.c
```
This command starts Vim and tells it that you will be editing three files. Vim displays just the first file. After you have done your thing in this file, to edit the next file you use this command:
```text
        :next
```
If you have unsaved changes in the current file, you will get an error message and the `:next` will not work. This is the same problem as with `:edit` mentioned in the previous section. To abandon the changes:
```text
        :next!
```
But mostly you want to save the changes and move on to the next file. There is a special command for this:
```text
        :wnext
```
This does the same as using two separate commands:
```text
        :write
        :next
```

WHERE AM I?

To see which file in the argument list you are editing, look in the window title. It should show something like **(2 of 3)**. This means you are editing the second file out of three files.<br>
If you want to see the list of files, use this command:
```text
        :args
```
This is short for ***arguments***. The output might look like this:
```text
        one.c [two.c] three.c
```
These are the files you started Vim with. The one you are currently editing, ***two.c***, is in square brackets.

### MOVING TO OTHER ARGUMENTS

To go back one file:
```text
        :previous
```
This is just like the `:next` command, except that it moves in the other direction. Again, there is a shortcut command for when you want to write the file first:
```text
        :wprevious
```
To move to the very last file in the list:
```text
        :last
```
And to move back to the first one again:
```text
        :first
```
There is no `:wlast` or `wfirst` command though!

You can use a count for `:next` and `:previous`. To skip two files forward:
```text
        :2next
```

### AUTOMATIC WRITING

When moving around the files and making changes, you have to remember to use `:write`. Otherwise you will get an error message. If you are sure you always want to write modified files, you can tell Vim to automatically write them:
```text
        :set autowrite
```
When you are editing a file which you may not want to write, switch it off again:
```text
        :set noautowrite
```

### EDITING ANOTHER LIST OF FILES

You can redefine the list of files without the need to exit Vim and start it again. Use this command to edit three other files:
```text
        :args five.c six.c seven.h
```
Or use a wildcard, like it's used in the shell:
```text
        :args *.txt
```
Vim will take you to the first file in the list. Again, if the current file has changes, you can either write the file first, or use `:args!` (wirh ! added) to abandon the changes.

### arglist-quit

DID YOU EDIT THE LAST FILE?

When you use a list of files, Vim assumes you want to edit them all. To protect you from exiting too early, you will get this error when you didn't edit the last file in the list yet:
```text
        E173: 46 more files to edit
```
If you really want to exit, just do it again. Then it will work (but not when you did other commands in between).

___
___

## 07.3 Jumping from file to file

To quickly jump between two files, press `CTRL-^` (on English-US keyboards the ^ is above the 6 key). Example:
```text
        :args one.c two.c three.c
```
You are now in one.c.
```text
        :next
```
Now you are in two.c. Now use `CTRL-^` to go back to one.c. Another `CTRL-^` and you are back in two.c. Another `CTRL-^` and you are in one.c again. If you now do:
```text
        :next
```
You are in three.c. Notice that the `CTRL-^` command does not change the idea of where you are in the list of files. Only commands like `:next` and `:previous` do that.

The file you were previously editing is called the `alternate` file. When you just started Vim `CTRL-^` will not work, since there isn't a previous file.

###  PREDEFINED MARKS

After jumping to another file, you can use two predefined marks which are very useful:
```
        `"
```
This takes you to the position where the cursor was when you left the file. Another mark that is remembered is the position where you made the last change:
```text
        `.
```
Suppose you are editing the file *one.txt*. Somewhere halfway through the file you use `x` to delete a character. Then you go to the last line with `G` and write the file with `:w`. You edit several other files, and then use `:edit one.txt` to come back to *one.txt*. If you now use ***&#96;"*** Vim jumps to the last line of the file. Using ***&#96;.*** takes you to the position where you deleted the character. Even when you move around in the file ***&#96;"*** and ***&#96;.*** will take you to the remembered position. At least until you make another change or leave the file.

### FILE MARKS

In section [03.10](../usr_03#0310-using-marks) was explained how you can place a mark in a file with `mx` and jump to that position with ***&#96;x***. That works within one file. If you edit another file and place marks there, these are specific for that file. Thus each file has its own set of marks, they are local to the file.<br>
So far we were using marks with a lowercase letter. There are also marks with an uppercase letter. These are global, they can be used from any file. For example suppose that we are editing the file *foo.txt*. Go to halfway down the file ("50%") and place the F mark there (F for foo):
```text
        50%mF
```
Now edit the file *bar.txt* and place the B mark (B for bar) at its last line:
```text
        GmB
```
Now you can use the ***'F*** command to jump back to halfway foo.txt. Or edit yet another file, type ***'B*** and you are at the end of bar.txt again.

The file marks are remembered until they are placed somewhere else. Thus you can place the mark, do hours of editing and still be able to jump back to that mark.<br>
It's often useful to think of a simple connection between the mark letter and where it is placed. For example, use the H mark in a header file, M in a Makefile and C in a C code file.

To see where a specific mark is, give an argument to the `:marks` command:
```text
        :marks M
```
You can also give several arguments:
```text
        :marks MCP
```
Don't forget that you can use `CTRL-O` and `CTRL-I` to jump to older and newer positions without placing marks there.

___
___

## 07.4 Backup files

Usually Vim does not produce a backup file. If you want to have one, all you need to do is execute the following command:
```text
        :set backup
```
The name of the backup file is the original file with a ~ added to the end. If your file is named data.txt, for example, the backup file name is data.txt~.<br>
If you do not like the fact that the backup files end with ~, you can change the extension:
```text
        :set backupext=.bak
```
This will use data.txt.bak instead of data.txt~.<br>
Another option that matters here is [backupdir](../../../options#backupdir-bdir). It specifies where the backup file is written. The default, to write the backup in the same directory as the original file, will mostly be the right thing.

Note:<br>
When the [backup](../../../options#backup-bk-nobackup-nobk) option isn't set but the [writebackup](../../../options#writebackup-wb-nowritebackup-nowb) is, Vim will still create a backup file. However, it is deleted as soon as writing the file was completed successfully. This functions as a safety against losing your original file when writing fails in some way (disk full is the most common cause; being hit by lightning might be another, although less common).

### KEEPING THE ORIGINAL FILE

If you are editing source files, you might want to keep the file before you make any changes. But the backup file will be overwritten each time you write the file. Thus it only contains the previous version, not the first one.<br>
To make Vim keep the original file, set the [patchmode](../../../options#patchmode-pm-e205-e206) option. This specifies the extension used for the first backup of a changed file. Usually you would do this:
```text
        :set patchmode=.orig
```
When you now edit the file data.txt for the first time, make changes and write the file, Vim will keep a copy of the unchanged file under the name *data.txt.orig*.<br>
If you make further changes to the file, Vim will notice that *data.txt.orig* already exists and leave it alone. Further backup files will then be called *data.txt~* (or whatever you specified with [backupext](../../../options#backupext-bex-e589)).<br>
If you leave [patchmode](../../../options#patchmode-pm-e205-e206) empty (that is the default), the original file will not be kept.

___
___

## 07.5 Copy text between files

This explains how to copy text from one file to another. Let's start with a simple example. Edit the file that contains the text you want to copy. Move the cursor to the start of the text and press `v`. This starts Visual mode. Now move the cursor to the end of the text and press `y`. This yanks (copies) the selected text.<br>
To copy the above paragraph, you would do:
```text
        :edit thisfile
        /This
        vjjjj$y
```
Now edit the file you want to put the text in. Move the cursor to the character where you want the text to appear after. Use `p` to put the text there.
```text
        :edit otherfile
        /There
        p
```
Of course you can use many other commands to yank the text. For example, to select whole lines start Visual mode with `V`. Or use `CTRL-V` to select a rectangular block. Or use `Y` to yank a single line, `yaw` to yank-a-word, etc.<br>
The `p` command puts the text after the cursor. Use `P` to put the text before the cursor. Notice that Vim remembers if you yanked a whole line or a block, and puts it back that way.

### USING REGISTERS

When you want to copy several pieces of text from one file to another, having to switch between the files and writing the target file takes a lot of time. To avoid this, copy each piece of text to its own register.<br>
A register is a place where Vim stores text. Here we will use the registers named a to z (later you will find out there are others). Let's copy a sentence to the f register (f for First):
```text
        "fyas
```
The *yas* command yanks a sentence like before. It's the `"f` that tells Vim the text should be placed in the *f* register. This must come just before the yank command.<br>
Now yank three whole lines to the *l* register (l for line):
```text
        "l3y
```
The count could be before the `"l` just as well. To yank a block of text to the *b* (for block) register:
```text
        CTRL-Vjjww"by
```
Notice that the register specification `"b` is just before the `y` command. This is required. If you would have put it before the `w` command, it would not have worked.<br>
Now you have three pieces of text in the f, l and b registers. Edit another file, move around and place the text where you want it:
```text
        "fp
```
Again, the register specification `"f` comes before the `p` command.<br>
You can put the registers in any order. And the text stays in the register until you yank something else into it. Thus you can put it as many times as you like.

When you delete text, you can also specify a register. Use this to move several pieces of text around. For example, to delete-a-word and write it in the w register:
```text
        "wdaw
```
Again, the register specification comes before the delete command `d`.

### APPENDING TO A FILE

When collecting lines of text into one file, you can use this command:
```text
        :write >> logfile
```
This will write the text of the current file to the end of *logfile*. Thus it is appended. This avoids that you have to copy the lines, edit the log file and put them there. Thus you save two steps. But you can only append to the end of a file.<br>
To append only a few lines, select them in Visual mode before typing `:write`. In chapter 10 you will learn other ways to select a range of lines.

___
___

## 07.6 Viewing a file

Sometimes you only want to see what a file contains, without the intention to ever write it back. There is the risk that you type `:w` without thinking and overwrite the original file anyway. To avoid this, edit the file read-only.<br>
To start Vim in readonly mode, use this command:
```text
        vim -R file
```
On Unix this command should do the same thing:
```text
        view file
```
You are now editing *file* in read-only mode. When you try using `:w` you will get an error message and the file won't be written.<br>
When you try to make a change to the file Vim will give you a warning:
```text
        W10: Warning: Changing a readonly file
```
The change will be done though. This allows for formatting the file, for example, to be able to read it easily.<br>
If you make changes to a file and forgot that it was read-only, you can still write it. Add the ! to the write command to force writing.

If you really want to forbid making changes in a file, do this:
```text
        vim -M file
```
Now every attempt to change the text will fail. The help files are like this, for example. If you try to make a change you get this error message:
```text
        E21: Cannot make changes, 'modifiable' is off
```
You could use the -M argument to setup Vim to work in a viewer mode. This is only voluntary though, since these commands will remove the protection:
```text
        :set modifiable
        :set write
```

___
___

## 07.7 Changing the file name

A clever way to start editing a new file is by using an existing file that contains most of what you need. For example, you start writing a new program to move a file. You know that you already have a program that copies a file, thus you start with:
```text
        :edit copy.c
```
You can delete the stuff you don't need. Now you need to save the file under a new name. The `:saveas` command can be used for this:
```text
        :saveas move.c
```
Vim will write the file under the given name, and edit that file. Thus the next time you do `:write`, it will write *move.c*. *copy.c* remains unmodified.<br>
When you want to change the name of the file you are editing, but don't want to write the file, you can use this command:
```text
        :file move.c
```
Vim will mark the file as *not edited*. This means that Vim knows this is not the file you started editing. When you try to write the file, you might get this message:
```text
        E13: File exists (use ! to override)
```
This protects you from accidentally overwriting another file.

___
___

Next chapter: [usr_08.txt](../usr_08)  Splitting windows