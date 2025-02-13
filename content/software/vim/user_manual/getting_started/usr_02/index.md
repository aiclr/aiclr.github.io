+++
title = 'usr_02 The first steps in Vim'
date = '2025-02-07'
author = 'aiclr'
categories = ['vim']
tags = ['vim','help','user manual','getting started']
Summary=':helo usr_02 This chapter provides just enough information to edit a file with Vim. Notwell or fast, but you can edit. Take some time to practice with these commands, they form the base for what follows.'
+++

This chapter provides just enough information to edit a file with Vim.  Not well or fast, but you can edit.  Take some time to practice with these commands, they form the base for what follows.

- [02.1    Running Vim for the First Time](#021-running-vim-for-the-first-time)
- [02.2    Inserting text](#022-inserting-text)
- [02.3    Moving around](#023-moving-around)
- [02.4    Deleting characters](#024-deleting-characters)
- [02.5    Undo and Redo](#025-undo-and-redo)
- [02.6    Other editing commands](#026-other-editing-commands)
- [02.7    Getting out](#027-getting-out)
- [02.8    Finding help](#028-finding-help)

Next chapter: [usr_03](../usr_03)  Moving around<br/>
Previous chapter: [usr_01](../usr_01)  About the manuals<br/>
Table of contents: [usr_toc](../../usr_toc)

___
___

## 02.1 Running Vim for the First Time

To start Vim, enter this command: `gvim file.txt`. In UNIX you can type this at any command prompt. If you are running Mocrosoft Windows, open a Command Prompt and enter the command. In either case, Vim starts editing a file called file.txt. Because this is a new file, you get a blank window. This is what your screen will look like:
```text
        +---------------------------------------+
        |#                                      |
        |~                                      |
        |~                                      |
        |~                                      |
        |~                                      |
        |"file.txt" [New file]                  |
        +---------------------------------------+
                ('#' is the cursor position.)
```
The tilde(~) lines indicate lines not in the file. In other words, when Vim runs out of file to display, it displays tilde lines. At the bottom of the screen, a message line indicates the file is named file.txt and shows that you are creating a new file. The message information is temporary and other information overwrites it.

### THE VIM COMMAND

The gvim command causes the editor to create a new window for editing. If you use this command:
```text
vim file.txt
```
the editing occurs inside your command window. In other words, if you are running inside an xterm, the editor uses your xterm window. If you are using as MS-Windows command prompt window, the editing occurs inside this window. The text in the window will look the same for both versions, but with gvim you have extra features, like a menu bar. More about that later.

___
___

## 02.2 Inserting text

The Vim editor is a modal editor. That means that the editor behaves differently, depending on which mode you are in. The two basic modes are called **Normal mode** and **Insert mode**. In Normal mode the characters you type are commands. In Insert mode the characters are inserted as text.

Since you have just started Vim it will be in Normal mode. To start Insert mode you type the `i` command(i for Insert). Then you can enter the text. It will be inserted into the file. Do not worry if you make mistakes; you can correct then later. To enter the following programmer's limerick, this is what you type:
```text
iA very intelligent turtle
Found programming UNIX a hurdle
```
After typing **turtle** you press the `<Enter>`key to start a new line. Finally you press the `<Esc>` key to stop Insert mode and go back to Normal mode. You now have two lines of text in your Vim window:
```text
        +---------------------------------------+
        |A very intelligent turtle              |
        |Found programming UNIX a hurdle        |
        |~                                      |
        |~                                      |
        |                                       |
        +---------------------------------------+
```

### WHAT IS THE MODE?

To be able to see what mode you are in, type this command:
```text
:set showmode
```
You will notice that when typing the colon Vim moves the cursor to the last line of the window. That's where you type colon commands (commands that start with a colon). Finish this command by pressing the `<Enter>` key (all commands that start with a colon are finished this way).

Now, if you type the `i` command Vim will display **--INSERT--** at the bottom of the window. This indicates you are in Insert mode.
```text
        +---------------------------------------+
        |A very intelligent turtle              |
        |Found programming UNIX a hurdle        |
        |~                                      |
        |~                                      |
        |-- INSERT --                           |
        +---------------------------------------+
```
If you press `<Esc>` to go back to Normal mode the last line will be made blank.

### GETTING OUT OF TROUBLE

One of the problems for Vim novices is mode confusion, which is caused by forgetting which mode you are in or by accidentally typing a command that switches modes. To get back to Normal mode, no matter what mode you are in, press the `<Esc>` key. Sometimes you have to press it twice. If Vim beeps back at you, you already are in Normal mode.

___
___

## 02.3 Moving around

After you return to Normal mode, you can move around by using these keys:

| | | |
|---|---|---|
|h|left|左|
|j|down|下|
|k|up|上|
|l|right|右|

At first, it may appear that these commands were chosen at random. After all, who ever heard of using `l` for right? But actually, these is a very good reason for these choices: Moving the cursor is the most common thing you do in an editor, and these keys are on the home row of your right hand. In other words, these commands are placed where you can type them the fastest (especially when you type with ten fingers).

### Note

You can alse move the cursor by using the arrow keys. If you do, however, you greatly slow down your editing because to press the arrow keys, you must move your hand from the text keys to the arrow keys. Considering that you might be doing it hundreds of times an hour, this can take a significant amount of time.

Also, there are keyboards which do not have arrow keys, or which locate them in unusual places; therefore, knowning the use of the hjkl keys helps in those situations.

One way to remember these commands is that `h` is on the left, `l` is on the right and `j` points down. In a picture:
```text
                       k
                   h     l
                     j
```
The best way to learn these commands is by using them. Use the **i** command to insert some more lines of text. Then use the hjkl keys to move around and insert a word somewhere. Don't forget to press `<Esc>` to go back to Normal mode. The [vimtutor](../usr_01/#013-using-the-vim-tutor) is also a nice way to learn by doing.

For Japanese users, Hiroshi Iwatani suggested using this:
```text
                        Komsomolsk
                            ^
                            |
           Huan Ho      <--- --->  Los Angeles
        (Yellow river)      |
                            v
                          Java (the island, not the programming language)
```

___
___

## 02.4 Deleting characters

To delete a character, move the cursor over it and type `x`. (This is a throwback to the old days of the typewriter, when you deleted things by typing xxxx over them.) Move the cursor to the beginning of the first line, for example, and type xxxxxxx (seven x's) to delete "A very ". The result should look like this:
```text
        +---------------------------------------+
        |intelligent turtle                     |
        |Found programming UNIX a hurdle        |
        |~                                      |
        |~                                      |
        |                                       |
        +---------------------------------------+
```
Now you can insert new text, for example by typing:
```text
iA young <Esc>
```
This begins an insert (the i), inserts the words **A young**, and then exits insert mode (the final `<Esc>`). The result:
```text
        +---------------------------------------+
        |A young intelligent turtle             |
        |Found programming UNIX a hurdle        |
        |~                                      |
        |~                                      |
        |                                       |
        +---------------------------------------+
```

### DELETING A LINE

To detete a whole line use the `dd` command. The following line will then move up to fill the gap:
```text
        +---------------------------------------+
        |Found programming UNIX a hurdle        |
        |~                                      |
        |~                                      |
        |~                                      |
        |                                       |
        +---------------------------------------+
```

### DELETING A LINE BREAK

In Vim you can join two lines together, which means that the line break between them is deleted. The `J` command does this.
Take these two lines:
```text
        A young intelligent
        turtle
```
Move the cursor to the first line and press `J`:
```text
        A young intelligent turtle
```

___
___

## 02.5 Undo and Redo

Suppose you delete too much. Well, you can type it in again, but an easier way exists. The `u` command undoes the last edit. Take a look at this in action: After using `dd` to delete the first line, `u` brings it back.

Another one: Move the cursor to the **A** in the first line:
```text
        A young intelligent turtle
```
Now type xxxxxxx to delete **A young**. The result is as follows:
```text
         intelligent turtle
```
Type `u` to undo the last delete. That delete removed the **g**, so the undo restores the character.
```text
        g intelligent turtle
```
The next `u` command restores the next-to-last character deleted:
```text
        ng intelligent turtle
```
The next `u` command gives you the **u**, and so on:
```text
        ung intelligent turtle
        oung intelligent turtle
        young intelligent turtle
         young intelligent turtle
        A young intelligent turtle
```
### Note

If you type `u` twice, and the result is that you get the same text back, you have Vim configured to work **Vi compatible**. Look here to fix this: [not-compatible](../usr_01/#not-compatible).

This text assumes you work **The Vim Way**. You might prefer to use the good old **Vi way**, but you will have to watch out for small differences in the text then.

### REDO

If you undo too many times, you can press `CTRL-R` (redo) to reverse the preceding command. In other words, it undoes the undo. To see this in action, press `CTRL-R` twice. The character **A** and the space after it disappear:
```text
        young intelligent turtle
```
There's a special version of the undo command, the `U` (undo line) command.The undo line command undoes all the changes made on the last line that was edited. Typing this command twice cancels the preceding `U`.
```text
        A very intelligent turtle
          xxxx                          Delete very

        A intelligent turtle
                      xxxxxx            Delete turtle

        A intelligent
                                        Restore line with "U"
        A very intelligent turtle
                                        Undo "U" with "u"
        A intelligent
```
The `U` command is a change by itself, which the `u` command undoes and `CTRL-R` redoes. This might be a bit confusing. Don't worry, with `u` and `CTRL-R` you can go to any of the situations you had. More about that in section [32.2](../../editing_effectively/usr_32/#322-numbering-changes)

___
___

## 02.6 Other editing commands

Vim has a large number of commands to change the text. See [Q_in](.) and below.

Here are a few often used ones.

### APPENDING

The `i` command inserts a character before the character under the cursor.

That works fine; but what happens if you want to add stuff to the end of the line? For that you need to insert text after the cursor. This is done with the `a` (append) command.

For example, to change the line
```text
        and that's not saying much for the turtle.
```
to
```text
        and that's not saying much for the turtle!!!
```
move the cursor over to the dot at the end of the line. Then type `x` to delete the period. The cursor is now positioned at the end of the line on the **e** in turtle. Now type
```text
        a!!!<Esc>
```
to append three exclamation points after the **e** in turtle:
```text
        and that's not saying much for the turtle!!!
```

### OPENING UP A NEW LINE

The `o` command creates a new, empty line below the cursor and puts Vim in Insert mode. Then you can type the text for the new line.

Suppose the cursor is somewhere in the first of these two lines:
```text
        A very intelligent turtle
        Found programming UNIX a hurdle
```
If you now use the `o` command and type new text:
```text
        oThat liked using Vim<Esc>
```
The result is:
```text
        A very intelligent turtle
        That liked using Vim
        Found programming UNIX a hurdle
```
The `O` command (uppercase) opens a line above the cursor.

### USING A COUNT

Suppose you want to move up nine lines. You can type `kkkkkkkkk` or you can enter command `9k`. In fact, you can precede many commands with a number. Earlier in this chapter, for instance, you added three exclamation points to the end of a line by typing `a!!!<Esc>`. Another wat to do this is to use the command `3a!<Esc>`. Thr count of **3** tells the command that follows to triple its effect. Similarly, to delete three characters, use the command `3x`. The count always comes before the command it applies to.

___
___

## 02.7 Getting out

To exit, use the `ZZ` command. This command writes the file and exits.

### Note:

Unlike many other editors, Vim does not automatically make a backup file. If you type `ZZ`, your changes are committed and there's no turning back. You can configure the Vim editor to produce backup files;see [07.4](../usr_07/#074-backup-files).

### DISCARDING CHANGES

Sometimes you will make a sequence of changes and suddenly realize you were better off before you started. Not to worry; Vim has a quit-and-throw-things-away command. It is:
```text
:q!
```
Don't forget to press `<Enter>` to finish the command.

For those of you interested in the details, the three parts of this command are the colon (:), which enters Command-line mode; the **q** command, which tells the editor to quit; and the override command modifier (!).

The override command modifier is needed because Vim is reluctant to throw away changes. If you were to just type `:q`, Vim would display an error message and refuse to exit:
```text
        E37: No write since last change (use ! to override)
```
By specifying the override, you are in effect telling Vim, "I know that what I'm doing looks stupid, but I really want to do this."

If you want to continue editing with Vim: The `:e!` command reloads the original version of the file.

___
___

## 02.8 Finding help

Everything you always wanted to know can be found in the Vim help files. Don't be afraid to ask!

If you know what you are looking for, it is usually easier to search for it using the help system, instead of using Google. Because the subjects follow a certain style guide.

Also the help has the advantage of belonging to your particular Vim version.

You won't see help for commands added later. These would not work for you.

To get generic help use this command
```text
        :help
```
You could alse use the first function key `<F1>`. If you keybord has a `<Help>` key it might work as well.

If you don't supply a subject, `:help` displays the general help window. The creators of Vim did something very clever (or very lazy) with the help system: They made the help window a normal editing window. You can use all the normal Vim commands to move through the help information. Therefore h, j, k, and l move left, down, up and right.

To get out of the help window, use the same command you use to get out of the editor: `ZZ`. This will only close the help window, not exit Vim.

As you read the help text, you will notice some text enclosed in vertical bars (for example, **help**). This indicates a hyperlink. If you position the cursor anywhere between the bars and press `CTRL-]` (jump to tag), the help system takes you to the indicated subject. (For reasons not discussed here, the Vim terminology for a hyperlink is tag. So `CTRL-]` jumps to the location of the tag given by the word under the cursor.)

After a few jumps, you might want to go back. `CTRL-T` (pop tag) takes you back to the preceding position. `CTRL-O` (jump to older position) also works nicely here.

At the top of the help screen, there is the notation `*help.txt*`. This name between `*` characters is used by the help system to define a tag (hyperlink destination).

See [29.1](../../editing_effectively/usr_29/#291-using-tags) for details about using tags.

To get help on a given subject, use the following command:
```text
        :help {subject}
```
To get help on the `x` command, for example, enter the following:
```text
        :help x
```
To find out how to delete text, use this command:
```text
        :help deleting
```
To get a complete index of all Vim commands, use the following command:
```text
        :help index
```
When you need to get help for a control character command (for example, `CTRL-A`), you need to spell it with the prefix **CTRL-**.
```text
        :help CTRL-A
```
The Vim editor has many different modes. By default, the help system displays the normal-mode commands. For example, the following command displays help for the normal-mode `CTRL-H` command:
```text
        :help CTRL-H
```
To identify other modes, use a mode prefix. If you want the help for the insert-mode version of a command, use **i_**. For `CTRL-H` this gives you the following command:
```text
        :help i_CTRL-H
```
When you start the Vim editor, you can use several command-line arguments. These all begin with a dash (-). To find what the **-t** argument does, for example, use the command:
```text
        :help -t
```
The Vim editor has a number of options that enable you to configure and customize the editor. If you want help for an option, you need to enclose it in single quotation marks To find out what the **number** option does, for example, use the following command:
```text
        :help 'number'
```
The table with all mode prefixes can be found below: [help-summary](#)

Special keys are enclosed in angle brackets. To find help on the up-arrow key in Insert mode, for instance, use this command:
```text
        :help i_<Up>
```
If you see an error message that you don't understand, for example:
```text
        E37: No write since last change (use ! to override)
```
You can use the error ID at the start to find help about it:
```text
        :help E37
```

### help-summary

1. Use `Ctrl-D` after typing a topic and let Vim show all avaiable topics.
Or press Tab to complete:
```text
        :help some<Tab>
```
More information on how to use the help:
```text
        :help helphelp
```
2. Follow the links in bars to related help. You can go from the detailed help to the user documentation, which describes certain commands more from a user perspective and less detailed. E.g. after:
```text
        :help pattern.txt
```
You can see the user guide topics [03.9](../usr_03/#039-simple-search-patterns) and [usr_27](../../editing_effectively/usr_27) in the introduction.

3. Options are enclosed in single apostrophes. To go to the help topic for the list option:
```text
       :help 'list'
```
If you only know you are looking for a certain option, you can also do:
```text
        :help options.txt
```
to open the help page which describes all option handling and then search using regular expressions, e.g. textwidth.
<br/>
Certain options have their own namespace, e.g.:
```text
        :help cpo-<letter>
```
for the corresponding flag of the ***cpoptions*** settings, substitute `<letter>` by a specific flag, e.g.:
```text
        :help cpo-<letter>
```
And for the ***guioptions*** flags:
```text
        :help go-<letter>
```

4. Normal mode commands do not have a prefix. To go to the help page for the **gt** command:
```text
        :help gt
```

5. Insert mode commands start with **i_**. help for deleting a word:
```text
        :help i_CTRL-W
```

6. Visual mode commands start with **v_**. Help for jumping to the other side of the Visual area:
```text
        :help v_o
```

7. Command line editing and arguments start with **c_**. Help for using the command argument **%**:
```text
        :help c_%
```

8. Ex-commands always start with **:**, so to go to the `:s` command help:
```text
        :help :s
```

9. COmmands specifically for debugging start with **>**. To go to the help for the **cont** debug command:
```text
        :help >cont
```

10. Key combinations. They usually start with a single letter indicating the mode for which they can be used. E.g.:
```text
        :help i_CTRL-X
```
takes you to the family of `CTRL-X` commands for insert mode which can be used to auto-complete different things. Note, that certain keys will always be written the same, e.g. Control will always be `CTRL`.
<br/>
For normal mode commands there is no prefix and the topic is available at `:h CTRL-<Letter>`. E.g.
```text
        :help CTRL-W
```
In contrast
```text
        :help c_CTRL-R
```
will describe what the `CTRL-R` does when entering commands in the Command line and
```text
        :help v_CTRL-A
```
talks about incrementing numbers in visual mode and
```text
        :help g_CTRL-A
```
talks about the `g<C-A>` command (e.g. you have to press **g** then `<CTRL-A>`). Here the **g** stands for the normal command **g** which always expects a second key before doing something similar to the commands starting with **z**.

11. Regexp items always start with `/.` So to get help for the `\+` quantifier in Vim regexes:
```text
        :help /\+
```
If you need to know everything about regular expressions, start reading at:
```text

```

12. Registers always start with ***quote***. To find out about the special ***:*** register:
```text

```

13. Vim script is available at
```text
        :help eval.txt
```
Certain aspects of the language are available at `:h expr-X` where ***X*** is a single letter. E.g.
```text
        :help expr-!
```
will take you to the topic describing the ***!*** (Not) operator for Vim script.
<br/>
Also important is
```text
        :help function-list
```
to find a short description of all functions available. Help topics for Vim script functions always include the `()`, so:
```text
        :help append()
```
talks about the append Vim script function rather than how to append text in the current buffer.

14. Mapping are talked about in the help page `:h map.txt`. Use
```text
        :help mapmode-i
```
to find out about the `:imp` command. Also use `:map-topic`<br/>
to find out about certain subtopics particular for mapping. e.g:
```text
        :help :map-local
```
for buffer-local mapping or
```text
        :help map-bar
```
for how the `|` is handled in mappings.

15. Command definitions are talked about `:h command-topic`, so use
```text
        :help command-bar
```
to find out about the `!` argument for custom commands.

16. Window management commands always start with `CTRL-W`, so you find the corresponding help at `:h CTRL-W_letter`. E.g.
```text
        :help CTRL-W_p
```
for moving the previous accessed window. You can also access
```text
        :help windows.txt
```
and read your way through if you are looking for window handling commands.

17. Use `:helpgrep` to search in all help pages (and also of any installed plugins). See `:helpgrep` for how to use it.<br/>
To search for a topic:
```text
        :helpgrep topic
```
This takes you to the first match. To go to the next one:
```text
        :cnext
```
All matches are available in the quickfix window which can be opened with:
```text
        :copen
```
Move around to the match you like and press Enter to jump to that help.

18. The user manual. This describes help topics for beginners in a rather friendly way. Start at [usr_toc.txt](../../usr_toc) to find the table of content (as you might have guessed):
```text
        :help usr_toc.txt
```
Skim over the contents to find interesting topics. The **Digraphs** and "Entering special characters" items are in chapter 24, so to go to that particular help page:
```text
        :help usr_24.txt
```
Also if you want to access a certain chapter in the help, the chapter number can be accessed directly like this:
```text
        :help 10.1
```
which goes to chapter [10.1](../usr_10/#101-record-and-playback-commands) in [usr_10.txt](../usr_10) and talks about recording macros.

19. Highlighting groups. Always start with hl-groupname. E.g.
```text
        :help hl-WarningMsg
```
talks about the WarningMsg highlighting group.

20. Syntax highlighting is namespaced to `:syn-topic`. E.g.
```text
        :help :syn-conceal
```
talks about the conceal argument for the `:syn` command.

21. Quickfix commands usually start with `:c` while location list commands usually start with `:l`
22. Autocommand events can be found by their name:
```text
        :help BufWinLeave
```
To see all possible events:
```text
        :help autocommand-events
```

23. Command-line switches always start with `-`. SO for the help of the `-f` command switch of Vim use:
```text
        :help -f
```

24. Optional features always start with `+`. To find out about the conceal feature use:
```text
        :help +conceal
```

25. Documentation for included filetype specific functionality is usually available in the form `ft-<filetype>-<functionality>`. So
```text
        :help ft-c-syntax
```
talks about the **C** syntax file and the option it provides. Sometimes, additional sections for omni completion
```text
        :help ft-php-omni
```
or filetype plugins
```text
        :help ft-tex-plugin
```
are available.

26. Error and Warning codes can be looked up directly in the help. So
```text
        :help E297
```
takes you exactly to the description of the swap error message and
```text
        :help W10
```
talks about the warning ***Changing a readonly file***.<br/>
SomeTimes, however, those error codes are not described, but rather are listed at the Vim command that usually causes this. So:
```text
        :help E128
```
takes you to the `:function` command

___
***

Next chapter: [usr_03](../usr_03)  Moving around