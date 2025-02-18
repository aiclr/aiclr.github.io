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
- [03.4    Matching a paren](#034-matching-a-parenthesis)
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

The `CTRL-U` command scrolls down half a screen of text. Think of looking through a viewing window at the text and moving this window up by half the height of the window. Thus the window moves up over the text, which is backward in the file. Don't worry if you have a little trouble remembering which end is up. Most users have the same problem.<br>
The `CTRL_D` command moves the viewing window down half a screen in the file, thus scrolls the text up half a screen.
```text
                                       +----------------+
                                       | some text      |
                                       | some text      |
                                       | some text      |
        +---------------+              | some text      |
        | some text     |  CTRL-U  --> |                |
        |               |              | 123456         |
        | 123456        |              +----------------+
        | 7890          |
        |               |              +----------------+
        | example       |  CTRL-D -->  | 7890           |
        +---------------+              |                |
                                       | example        |
                                       | example        |
                                       | example        |
                                       | example        |
                                       +----------------+
```
To scroll one line at a time use `CTRL-E` (scroll up) and `CTRL-Y` (scroll down).<br>
Think of `CTRL-E` to give you one line Extra. (If you use MS-Windows compatible key mappings `CTRL-Y` will redo a change instead of scroll.)

To scroll forward by a whole screen (except for two lines) use `CTRL-F`. To scroll backwards, use `CTRL-B`. These should be easy to remember: ***F*** for forwards and ***B*** for Backwards.

A common issue is that after moving down many lines with `j` your cursor is at the bottom of the screen. You would like to see the context of the line with the cursor. That's done with the `zz` command.
```text
        +------------------+             +------------------+
        | earlier text     |             | earlier text     |
        | earlier text     |             | earlier text     |
        | earlier text     |             | earlier text     |
        | earlier text     |   zz  -->   | line with cursor |
        | earlier text     |             | later text       |
        | earlier text     |             | later text       |
        | line with cursor |             | later text       |
        +------------------+             +------------------+
```
The `zt`command put the cursor line at the top, `zb` at the bottom. There are a few more scrolling commands, see [Q_sc](../../../quickref#q_sc-scrolling). To always keep a few lines of context around the cursor, use the `scrolloff` option.

___
___

## 03.8 Simple searches

To search for a ***string***, use the `/string` command. To find the word ***include***, for example, use the command:
```text
        /include
```
You will notice that when you type the `/` the cursor jumps to the last line of the Vim window, like with colon commands. That is where you type the word. You can press the backspace key (backarrow or `<BS>`) to make corrections. Use the `<Left>` and `<Right>` cursor keys when necessary.<br>
Pressing `<Enter>` executes the command.

Note:<br>
The characters `.*[]^%/\?~$` have special meanings. If you want to use them in a search you must put a `\` in front of them. See below.

To find the next occurrence of the same string use the `n` command. Use this to find the first ***#include*** after the cursor:
```text
        /#include
```
And then type `n` several times. You will move to each ***#include*** in the text.<br>
You can also use a count if you know which match you want. Thus `3n` finds the third match. You can also use a count with `/`: `4/the` goes to the fourth match of ***the***.

The `?` command works like `/` but searches backwards:
```text
        ?word
```
The `N` command repeats the last search the opposite direction. Thus using `N` after a `/` command searches backwards, using `N` after `?` searches forwards.

### IGNORING CASE

Normally you have to type exactly what you want to find. If you don't care about upper or lowercase in a word, set the `ignorecase` option:
```text
        :set ignorecase
```
If you now search for ***word***, it will also match ***Word*** and ***WORD***. To match case again:
```text
        :set noignorecase
```

### HISTORY

Suppose you do three searches:
```text
        /one
        /two
        /three
```
Now let's start searching by typing a simple `/` without pressing `<Enter>`. If you press `<Up>` (the cursor key), Vim puts `/three` on the command line. Pressing `<Enter>` at this point searches for three. If you do not press `<Enter>`, but press `<Up>` instead, Vim changes the prompt to `/two`. Another press of `<Up>` moves you to `/one`.<br>
You can also use the `<Down>` cursor key to move through the history of search commands in the other direction.

If you know what a previously used pattern starts with, and you want to use it again, type that character before pressing `<Up>`. With the previous example, you can type `/o<Up>` and Vim will put `/one` on the command line.

The commands starting with `:` also have a history. That allows you to recall a previous command and execute it again. These two histories are separate.

### SEARCHING FOR A WORD IN THE TEXT

Suppose you see the word ***TheLongFunctionName*** in the text and you want to find the next occurrence of it. You could type `/TheLongFunctionName`, but that's a lot of typing. And when you make a mistake Vim won't find it.<br>
There is an easier way: Position the cursor on the word and use the `*` command. Vim will grab the word under the cursor and use it as the search string.<br>
The `#` command does the same in the other direction. You can prepend a count: `3*` searches for the third occurrence of the word under the cursor.

### SEARCHING FOR WHOLE WORDS

If you type `/the` it will also match ***there***. To only find words that end in ***the*** use:
```text
        /the\>
```
The `\>` item is a special marker that only matches at the end of a word.<br>
Similarly `\<` only matches at the begining of a word. Thus to search for the word ***the*** only:
```text
        /\<the\>
```
This does not match ***there*** or ***soothe***. Notice that the `*` and `#` commands use these start-of-word and end-of-word markers to only find whole words (you can use `g*` and `g#` to match partial words).

### HIGHLIGHTING MATCHES

While editing a program you see a variable called ***nr***. You want to check where it's used. You could move the cursor to ***nr*** and use the `*` command and press `n` to go along all the matches.<br>
There is another way. Type this command:
```text
        :set hlsearch
```
If you now search for ***nr***, Vim will highlight all matches. That is a very good way to see where the variable is used, without the need to type commands.<br>
To switch this off:
```text
        :set nohlsearch
```
Then you need to switch it on again if you want to use it for the next search command. If you only want to remove the highlighting, use this command:
```text
        :nohlsearch
```
This doesn't reset the option. Instead, it disables the highlighting. As soon as you execute a search command, the highlighting will be used again. Also for the `n` and `N` commands.

### TUNING SEARCHES

There are a few options that change how searching works. These are the essential ones:
```text
        :set incsearch
```
This makes Vim display the match for the string while you are still typing it. Use this to check if the right match will be found. Then press `<Enter>` to really jump to that location. Or type more to change the search string.
```text
        :set nowrapscan
```
This stops the search at the end of the file. Or, when you are searching backwards, it stops the search at the start of the file. The `wrapscan` option is on by default, thus searching wraps around the end of the file.

### INTERMZZO

If you like one of the options mentioned before, and set it each time you use Vim, you can put the command in your Vim startup file.<br>
Edit the file, as mentioned at [not-compatible](../usr_01#not-compatible). Or use this command to find out where it is:
```text
        :scriptnames
```
Edit the file, for example with:
```text
        :edit ~/.vimrc
```
Then add a line with the command to set the option, just like you typed it in Vim. Example:
```text
        Go:set hlsearch<Esc>
```
`G` Moves to the end of the file. `o` starts a new line, where you type the `:set` command. You end insert mode with `<Esc>`. Then write and close the file:
```text
        ZZ
```
If you now start Vim again, the `hlsearch` option will already be set.

___
___

## 03.9 Simple search patterns

The Vim editor uses regular expressions to specify what to search for.<br>
Regular expressions are an extremely powerful and compact way to specify a search pattern. Unfortunately, this power comes at a price, because regular expressions are a bit tricky to specify.<br>
In this section we mention only a few essential ones. More about search patterns and commands can be found in chapter 27 [usr_27.txt](../../editing_effectively/usr_27). You can find the full explanation here: [pattern](../../../pattern#the-definition-of-a-pattern)

### BEGINING AND END OF A LINE

The `^` character matches the begining of a line. On an English-US keyboard you find it above the `6`. The pattern ***include*** matches the word include anywhere on the line. But the pattern `^include` matches the word include only if it is at the begining of a line.<br>
The `$` character matches the end of a line. Therefore, `was$` matches the word was only if it is at the end of a line.

Let's mark the places where `/the` matches in this example line with **x**s:
```text
        the solder holding one of the chips melted and the
        xxx                       xxx                  xxx
```
Using `/the$` we find this match:
```text
        the solder holding one of the chips melted and the
                                                       xxx
```
And with `/^the` we find this one:
```text
        the solder holding one of the chips melted and the
        xxx
```
You can try searching with `/^the$`; it will only match a single line consisting entirely of ***the***. White space does matter here, thus if a line contains a space after the word, like `the `, the pattern will not match.

### MATCHING ANY SINGLE CHARACTER

The `.` (dot) character matches any existing character. For example, the pattern `c.m` matches a string whose first character is a `c`, whose second character is anything, and whose third character is `m`. Example:
```text
        We use a computer that became the cummin winter.
                 xxx             xxx      xxx
```

### MATCHING SPECIAL CHARACTERS

If you really want to match a dot, you must avoid its special meaning by putting a backslash before it.<br>
If you search for `the.`, you will find these matches:
```text
        We use a computer that became the cummin winter.
                      xxxx                          xxxx
```
Searching for `the\.` only finds the second match.

___
___

## 03.10 Using marks

When you make a jump to a position with the `G` command, Vim remembers the position from before this jump. This position is called a mark. To go back where you came from, use this command:
```text
        ``
```
This ***&#96;*** is a backtick or open single-quote character.<br>
If you use the same command a second time you will jump back again. That's because the ***&#96;*** command is a jump itself, and the position from before this jump is remembered.

Generally, every time you do a command that can move the cursor further than within the same line, this is called a jump. This includes the search commands `/` and `n` (it doesn't matter how far away the match is). But not the character searches with `fx` and `tx` or the word movements `w` and `e`.<br>
Also, `j` and `k` are not considered to be a jump, even when you use a count to make them move the cursor quite a long way away.

The ***&#96;&#96;*** command jumps back and forth, between two points. The `CTRL-O` command jumps to older positions (Hint: O for older). `CTRL-I` then jumps back to newer positions (Hint: for many common keyboard layouts, `I` is just next to `O`).Consider this sequence of commands:
```text
        33G
        /^The
        CTRL-O
```
You first jump to line 33, then search for a line that starts with ***The***.Then with `CTRL-O` you jump back to line 33. Another `CTRL-O` takes you back to where you started. If you now use `CTRL-I` you jump to line 33 again. And to the match for ***The*** with another `CTRL-I`.
```text
             |  example text   ^             |
        33G  |  example text   |  CTRL-O     | CTRL-I
             |  example text   |             |
             V  line 33 text   ^             V
             |  example text   |             |
       /^The |  example text   |  CTRL-O     | CTRL-I
             V  There you are  |             V
                example text
```
Note:<br>
`CTRL-I` is the same as `<Tab>`.

The `:jumps` command givesa list of positions you jumped to. The entry which you used last is marked with a `>`.

### NAMED MARKS

Vim enables you to place your own marks in the text. The command `ma` marks the place under the cursor as mark ***a***. You can place 26 marks (a through ***z***) in your text. You can't see them, it's just a position that Vim remembers.<br>
To go to a mark, use the command ***&#96;{mark}***, where `{mark}` is the mark letter. Thus to move to the ***a*** mark:
```text
        `a
```
The command ***&#96;mark*** (single quotation mark, or apostrophe) moves you to the begining of the line containing the mark. This differs from the ***&#96;mark*** command, which also moves you to the marked column.

The marks can be very useful when working on two related parts in a file. Suppose you have some text near the start of the file you need to look at, while working on some text near the end of the file.<br>
Move to the text at the start and place the s (start) mark there:
```text
        ms
```
Then move to the text you want to work on and put the e (end) mark there:
```text
        me
```
Now you can move around, and when you want to look at the start of the file, you use this to jump there:
```text
       's
```
Then you can use ***''*** to jump back to where you were, or ***'e*** to jump to the text you were working on at the end.<br>
There is nothing special about using s for start and e for end, they are just easy to remember.

You can use this command to get a list of marks:
```text
        :marks
```
You will notice a few special marks. These include:
```text
        '       The cursor position before doing a jump
        "       The cursor position when last editing the file
        [       Start of the last change
        ]       End of the last change
```

___
___

Next chapter: [usr_04.txt](../usr_04)  Making small changes