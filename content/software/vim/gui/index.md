+++
title = 'Vim GUI'
date = '2025-02-20'
author = 'aiclr'
draft = true
categories = ['vim']
tags = ['vim','help','gui']
Summary=':help gui'
+++

Vim's Graphical User Interface

1. Starting the GUI             gui-start
2. Scrollbars                   gui-scrollbars
3. Mouse Control                gui-mouse
[4. Making GUI Selections        gui-selections](#4-making-gui-selections)
5. Menus                        menus
6. Font                         gui-font
7. Extras                       gui-extras
8. Shell Commands               gui-shell

Other GUI documentation:<br>
[gui_x11.txt     For specific items of the X11 GUI.](../gui_x11)<br>
[gui_w32.txt     For specific items of the Win32 GUI.]()

___
___

## 4. Making GUI Selections

### clipboard

There is a special register for storing this selection, it is the `"*` register. Nothing is put in here unless the information about what text is selected is about to change (e.g. with a left mouse click somewhere), or when another application wants to paste the selected text. Then the text is put in the `"*` register. For example, to cut a line and make it the current selection/put it on the clipboard:
```text
         "*dd
```
Similary, when you want to paste a selection from another application, e.g., by clicking the middle mouse button, the selection is put in the `"*` register first, and then ***put*** like any other register. For example, to put the selection (contents of the clipboard):
```text
         "*p
```
When using this register under X11, also see [x11-selection](../gui_x11#x11-selection). This also explains the related `"*` register.

Note that when pasting text from one Vim into another separate Vim, the type of selections (character, line, or block) will also be copied. For other applications the type is always character. However, if the text gets transferred via the [x11-cut-buffer](../gui_x11#x11-cut-buffer), the selection type is ALWAYS lost.

When the `unnamed` string is include in the ***clipboard*** option, the unnamed register is the same as the `"*` register. Thus you can yank to and paste the selection without prepending `"*` to commands.

___
___