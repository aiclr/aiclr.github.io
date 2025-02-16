+++
title = 'quick reference guide'
date = '2025-02-14'
author = 'aiclr'
draft = true
categories = ['vim']
tags = ['vim','help','quickref']
Summary=':help quickref'
+++

## quickref Contents

|tag|subject|tag|subject|
|:---|:---|:---|:---|
|Q_ct|list of help files         |Q_re|Repeating commands|
|[Q_lr](#q_lr-left-right-motions)|motion: Left-right         |Q_km|Key mapping|
|Q_ud|motion: Up-down            |Q_ab|Abbreviations|
|Q_tm|motion: Text object        |Q_op|Options|
|Q_pa|motion: Pattern searches   |Q_ur|Undo/Redo commands|
|Q_ma|motion: Marks              |Q_et|External commands|
|Q_vm|motion: Various            |Q_qf|Quickfix commands|
|Q_ta|motion: Using tags         |Q_vc|Various commands|
|Q_sc|Scrolling                  |Q_ce|Ex: Command-line editing|
|Q_in|insert: Inserting text     |Q_ra|Ex: Ranges|
|Q_ai|insert: Keys               |Q_ex|Ex: Special characters|
|Q_ss|insert: Special keys       |Q_st|Starting Vim|
|Q_di|insert: Digraphs           |Q_ed|Editing a file|
|Q_si|insert: Special inserts    |Q_fl|Using the argument list|
|Q_de|change: Deleting text      |Q_wq|Writing and quitting|
|Q_cm|change: Copying and moving |Q_ac|Automatic commands|
|Q_ch|change: Changing text      |Q_wi|Multi-window commands|
|Q_co|change: Complex            |Q_bu|Buffer list commands|
|Q_vi|Visual mode                |Q_sy|Syntax highlighting|
|Q_to|Text objects               |Q_gu|GUI commands|
|    |                           |Q_fo|Folding|


## Q_lr Left-right motions

|||||
|:---|:---|:---|:---|
|h  |N|h      | left (also: CTRL-H, <BS>, or <Left> key)|
|l  |N|l      | right (also: <Space> or <Right> key)|
|0  | |0      | to first character in the line (also: <Home> key)|
|^  | |^      | to first non-blank character in the line|
|$  |N|$      | to the last character in the line (N-1 lines lower) (also: <End> key)|
|g0 | |g0     | to first character in screen line (differs from "0" when lines wrap)|
|g^ | |g^     | to first non-blank character in screen line (differs from "^" when lines wrap)|
|g$ |N|g$     | to last character in screen line (differs from "$" when lines wrap)|
|gm | |gm     | to middle of the screen line|
|gM | |gM     | to middle of the line|
|bar|N|&#124; | to column N (default: 1)|
|f  |N|f{char}| to the Nth occurrence of {char} to the right|
|F  |N|F{char}| to the Nth occurrence of {char} to the left|
|t  |N|t{char}| till before the Nth occurrence of {char} to the right|
|T  |N|T{char}| till before the Nth occurrence of {char} to the left|
|;  |N|;      | repeat the last "f", "F", "t", or "T" N times|
|,  |N|,      | repeat the last "f", "F", "t", or "T" N times in opposite direction|