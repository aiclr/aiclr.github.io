+++
title = 'usr_11 Recovering from a crash'
date = '2025-02-07'
author = 'aiclr'
categories = ['vim']
tags = ['vim','help','user manual','getting started']
Summary='Recovering from a crash'
+++

Did your computer crash? And you just spent hours editing? Don't panic! Vim stores enough information to be able to restore most of your work. This chapter shows you how to get your work back and explains how the swap file is used.

- [11.1    Basic recovery](#111-basic-recovery)
- [11.2    Where is the swap file?](#112-where-is-the-swap-file)
- [11.3    Crashed or not?](#113-crashed-or-not)
- [11.4    Further reading](#114-further-reading)

Next chapter: [usr_12.txt  Clever tricks](../usr_12)<br>
Previous chapter: [usr_10.txt  Making big changes](../usr_10)<br>
Table of contents: [usr_toc.txt](../../usr_toc)

___
___

## 11.1 Basic recovery

In most cases recovering a file is quite simple, assuming you know which file you were editing (and the harddisk is still working). Start Vim on the file, with the `-r` argument added:
```shell
        vim -r help.txt
```
Vim will read the swap file (used to store text you were editing) and may read bits and pieces of the original file. If Vim recovered your changes you will see these messages (with different file names, of course):
```text
        Using swap file ".help.txt.swp"
        Original file "~/vim/runtime/doc/help.txt"
        Recovery completed. You should check if everything is OK.
        (You might want to write out this file under another name
        and run diff with the original file to check for changes)
        You may want to delete the .swp file now.
```
To be on the safe side, write this file under another name:
```text
        :write help.txt.recovered
```
Compare the file with the original file to check if you ended up with what you expected. Vimdiff is very useful for this [08.7](../usr_08#087-viewing-differences-with-vimdiff). For example:
```text
        :write help.txt.recovered
        :edit #
        :diffsp help.txt
```
Watch out for the original file to contain a more recent version (you saved the file just before the computer crashed). And check that no lines are missing (something went wrong that Vim could not recover).<br>
If Vim produces warning messages when recovering, read them carefully. This is rare though.

If the recovery resulted in text that is exactly the same as the file contents, you will get this message:
```text
        Using swap file ".help.txt.swp"
        Original file "~/vim/runtime/doc/help.txt"
        Recovery completed. Buffer contents equals file contents.
        You may want to delete the .swp file now.
```
This usually happens if you already recovered your changes, or you wrote the file after making changes. It is safe to delete the swap file now.

It is normal that the last few changes can not be recovered. Vim flushes the changes to disk when you don't type for about four seconds, or after typing about two hundred characters. This is set with the `undatetime` and `updatecount` options. Thus when Vim didn't get a chance to save itself when the system went down, the changes after the last flush will be lost.

If you were editing without a file name, give an empty string as argument:
```shell
        vim -r ""
```
You must be in the right directory, otherwise Vim can't find the swap file.

___
___

## 11.2 Where is the swap file?

Vim can store the swap file in several places. Normally it is in the same directory as the original file. To find it, change to the directory of the file, and use:
```shell
      vim -r
```
Vim will list the swap files that it can find. It will also look in other directories where the swap file for files in the current directory may be located. It will not find swap files in any other directories though, it doesn't search the directory tree.<br>
The output could look like this:
```text
        Swap files found:
           In current directory:
        1.    .main.c.swp
                  owned by: mool   dated: Tue May 29 21:00:25 2001
                 file name: ~mool/vim/vim6/src/main.c
                  modified: YES
                 user name: mool   host name: masaka.moolenaar.net
                process ID: 12525
           In directory ~/tmp:
              -- none --
           In directory /var/tmp:
              -- none --
           In directory /tmp:
              -- none --
```
If there are several swap files that look like they may be the one you want to use, a list is given of these swap files and you are requested to enter the number of the one you want to use. Carefully look at the dates to decide which one you want to use.<br>
In case you don't know which one to use, just try them one by one and check the resulting files if they are what you expected.

### USING A SPECIFIC SWAP FILE

If you know which swap file needs to be used, you can recovering by giving the swap file name. Vim will then find out the name of the original file from the swap file.

Example:
```shell
        vim -r .help.txt.swo
```
This is also handy when the swap file is in another directoey than expected. Vim recognizes files with the pattern `*.s[uvw][a-z]` as swap files.

If this still does not work, see what file names Vim reports and rename the files accordingly. Check the [directory](../../../options#directory-dir) option to see where Vim may have put the swap file.

Note:<br>
Vim tries to find the swap file by searching the directories in the [dir](../../../options#directory-dir) option, looking for files that match `filename.sw?`. If wildcard expansion doesn't work (e.g., when the [shell](../../../options#shell-sh-e91) option is invalid), Vim does a desperate try to find the file `filename.swp`. If that fails too, you will have to give the name of the swapfile itself to be able to recover the file.

___
___

## 11.3 Crashed or not?

### ATTENTION E325

Vim tries to protect you from doing stupid things. Suppose you innocently start editing a file, expecting the contents of the file to show up. Instead, Vim produces a very long message:
```text
                E325: ATTENTION
        Found a swap file by the name ".main.c.swp"
                  owned by: mool   dated: Tue May 29 21:09:28 2001
                 file name: ~mool/vim/vim6/src/main.c
                  modified: no
                 user name: mool   host name: masaka.moolenaar.net
                process ID: 12559 (still running)
        While opening file "main.c"
                     dated: Tue May 29 19:46:12 2001

        (1) Another program may be editing the same file.
            If this is the case, be careful not to end up with two
            different instances of the same file when making changes.
            Quit, or continue with caution.

        (2) An edit session for this file crashed.
            If this is the case, use ":recover" or "vim -r main.c"
            to recover the changes (see ":help recovery").
            If you did this already, delete the swap file ".main.c.swp"
            to avoid this message.
```
You get this message, because, when starting to edit a file, Vim checks if a swap file already exists for that file. If there is one, there must be something wrong. It may be one of these two situations.

1. Another edit session is active on this file. Look in the message for the line with `process ID`. It might look like this:
```text
                process ID: 12559 (still running)
```
The text ***(still running)*** indicates that the process editing this file runs on the same computer. When working on a non-Unix system you will not get this extra hint. When editing a file over a network, you may not see the hint, because the process might be running on another computer. In those two cases you must find out what the situation is yourself.<br>
If there is another Vim editing the same file, continuing to edit will result in two versions of the same file. The one that is written last will overwrite the other one, resulting in loss of changes. You better quit this Vim.

2. The swap file might be the result from a previous crash of Vim or the computer. Check the dates mentioned in the message. If the date of the swap file is newer than the file you were editing, and this line appers:
```text
                modified: YES
```
Then you very likely have a crashed edit session that is worth recovering.<br>
If the date of the file is newer than the date of the swap file, then either it was changed after the crash (perhaps you recovered it earlier, but didn't delete the swap file?), or else the file was saved before the crash but after the last write of the swap file (then you're lucky: you don't even need that old swap file). Vim will warn you for this with this extra line:
```text
      NEWER than swap file!
```

Note that in the following situation Vim knows the swap file is not useful and will automatically delete it:
- The file is a valid swap file (Magic number is correct).
- The flag that the file was modified is not set.
- The process is not running.

You can programmatically deal with this situation with the [FileChangedShell](../../../autocmd#filechangedshell) autocommand event.

### UNREADABLE SWAP FILE

Sometimes the line
```text
        [cannot be read]
```
will appear under the name of the swap file. This can be good or bad, depending on circumstances.

It is good if a previous editing session crashed without having made any changes to the file. Then a directory listing of the swap file will show that it has zero bytes. You may delete it and proceed.

It is slightly bad if you don't have read permission for the swap file. You may want to view the file read-only, or quit. On multi-user systems, if you yourself did the last changes under a different login name, a logout followed by a login under that other name might cure the ***read error***. Or else you might want to find out who last edited (or is editing) the file and have a talk with them.

It is very bad if it means there is a physical read error on the disk containing the swap file. Fortunately, this almost never happens. You may want to view the file read-only at first (if you can), to see the extent of the changes that were ***forgotten***. If you are the one in charge of that file, be prepared to redo your last changes.

### swap-exists-choices

WHAT TO DO?

If dialogs are supported you will be asked to select one of six choices:
```text
  Swap file ".main.c.swp" already exists!
  [O]pen Read-Only, (E)dit anyway, (R)ecover, (Q)uit, (A)bort, (D)elete it:
```

- ***O*** Open the file readonly. Use this when you just want to view the file and don't need to recover it. You might want to use this when you know someone else is editing the file, but you just want to look in it and not make changes.
- ***E*** Edit the file anyway. Use this with caution! If the file is being edited in another Vim, you might end up with two versions of the file. Vim will try to warn you when this happens, but better be safe than sorry.
- ***R*** Recover the file from the swap file. Use this if you know that the swap file contains changes that you want to recover.
- ***Q*** Quit. This avoids starting to edit the file. Use this if there is another Vim editing the same file.<br>
When you just started Vim, this will exit Vim. When starting Vim with files in several windows, Vim quits only if there is a swap file for the first one. When using an edit command, the file will not be loaded and you are taken back to the previously edited file.
- ***A*** Abort. Like Quit, but also abort further commands. This is useful when loading a script that edits several files, such as a session with multiple windows.
- ***D*** Delete the swap file. Use this when you are sure you no longer need it. For example, when it doesn't contain changes, or when the file itself is newer than the swap file.<br>
On Unix this choice is noly offered when the process that created the swap file does not appear to be running.

If you do not get the dialog (you are running a version of Vim that does not support it), you will have to do it manually. To recover the file, use this command:
```text
        :recover
```
Vim cannot always detect that a swap file already exists for a file. This is the case when the other edit session puts the swap files in another directory or when the path name for the file is different when editing it on different machines. Therefore, don't rely on Vim always warning you.

If you really don't want to see this message, you can add the `A` flag to the [shortmess](../../../options#shortmess-shm) option. But it's very unusual that you need this.

For remarks about encryption and the swap file, see [:recover-crypt](../../../recover#recover-crypt).<br>
For programmatic access to the swap file, see [swapinfo()](../../../builtin#swapinfo).

___
___

## 11.4 Further reading

- [swap-file](../../../recover#swap-file)  An explanation about where the swap file will be created and what its name is.
- [:preserve](../../../recover#pre-preserve-e313-e314)  Manually flushing the swap file to disk.
- [:swapname](../../../recover#sw-swapname)  See the name of the swap file for the current file.
- [updatecount](../../../options#updatecount-uc)   Number of key strokes after which the swap file is flushed to disk.
- [updatetime](../../../options#updatetime-ut)   Timeout after which the swap file is flushed to disk.
- [swapsync](../../../options#swapsync-sws)   Whether the disk is synced when the swap file is flushed.
- [directory](../../../options#directory-dir)   List of directory names where to store the swap file.
- [maxmem](../../../options#maxmem-mm)   Limit for memory usage before writing text to the swap file.
- [maxmemtot](../../../options#maxmemtot-mmt)   Same, but for all files in total.

___
___

Next chapter: [usr_12.txt  Clever tricks](../usr_12)