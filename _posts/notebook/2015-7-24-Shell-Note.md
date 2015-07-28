---
layout: post
category: note
tagline: "Shell Note"
tags: [Shell]
---
{% include JB/setup %}

# Vim mode in Bash

Reference to [Arabesque](http://blog.sanctum.geek.nz/vi-mode-in-bash/)

Because the Bash shell uses GNU Readline, you're able to set options in .bashrc
and .inputrc to extensively customise how you enter your command lines,
including specifying whether you want the default Emacs-like keybindings (e.g.
Ctrl+W to erase a word), or bindings using a modal interface familiar to vim
users.

    set editing-mode vi

If you only wnat to use this mode in Bash, an alternative is to use the
following in your **.bashrc**, again with a login/logout or resourcing of the
file:

    set -o vi

You can check this has applied correctly with the following, which will spit
out a list of the currently available bindings for a set of fixed possible
actions for text:

    $ bind -P

The other possible value fo **editing-mode is **emacs**. Both option simply
change settings in the output of **bind -P** above.

This done, you should find that you open a command line in insert mode, but
when you press Escape or `Ctrl+[`, you will enter an emulation of Vim's _normal
mode_. Enter will run the command in its present state while in either mode.
The bindings of most use in this mode are the classic keys for moving to
positions on a line:

 * `^` - Move to start of line
 * `$` - Move to end of lin
 * `b` - Move back a word
 * `w` - Move forward a word
 * `e` - Move to the end of the next word

- - - -
