+++
title = 'Emacs'
date = '2024-06-08'
author = 'aiclr'
categories = ['software']
tags = ['emacs','editor']
Summary='Emacs is an extensible, customizable, self-documenting real-time display editor.'
+++

|key|desc|
|---|---|
|`C-<chr>`|hold the `CONTROL` key while typing the character `<chr>`|
|`M-<chr>`|hold the `META` or `ALT` key down while typing `<chr>`.If there is no `META` or `ALT` key, instead press and release the `ESC` key and then type `<chr>`.  We write `<ESC>` for the `ESC` key.|
|`<DEL>`|This is the key on the keyboard usually labeled "Backspace"--the same one you normally use, outside Emacs, to delete the last character typed.|
|`<Delete>`|There is usually another key on your keyboard labeled `<Delete>`, but that's not the one we refer to as `<DEL>` in Emacs.|
|`<SPC>`|the Space bar|
|`<Return>`|this is the key on the keyboard which is sometimes labeled "Enter"|
| ***IF EMACS STOPS RESPONDING*** |
|C-g|If Emacs stops responding to your commands, you can stop it safely by typing `C-g`.  You can use `C-g` to stop a command which is taking too long to execute. You can also use `C-g` to discard a numeric argument or the beginning of a command that you do not want to finish.|
| ***undo***|
|C-/|undo command|
|C-_|alternative undo command; it works exactly the same as `C-/`. On some text terminals, you can omit the shift key when you type `C-_`. On some text terminals, typing `C-/` actually sends `C-_` to Emacs. Alternatively, `C-x u` also works exactly like `C-/`, but is a little less convenient to type.|
|C-x u|undo command|
| ***del、cut、copy、yank、paste***|
| |Some other editors call killing and yanking "cutting" and "pasting"|
|`<DEL>`|Delete the character just before the cursor|
|`M-<DEL>`|Kill<sub>cut</sub> the word immediately before the cursor|
|C-d|Delete the next character after the cursor|
|M-d|Kill<sub>cut</sub> the next word after the cursor|
|C-k|Kill<sub>cut</sub> from the cursor position to end of line|
|M-k|Kill<sub>cut</sub> to the end of the current sentence<sub>句子</sub>|
|C-y|yank<sub>paste</sub>.If you do several `C-k`<sub>`M-k`</sub>'s in a row, all of the killed<sub>cuted</sub> text is saved together, so that one `C-y` will yank all of the lines at once.|
|C-y M-y M-y...|yank the previous text.After you have done `C-y` to get the most recent kill, typing `M-y` replaces that yanked text with the previous kill.  Typing `M-y` again and again brings in earlier and earlier kills.  When you have reached the text you are looking for, you do not have to do anything to keep it.  Just go on with your editing, leaving the yanked text where it is.|
|C-y C-u 1 M-y|粘贴上一个|
|C-y C-u -1 M-y|粘贴下一个|
|`M-x repl s<Return>changed<Return>altered<Return>`|replaced the word "changed" with "altered" wherever it occurred, after the initial position of the cursor.|
| ***select text*** |
|`C-<SPC>`| type `C-<SPC>`.  (`<SPC>` is the Space bar.) Next, move the cursor to the other end of the text you intend to kill. As you do this, Emacs highlights the text between the cursor and the position where you typed `C-<SPC>`.  Finally, type `C-w`.  This kills all the text between the two positions. |
|C-@|`C-<SPC>` 被占用可使用此组合代替|
|C-w|cut select|
|M-w|copy select|
| ***SEARCHING*** |
||Emacs can do searches for strings (a "string" is a group of contiguous characters) either forward through the text or backward through it. Searching for a string is a cursor motion command; it moves the cursor to the next place where that string appears.|
|C-s|`C-s` starts a search that looks for any occurrence of the search string AFTER the current cursor position. When you type `C-s` you'll notice that the string "I-search" appears as a prompt in the echo area.  This tells you that Emacs is in what is called an incremental search waiting for you to type the thing that you want to search for. Type `C-s` again, to search for the next occurrence of you typed.Type `<DEL>` the cursor moving backward the first occurrence and The del the string.Type `<Return>` to terminate the search.`C-g` would also terminate the search|
|C-r|`C-r` starts a search that looks for any occurrence of the search string BEFORE the current cursor position.|
| ***move*** |
|C-v|Move forward one screenful|
|M-v|Move backward one screenful|
|C-l|Clear screen and redisplay all the text,moving the text around the cursor to the center of the screen.(That's CONTROL-L, not CONTROL-1.)|
|C-p|&#8593; Move to previous line|
|C-n|&#8595; Move to next line|
|C-f|&#8594; Move forward a character|
|C-b|&#8592; Move backward a character|
|M-f|&#8594; Move forward a word|
|M-b|&#8592; Move backward a word|
|C-a|Move to beginning of line|
|C-e|Move to end of line|
|M-a|Move back to beginning of sentence<sub>句子</sub>|
|M-e|Move forward to end of sentence|
|`M-<`|(META Less-than) moves to the beginning of the whole text. On most terminals, the "<" is above the comma<sub>逗号</sub>, so you must use the `shift` key to type it.  On these terminals you must use the `shift` key to type `M-<` also; without the `shift` key, you would be typing `M-comma`<sub>逗号</sub>.|
|`M->`|(META Greater-than) moves to the end of the whole text. |
|C-u|Most Emacs commands accept a numeric argument; for most commands, this serves as a repeat-count.  The way you give a command a repeat count is by typing `C-u` and then the digits before you type the command.  If you have a `META` (or `ALT`) key, there is another, alternative way to enter a numeric argument: type the digits while holding down the `META` key.|
|C-u 8 C-f|&#8594;moves forward eight characters|
|C-u 8 C-v|`C-v` and `M-v` are another kind of exception.  When given an argument,they scroll the text up or down by that many lines, rather than by a screenful.  For example, `C-u 8 C-v` scrolls by 8 lines.|
| ***argument*** |
|C-u|Most Emacs commands accept a numeric argument; for most commands, this serves as a repeat-count.  The way you give a command a repeat count is by typing `C-u` and then the digits before you type the command.  If you have a `META` (or `ALT`) key, there is another, alternative way to enter a numeric argument: type the digits while holding down the `META` key.|
|C-u 8 C-f|moves forward eight characters|
|C-u 8 C-v|`C-v` and `M-v` are another kind of exception.  When given an argument,they scroll the text up or down by that many lines, rather than by a screenful.  For example, `C-u 8 C-v` scrolls by 8 lines.|
|C-u 8 *|insert ********|
| ***Extending the command set*** |
|C-x|Character eXtend. Followed by one character|
|M-x|Named command eXtend. Followed by a long name. Just type `repl s<TAB>` and Emacs will complete the name.  (`<TAB>` is the `Tab` key, usually found above the `Caps Lock` or `Shift` key near the left edge of the keyboard.) Submit the command name with `<Return>`.|
| ***BUFFERS*** |
| | Emacs stores each file's text inside an object called a "buffer" |
|C-x C-b|List buffers|
|C-x b|Switch buffer|
| ***file*** |
| |You can find an existing file, to view it or edit it.  You can also find a file which does not already exist.  This is the way to create a file with Emacs|
| |Emacs periodically writes an "auto save" file for each file that you are editing.  The auto save file name has a # at the beginning and the end; for example, if your file is named "hello.c", its auto save file's name is "#hello.c#".  When you save the file in the normal way, Emacs deletes its auto save file.|
|C-x C-f|Find a file|
|C-x C-s|Save the file|
|C-x s|Save some buffers to their files|
|C-x C-c|Quit Emacs.`C-x C-c` offers to save each changed file before it kills Emacs.|
| ***MULTIPLE WINDOWS*** |
|C-x 1|Delete all but One window (i.e., kill all other windows). That is `CONTROL-x` followed by the digit 1.  `C-x 1` expands the window which contains the cursor, to occupy the full screen.  It deletes all other windows.|
|C-x 2|splits the screen into two windows. The editing cursor stays in the top window.|
|C-M-v|scroll the bottom window(If you do not have a real `META` key, type `<ESC> C-v`.)|
|C-x o|Type `C-x o` ("o" for "other") to move the cursor to the other window.|
|C-x 4 C-f|Type `C-x 4 C-f` followed by the name of one of your files. End with `<Return>`.  See the specified file appear in the bottom window.  The cursor goes there, too.|
| ***MULTIPLE FRAMES*** |
||Emacs can also create multiple "frames".  A frame is what we call one collection of windows, together with its menus, scroll bars, echo area, etc.  On graphical displays, what Emacs calls a "frame" is what most other applications call a "window".  Multiple graphical frames can be shown on the screen at the same time.  On a text terminal, only one frame can be shown at a time.|
|C-x 5 2|create A  new frame|
|C-x 5 0|removes the selected frame.|
| ***Suspend*** |
|C-z|C-z is the command to exit Emacs *temporarily*--so that you can go back to the same Emacs session afterward.  When Emacs is running on a text terminal, C-z "suspends" Emacs; that is, it returns to the shell but does not destroy the Emacs job.  In the most common shells, you can resume Emacs with the "fg" command or with "%emacs".|
| ***Major MODE*** |
|`M-x fundamental-mode <Return>`|The default mode is Fundamental|
|`M-x text-mode <Return>`|editing human-language text you should probably use Text Mode.|
| ***Minor MODE*** |
||Minor modes are not alternatives to the major modes, just minor modifications of them. Each minor mode can be turned on or off by itself, independent of all other minor modes, and independent of your major mode.  So you can use no minor modes, or one minor mode, or any combination of several minor modes|
|`M-x auto-fill-mode <Return>`|When this mode is on, Emacs breaks the line in between words automatically whenever you insert text and make a line that is too wide.If the mode is off, this command turns it on, and if the mode is on, this command turns it off.|
|C-u 120 C-x f|The margin is usually set at 70 characters, but you can change it with the `C-x f` command.  You should give the margin setting you want as a numeric argument.If you make changes in the middle of a paragraph, Auto Fill mode does not re-fill it for you. To re-fill the paragraph, type `M-q` (META-q) with the cursor inside that paragraph.|
|M-q|If you make changes in the middle of a paragraph, Auto Fill mode does not re-fill it for you. To re-fill the paragraph, type `M-q` (META-q) with the cursor inside that paragraph.|
| ***INSTALLING PACKAGES*** |
|M-x list-packages|To see a list of all available packages|
|`M-x package-install <Return>evil<Return>`| install evil|
| ***help*** |
||If `C-h` does not display a message about help at the bottom of the screen, try typing the `F1` key or `M-x` help `<Return>` instead|
|C-h k C-f|在新window显示 C-f 帮助说明|
|C-h c C-p|在echo area 显示简要说明|
|C-h x|Describe a command.  You type in the name of the command.|
|C-h a|Command Apropos.  Type in a keyword and Emacs will list all the commands whose names contain that keyword. These commands can all be invoked with `META-x`. For some commands, Command Apropos will also list a sequence of one or more characters which runs the same command.|
|`C-h a file <Return>`|This displays in another window a list of all `M-x` commands with "file" in their names.  You will see character-commands listed beside the corresponding command names (such as `C-x C-f` beside find-file).|
|C-h i|Read included Manuals (a.k.a. Info).  This command puts you into a special buffer called "*info*" where you can read manuals for the packages installed on your system. Type m emacs `<Return>` to read the Emacs manual. If you have never before used Info, type h and Emacs will take you on a guided tour of Info mode facilities. Once you are through with this tutorial, you should consult the Emacs Info manual as your primary documentation.|