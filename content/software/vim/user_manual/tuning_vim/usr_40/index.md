+++
title = 'usr_40 Make new commands'
date = '2025-02-07'
author = 'aiclr'
categories = ['vim']
tags = ['vim','help','user manual','tuning vim']
Summary='Make new commands'
+++

Vim is an extensible editor. You can take a sequence of commands you use often and turn it into a new command. Or redefine an existing command. Autocmooands make it possible to execute commands automatically.

- [40.1    Key mapping](#401-key-mapping)
- [40.2    Defining command-line commands](#402-defining-command-line-commands)
- [40.3    Autocommands](#403-autocommands)

Next chapter: [usr_41.txt](../usr_41)  Write a Vim script<br>
Previous chapter: [usr_32.txt](../../editing_effectively/usr_32)  The undo tree<br>
Table of contents: [usr_toc.txt](../../usr_toc)

___
___

## 40.1 Key mapping

A simple mapping was explained in section [05.4](../../getting_started/usr_05#054-simple-mapping). The principle is that one sequence of key strokes is translated into another sequence of key strokes. This is a simple, yet powerful mechanism.<br>
The simplest form is that one key is mapped to a sequence of keys. Since the function keys, except `<F1>`, have no predefined meaning in Vim, these are good choices to map. Example:
```text
        :map <F2> GoDate: <Esc>:read !date<CR>kJ
```
This shows how three modes are used. After going to the last line with `G`, the `o` command opens a new line and starts Insert mode. The text `Date:` is inserted and `<Esc>` takes you out of insert mode.<br>
Notice the use of special keys inside `<>`. This is called angle bracket notation. You type these as separate characters, not by pressing the key itself. This makes the mappings better readable and you can copy and paste the text without problems.<br>
The `:` character takes Vim to the command line. The `:read !date` command reads the output from the `date` command and appends it below the current line. The `<CR>` is required to execute the `:read` command.<br>
At this point of execution the text looks like this:
```text
        Date:
        Fri Jun 15 12:54:34 CEST 2001
```
Now `kj` moves the cursor up and joins the lines together.<br>
To decide which key or keys you use for mapping, see [map-which-keys](../../../map#map-which-keys).

### MAPPING AND MODES

The `:map` command defines remapping for keys in Normal mode. You can also define mappings for other modes. For example, `:imap` applies to Insert mode. You can use it to insert a date below the cursor:
```text
        :imap <F2> <CR>DATE: <Esc>:read !date<CR>kj
```
It looks a lot like the mapping for `<F2>` in Normal mode, only the start is different. The `<F2>` mapping for Normal mode is still there. Thus you can map the same key differently for each mode.<br>
Notice that, although this mapping starts in Insert mode, it ends in Normal mode. If you want it to continue in Insert mode, append an `a` to the mapping.

||Here is an overview of map commands and in which mode they work|
|:---|:---|
|:map|Normal, Visual and Operator-pending|
|:vmap|Visual|
|:nmap|Normal|
|:omap|Operator-pending|
|:map!|Insert and Command-line|
|:imap|Insert|
|:cmap|Command-line|

Operator-pending mode is when you typed an operator character, such as `d` or `y`, and you are expected to type the motion command or a text object. Thus when you type `dw`, the `w` is entered in operator-pending mode.

Suppose that you want to define `<F7>` so that the command `d<F7>` deletes a C program block (text enclosed in curly braces, {}). Similarly `y<F7>` would yank the program block into the unnamed register. Therefore, what you need to do is to define `<F7>` to select the current program block. You can do this with the following command:
```text
        :omap <F7> a{
```
This causes `<F7>` to perform a select block `a{` in operator-pending mode, just like you typed it. This mapping is useful if typing a `{` on your keyboard is a bit difficult.

### LISTING MAPPING

To see the currently defined mappings, use `:map` without arguments. Or one of the variants that include the mode in which they work. The output could look like this:
```text
           _g            :call MyGrep(1)<CR>
        v  <F2>          :s/^/> /<CR>:noh<CR>``
        n  <F2>          :.,$s/^/> /<CR>:noh<CR>``
           <xHome>       <Home>
           <xEnd>        <End>
```
The first column of the list shows in which mode the mapping is effective. This is `n` for Normal mode, `i` for Insert mode, etc. Ablank is used for a mapping defined with `:map`, thus effective in both Normal and Visual mode.<br>
One useful purpose of listing the mapping is to check if special keys in `<>` form have bean recognized (this only works when color is supported). For example, when `<Esc>` is displayed in clolor, it stands for the escape character. When it has the same color as the other text, it is five characters.

### REMAPPING

The result of a mapping is inspected for other mappings in it. For example, the mappings for `<F2>` above could be shortened to:
```text
        :map <F2> G<F3>
        :imap <F2> <Esc><F3>
        :map <F3>  oDate: <Esc>:read !date<CR>kJ
```
For Normal mode `<F2>` is mapped to go to the last line, and then behave like `<F3>` was pressed. In Insert mode `<F2>` stops Insert mode with `<Esc>` and then also uses `<F3>`. Then `<F3>` is mapped to do the actual work.

Suppose you hardly ever use Ex mode, and want to use the `Q` command to format text (this was so in old versions of Vim). This mapping will do it:
```text
        :map Q gq
```
But, in rare cases you need to use Ex mode anyway. Let's map `gQ` to Q,so that you can still go to Ex mode:
```text
        :map gQ Q
```
What happens now is that when you type `gQ` it is mapped to `Q`. So far so good. But then `Q` is mapped to `gq`, thus typing `gQ` results in `gq`, and you don't get to Ex mode at all.<br>
To avoid keys to be mapped again. use the `:noremap` command:
```text
        :noremap gQ Q
```
||Now Vim knows that the `Q` is not to be inspected for mappings that apply to it. There is a similar command for every mode:|
|:---|:---|
|:noremap|Normal,Visual and Operator-pending|
|:vnoremap|Visual|
|:nnoremap|Normal|
|:onoremap|Operator-pending|
|:noremap!|Insert and Command-line|
|:inoremap|Insert|
|:cnoremap|Command-line|

### RECURSIVE MAPPING

When a mapping triggers itself, it will run forever. This can be used to repeat an action an unlimited number of times.<br>
For example, you have a list of files that contain a version number in the first line. You edit these files with `Vim *.txt`. You are now editing the first file. Define this mapping:
```text
        :map ,, :s/5.1/5.2/<CR>:wnext<CR>,,
```
Now you type `,,`. This triggers the mapping. It replaces **5.1** with **5.2** in the first line. Then it does a `:wnext` to write the file and edit the next one. The mapping ends in `,,`. This triggers the same mapping again, thus doing the substitution, etc.<br>
This continues until there is an error. In this case it could be a file where the substitute command doesn't find a match for **5.1**. You can then make a change to insert **5.1** and continue by typing `,,` again. Or the `:wnext` fails, because you are in the last file in the list.<br>
When a mapping runs into an error halfway, the rest of the mapping is discarded. `CTRL-C` interrupts the mapping (`CTRL-Break` on MS-Windows).

### DELETE A MAPPING

To remove a mapping use the `:unmap` command.

||Again, the mode the unmapping applies to depends on the command used:|
|:---|:---|
|:unmap|Normal, Visual and Operator-pending|
|:vunmap|Visual|
|:nunmap|Normal|
|:ounmap|Operator-pending|
|:unmap!|Insert and Command-line|
|:iunmap|Insert|
|:cunmap|Command-line|

There is a trick to define a mapping that works in Normal and Operator-pending mode, but not in Visual mode. First define it for all three modes, then delete it for Visual mode:
```text
        :map <C-A> /---><CR>
        :vunmap <C-A>
```
Notice that the five characters `<C-A>` stand for the single key `CTRL-A`.

To remove all mappings use the `:mapclear` command. You can guess the variations for different modes by now. Be careful with this command, it can't be undone.

### SPECIAL CHARACTERS

The `:map` command can be followed by another command. A `|` character separates the two commands. This also means that a `|` character can't be used inside a map command. To include one, use `<Bar>` (five characters). Example:
```text
        :map <F8> :write <Bar> !checkin %:S<CR>
```
The same problem applies to the `:unmap` command, with the addition that you have to watch out for trailing white space. These two commands are different:
```text
        :unmap a | unmap b
        :unmap a| unmap b
```
The first command tries to unmap `a `, with a trailing space.

When using a space inside a mapping, use `<Space>` (seven characters):
```text
        :map <Space> W
```
This makes the spacebar move a blank-separated word forward.

It is not possible to put a comment directly after a mapping, because the *character is considered to be part of the mapping. You can use |*, this starts a new, empty command with a comment. Example:
```text
        :map <Space> W|     " Use spacebar to move forward a word
```

### MAPPING AND ABBREVIATIONS

Abbreviations are a lot like Insert mode mappings. The arguments are handled in the same way. The main difference is the way they are triggered. An abbreviation is triggered by typing a non-word character after the word. A mapping is triggered when typing the last character.<br>
Another difference is that the characters you type for an abbreviation are inserted in the text while you type them. When the abbreviation is triggered these characters are deleted and replaced by what the abbreviation produces. When typing the characters for a mapping, nothing is inserted until you type the last character that triggers it. If the [showcmd](../../../options#showcmd) option is set, the typed characters are displayed in the last line of the Vim window.<br>
An exception is when a mapping is ambiguous. Suppose you have done two mappings:
```text
        :imap aa foo
        :imap aaa bar
```
Now, when you type `aa`, Vim doesn't know if it should apply the first or the second mapping. It waits for another character to be typed. If it is an `a`, the second mapping is applied add results in `bar`. If it is a space, for example, the first mapping is applied, resulting in `foo`, and then the space is inserted.

### ADDITIONALLY...

The `<script>` keyword can be used to make a mapping local to a script. See [`:map-<script>`](../../../map#map-script).

The `<buffer>` keyword can be used to make a mapping local to a specific buffer. See [`:map-<buffer>`](../../../map#map-local-map-buffer-e224-e225).

The `<unique>` keyword can be used to make defining a new mapping fail when it already exists. Otherwise a new mapping simple overwrites the old one. See [`:map-<unique>`](../../../map#map-unique-e226-e227).

To make a key do nothing, map it to `<Nop>` (five characters). This will make the `<F7>` key do nothing at all:
```text
        :map <F7> <Nop>| map! <F7> <Nop>
```
There must be no space after `<Nop>`.

___
___

## 40.2 Defining command-line commands

The Vim editor enables you to define your own commands. You execute these commands just like any other Command-line mode command.<br>
To define a command, use the `:command` command, as follows:
```text
        :command DeleteFirst 1delete
```
Now when you execute the command `:DeleteFitst` Vim executes `:1delete`, which deletes the first line.

Note:<br>
User-defined commands must start with a capital letter. You cannot use `:X`, `:Next` and `:Print`. The underscore cannot be used! You can use digits, but this is discouraged.

To list the user-defined commands, execute the following command:
```text
        :command
```
Just like with the builtin commands, the user defined commands can be abbreviated. You need to type just enough to distinguish the command from another. Command line completion can be used to get the full name.

### NUMBER OF ARGUMENTS

User-defined commands can take a series of arguments. The number of arguments must be specified by the `-nargs` option. For instance, the example `:DeleteFirst` command takes no arguments, so you could have defined it as follows:
```text
        :command -nargs=0 DeleteFirst 1delete
```
However, beacuse zero arguments is the default, you do not need to add `-nargs=0`. The other values of `-narg` are as follows:
|-nargs||
|:---|:---|
|-nargs=0|No arguments|
|-nargs=1|One argument|
|-nargs=*|Any number of arguments|
|-nargs=?|Zero or one argument|
|-nargs=+|One or more arguments|

### USING THE ARGUMENTS

Inside the command definition, the arguments are represented by the `<args>` keyword. For example:
```text
        :command -nargs=+ Say :echo "<args>"
```
Now when you type
```text
        :Say Hello World
```
Vim echoes **Hello World**. However, if you add a double quote, it won't work. For example:
```text
        :Say he said "hello"
```
To get special characters turned into a string, properly escaped to use as an expression, use `<q-args>`:
```text
        :command -nargs=+ Say :echo <q-args>
```
Now the above `:Say` command will result in this to be executed:
```text
        :echo "he said" \"hello\""
```
The `<f-args>` keyword contains the same information as the `<args>` keyword, except in a format suitable for use as function call arguments. For example:
```text
        :command -nargs=* DoIt :call AFunction(<f-args>)
        :DoIt a b c
```
Executes the following command:
```text
        :call AFunction("a","b","c")
```

### LINE RANGE

Some commands take a range as their argument. To tell Vim that you are defining such a command, you need to specify a `-range` option. The values for this option are as follows:
```text
        -range          Range is allowed; default is the current line.
        -range=%        Range is allowed; default is the whole file.
        -range={count}  Range is allowed; the last number in it is used as a
                        single number whose default is {count}.
```
When a range is specified, the keywords `<line1>` and `<line2>` get the values of the first and last line in the range. For example, the following command defines the SaveIt command, which writes out the specified range to the file **save_file**:
```text
        :command -range=% SaveIt :<line1>,<line2>write! save_file
```

### OTHER OPTIONS

Some of the other options and keywords are as follows:
```text
        -count={number}         The command can take a count whose default is
                                {number}.  The resulting count can be used
                                through the <count> keyword.
        -bang                   You can use a !.  If present, using <bang> will                                result in a !.
        -register               You can specify a register.  (The default is
                                the unnamed register.)
                                The register specification is available as
                                <reg> (a.k.a. <register>).
        -complete={type}        Type of command-line completion used.  See
                                :command-completion for the list of possible
                                values.
        -bar                    The command can be followed by | and another
                                command, or " and a comment.
        -buffer                 The command is only available for the current
                                buffer.
```
Finally, you have the `<lt>` keyword. It stands for the character `<`. Use this to escape the special meaning of the `<>` items mentioned.

### REDEFINING AND DELETING

To redefine the same command use the `!` argument:
```text
        :command -nargs=+ Say :echo "<args>"
        :command! -nargs=+ Say :echo <q-args>
```
To delete a user command use `:delcommand`. It tales a single argument, which is the name of the command. Example:
```text
        :delcommand SaveIt
```
To delete all the user commands:
```text
        :comclear
```
Careful, this can't be undone!

More details about all this in the reference manual: [user-commands](../../../map#user-commands).

___
___

## 40.3 Autocommands

An autocommand is a command that is executed automatically in response to some event, such as a file being read or written or a buffer change, Through the use of autocommands you can train Vim to edit compressed files, for example. That is used in the [gzip](../../../pi_gzip#gzip-bzip2-compress) plugin.<br>
Autocommands are very powerfull. Use them with care and they will help you avoid typing many commands. Use them carelessly and they will cause a lot of trouble.

Suppose you want to replace a datestamp on the end of a file every time it is written. First you define a function:
```text
        :function DateInsert()
        :  $delete
        :  read !date
        :endfunction
```
You want this function to be called each time, just before a buffer is written to a file. This will make that happen:
```text
        :autocmd BufWritePre * call DateInsert()
```
`BufWritePre` is the event for which this autocommand is triggered: Just before (pre) writing a buffer to a file. The `*` is a pattern to match with the file name. In this case it matches all files.<br>
With this command enabled, when you do a `:write`, Vim checks for any matching BufWritePre autocommands and executes them, and then it performs the `:write`.<br>
The general form of the `:autocmd` command is as follows:
```text
        :autocmd [group] {events} {file-pattern} [++nested] {command}
```
The `[group]` name is optional. It is used in managing and calling the commands (more on this later). <br>
The `{event}` parameter is a list of events (comma separated) that trigger the command.<br>
`{file-pattern}` is a filename, usually with wildcards. For example, using `*.txt` makes the autocommand be used for all files whose name end in `.txt`.<br>
The optional `[++nested]` flag allows for nesting of autocommands (see below),<br>
and finally, `{command}` is the command to be executed.

When adding an autocommand the already existing ones remain. To avoid adding the autocommand several times you should use this form:
```text
        :augroup updateDate
        :  autocmd!
        :  autocmd BufWritePre *  call DateInsert()
        :augroup END
```
This will delete any previously defined autocommand with `:autocmd!` before defining the new one. Groups are explained later.

### EVENTS

One of the most useful events is BufReadPost. It is triggered after a new file is being edited. It is commonly used to set option values. For example, you know that `*.gsm` files are GUN assembly language. To get the syntax file right, define this autocommand:
```text
        :autocmd BufReadPost *.gsm set filetype=asm
```
If Vim is also to detect the type of file, it will set the [filetype](../../../options#filetype) option for you. This triggers the Filetype event. Use this to do something when a certain type of file is edited. For example, to load a list of abbreviations for text files:
```text
        :autocmd Filetype text source ~/.vim/abbrevs.vim
```
When starting to edit a new file, you could make Vim insert a skeleton<sub>骨架；框架</sub>:
```text
        :autocmd BufNewFile *.[ch] 0read ~/skeletons/skel.c
```
See [autocmd-events](../../../autocmd#autocmd-events-e215-e216) for a complete list of events.

### PATTERNS

The `{file-pattern}` argument can actually be a comma-separated list of file patterns. For example: `*.c,*.h` matches files ending in `.c` and `.h`.<br>
The usual file wildcards can be used. Here is a summary of the most often used ones:
```text
        *               Match any character any number of times
        ?               Match any character once
        [abc]           Match the character a, b or c
        .               Matches a dot
        a{b,c}          Matches "ab" and "ac"
```
When the pattern includes a slash (/) Vim will compare directory names. Without the slash only the last part of a file name is used. For example,`*.txt` matches `/home/biep/readme.txt`. The pattern `/home/biep/*` would also match it. But `/home/foo/*.txt` wouldn't.<br>
When including a slash, Vim matches the pattern against both the full path of the file (`/home/biep/readme.txt`) and the relative path (e.g., `biep/readme.txt`).

Note:<br>
When working on a system that uses a backslash as file separator, such as MS-Windows, you still use forward slashes in autocommands. This makes it easier to write the pattern, since a backslash has a special meaning. It also makes the autocommands portable.

### DELETING

To delete an autocommand, use the same command as what it was defined with, but leave out the `{command}` at the end and use a !. Example:
```text
        :autocmd! FileWritePre *
```
This will delete all autocommands for the `FileWritePre` event that use the `*` pattern.

### LISTING

To list all the currently defined autocommands, use this:
```
        :autocmd
```
The list can be very long, especially when filetype detection is used. To list only part of the commands, specify the group, event and/or pattern. For example, to list all BufNewFile autocommands:
```text
        :autocmd BufNewFile
```
To list all autocommands for the pattern `*.c`:
```text
        :autocmd * *.c
```
Using `*` for the event will list all the events. To list all autocommands for the cprograms group:
```text
        :autocmd cprograms
```

### GROUPS

The `{group}` item, used when defining an autocommand, groups related autocommands together. This can be used to delete all the autocommands in a certain group, for example.<br>
When defining several autocommands for a certain group, use the `:augroup` command. For example, let's define autocommands for C programs:
```text
        :augroup cprograms
        :  autocmd BufReadPost *.c,*.h :set sw=4 sts=4
        :  autocmd BufReadPost *.cpp   :set sw=3 sts=3
        :augroup END
```
This will do the same as:
```text
        :autocmd cprograms BufReadPost *.c,*.h :set sw=4 sts=4
        :autocmd cprograms BufReadPost *.cpp   :set sw=3 sts=3
```
To delete all autocommands in the **cprograms** group:
```text
        :autocmd! cprograms
```

### NESTING

Generally, commands executed as the result of an autocommand event will not trigger any new events. If you read a file in response to a FileChangedShell event, it will not trigger the autocommands that would set the syntax, for example. To make the events triggered, add the `nested` argument:
```text
        :autocmd FileChangedShell * ++nested  edit
```

### EXECUTING AUTOCOMMANDS

It is possible to trigger an autocommand by pretending an event has occurred. This is useful to have one autocommand trigger another one. Example:
```text
        :autocmd BufReadPost *.new  execute "doautocmd BufReadPost " . expand("<afile>:r")
```
This defines an autocommand that is triggered when a new file has been edited. The file name must end in `.new`. The `:execute` command uses expression evaluation to form a new command and execute it. When editing the file `tryout.c.new` the executed command will be:
```text
        :doautocmd BufReadPost tryout.c
```
The expand() function takes the `<afile>` argument, which stands for the file name the autocommand was executed for, and takes the root of the file name with `:r`.

`:doautocmd` executes on the current buffer. The `:doautoall` command works like `doautocmd` except it executes on all the buffers.

### USING NORMAL MODE COMMANDS

The commands executed by an autocommand are Command-line commands. If you want to use a Normal mode command, the `:normal` command can be used. Example:
```text
        :autocmd BufReadPost *.log normal G
```
This will make the cursor jump to the last line of `*.log` files when you start to edit it.<br>
Using the `:normal` command is a bit tricky. First of all, make sure its argument is a complete command, including all the arguments. When you use `i` to go to Insert mode, there must also be a `<Esc>` to leave Insert mode again. If you use a `/` to start a search pattern, there must be a `<CR>` to exeucte it.<br>
The `:normal` command uses all the text after it as commands. Thus there can be no | and another command following. To work around this, put the `:normal` command inside an `:execute` command. This also makes it possible to pass unprintable characters in a convenient way. Example:
```text
        :autocmd BufReadPost *.chg execute "normal ONew entry:\<Esc>" |
                \ 1read !date
```
This also shows the use of a backslash to break a long command into more lines. This can be used in Vim scripts (not at the command line).

When you want the autocommand do something complicated, which involves jumping around in the file and then returning to the original position, you may want to restore the view on the file. See [restore-position](../../../tips#restore-position) for an example.

### IGNORING EVENTS

At times, you will not want to trigger an autocommand. The [eventignore](../../../options#eventignore) option contains a list of events that will be totally ignored. For example, the following causes events for entering and leaving a window to be ignored:
```text
        :set eventignore=WinEnter,WinLeave
```
To ignore all events, use the following command:
```text
        :set eventignore=all
```
To set it back to the normal behavior, make `eventignore` empty:
```text
        :set eventignore=
```

___
___

Next chapter: [usr_41.txt](../usr_41)  Write a Vim script