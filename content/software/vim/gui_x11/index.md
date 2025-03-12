+++
title = 'Vim GUI-X11'
date = '2025-02-20'
author = 'aiclr'
draft = true
categories = ['vim']
tags = ['vim','help','gui','gui-x11']
Summary=':help gui_x11'
+++

Vim's Graphical User Interface

1. Starting the X11 GUI         gui-x11-start
2. GUI Resources                gui-resources
3. Shell Commands               gui-pty
4. Various                      gui-x11-various
5. GTK version                  gui-gtk
6. GNOME version                gui-gnome
7. KDE version                  gui-kde
8. Compiling                    gui-x11-compiling
[9. X11 selection mechanism      x11-selection](#9-x11-selection-mechanism)

Other relevant documentation:<br>
[gui.txt         For generic items of the GUI.](../gui)

___
___

## 9. X11 selection mechanism

### x11-selection

If using X11, in either the GUI or an xterm with an X11-aware Vim, then Vim provides varied access to the X11 selection and clipboard. These are accessed by using the two selection tegisters `"*` and `"+`.

X11 provides two basic types of global store, selections and cut-buffers, which differ in one important aspect: selections are ***owned*** by an application, and disappear when that application (e.g., Vim) exits, thus losing the data, whereas cut-buffers, are stored within the X-server itself and remain until written over or the X-server exits (e.g., upon logging out).

The contents of selections are held by the originating application (e.g., upon a copy), and only passed on to another application when that other application asks for them (e.g., upon a paste).

The contents of cut-buffers are immediately written to, and are then accessible directly from the X-server, without contacting the originating application.

### x11-cut-buffer

There are, by default, 8 cut-buffers: CUT_BUFFER0 to GUI_BUFFER7. Vim only uses CUT_BUFFER0, which is the one that xterm uses by default.

Whenever Vim is about to become unavailable (either via exiting or becoming suspended), and thus unable to respond to another application's selection request, it writes the contents of any owned selection to CUT_BUFFER0. If the `"+` CLIPBOARD selection is owned by Vim, then this is written in preference, otherwise if the `"*` PRIMARY selection is owned by Vim, then that is written.

Similarly, when Vim tries to paste from `"*` or `"+` (either explicitly, or, in the case of the `"*` register, when the middle mouse button is clicked), if the requested X selection is empty or unavailable, Vim reverts to reading the current value of the CUT_BUFFER0.

Note that when text is copied to CUT_ BUFFER0 in this way, the type of selection (character, line or block) is always lost, even if it is a Vim which later pastes it.

Xterm, by default, always writes visible selections to both PRIMARY and CUT_BUFFER0. When it pastes, it uses PRIMARY if this is avaiable, or else falls back upon CUT_BUFFER0. For this reason, when cutting and pasting between Vim and an xterm, you should use the `"*` register. Xterm doesn't use CLIPBOARD, thus the `"+` doesn't work with xterm.

Most newer applications will provide their current selection via PRIMARY `"*` and use CLIPBOART `"+` for cut/copy/paste operations. You thus have access to both by choosing to use either of the `"*` or `"+` registers.
