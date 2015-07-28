---
layout: post
title: "Vim Note"
subtitle: ""
date: 2015-7-23 12:00:00
author: "rightpeter"
header-img: ""
---

# [Python syntax highlighting](http://www.vim.org/scripts/script.php?script_id=790)

**Install details:**

Place [python.vim](http://www.vim.org/scripts/download_script.php?src_id=21056) file in ~/.vim/syntax/ folder.

- - - -

# [Markdown Vim Highlighting](http://www.vim.org/scripts/script.php?script_id=2882)

**Install Details**

Easy! Just open gzipball file with Vim (of course!) and use command :source %.
All files will be extracted in your Vim home directory.

`$ vim markdown-<version>.vba.gz +"source % | quit!"`

Please, visit
[https://github.com/hallison/vim-markdown/tree/changes](https://github.com/hallison/vim-markdown/tree/changes) for more information.

- - - -

# Mapping keys in Vim
Reference to [Mapping keys in Vim -
Tutorial](http://vim.wikia.com/wiki/Mapping_keys_in_Vim_-_Tutorial_(Part_1))

**Introduction**

Key mapping refers to creating a chortcut for repeating a sequence of keys or
commands. You can map keys to execute frequently used key sequences or to
invoke an Ex command or to invoke a Vim function or to invoke external
commands. Using key maps you can define your own Vim commands.

Vim supports several editing modes - normal, insert, replace, visual, select,
command-line and operator-pending. You can map a key to work in all or some of
these modes.

The general syntax of a map command is:

    {cmd} {attr} {lhs} {rhs}

    where
    {cmd}  is one of ':map', ':map!', ':nmap', ':vmap', ':imap',
           ':cmap', ':smap', ':xmap', ':omap', ':lmap', etc.
    {attr} is optional and one or more of the following: <buffer>, <silent>,
           <expr> <script>, <unique> and <special>.
           More than one attribute can be specified to a map.
    {lhs}  left hand side, is a sequence of one or more keys that you will use
           in your new shortcut.
    {rhs}  right hand side, is the sequence of keys that the {lhs} shortcut keys
           will execute when entered.

Examples:

    map <F2> :echo 'Current time is ' . strftime('%c')<CR>
    map! <F3> <C-R>=strftime('%c')<CR>
    nnoremap <silent> <F2> :lchdir %:p:h<CR>:pwd<CR>

The first step in creating a map is to decide the sequence of keys that needs
to be mapped. When you invoke a map, Vim will execute the sequence of keys as
though you entered it from the keyboard. You can do this by manually entering
the keysequence and verifying that they perform the desired operation.

The second step is to decide the editing mode (insert mode, visual mode,
command-line mode, normal mode, etc.) in which the map should work. Instead of
creating a map that works in all the modes, it is better to define the map that
works only in selected modes.

The third step is to find an unused key sequence that can be used to invoke the
map. You can invoke a map using either a single key or a sequence of keys.
[:help
map-which-keys](http://vimdoc.sourceforge.net/htmldoc/map.html#map-which-keys)

The above are explained in more detail in the following sections.

**Finding unused keys**

In your private maps you should use key sequences that are not used by Vim and
by other Vim plugins. [:help
map-which-keys](http://vimdoc.sourceforge.net/htmldoc/map.html#map-which-keys)

Many of the key sequences that you can enter from the keyboard are used by Vim
to implement the various internal commands. If you use a key sequence in your
map that is already used by Vim, then you will not be able to use the
functionality provided by Vim for that key sequence. To get a list of the key
sequences used by Vim, read the following help topic:

    :help index.txt

If you don't use some Vim functionality invoked by a particular key sequence or
you have an alternate key sequence to use that functionality then you can use
that key sequence in your maps.

Some of the key sequences may be used by the existing Vim scripts and plugins.
To display the list of keys that ar currently mapped, use the following
command:

    :map
    :map!

To determine the script or plugin that defines a map for a key sequence, use
the following command.

    :verbose map <key>

In the above command, replace <key> with the desired key sequence. For example,
to list all the locations where maps beginning with "," are defined, use the
following command:

    :verbose map ,

Try to use an unused key sequence in your maps. Typically, the <F2>, <F3>, ...
etc function keys are unused. The function keys in combination with Control,
Alt and Shift can also be used. But some of the key combinations may not work
in all the terminal emulators. Most of the key combinations should work in GUI
Vim.

You can alse prefix the desired key sequence with a backslash (\) or comma (,)
or underscore (\_), etc. and use that in your maps.

Note that you cannot map the Shift or Alt or Ctrl keys alone as they are key
modifiers. You have to combine these key modifiers with other keys to create a
map.

You should not use a frequently used Vim key sequence at the start of your map.
For example, you should not start your normal mode map key sequence with "j" or
"k" or "l" or "h". These keys are used for moving the cursor in normal mode. If
you use any of these keys at the begining of your maps, then you will observe a
delay when you enter a single "j" or "k" or "l" or "h".

**Using maps in Vim plugins and scripts**

A Vim plugin or script can define new key maps to let the user invoke the
commands and functions provided by the plugin. A Vim plugin can alse invoke key
maps defined by other Vim plugins.

A plugin can choose to map any available key. But to avoid surprising
(annoying) the user, it is better not to use the keys that already have
pre-defined functionality in Vim.

In a Vim plugin, the ":normal" command is used to execute normal mode commands.
For example, the "gqip" normal mode command is used to format a paragraph. To
invoke this command from a Vim plugin, the following line can be used:

    normal gqip

If any of the keys in "gqip" is mapped, then the mapped key sequence will be
executed. This may change the expected behavior of the "gqip" command. To avoid
this, add the "!" suffix to the "normal" command:

    normal! gqip

With the "!" suffix, the "normal" command executes the built-in functionality
provided by Vim for the specified sequence of keys.

To invoke a script local function, defined with the "s:" prefix, from a map,
you have to prefix the function name with <SID>. You cannot use the "s:" prefix
for the script-local function, as the map will be invoked from outside of the
script context.

    :inoremap <expr> <C-U> <SID>InsertFunc()

A plugin may map one or more keys to easily invoke the functionality provided
by the plugin. In the plugin functions used by these types of maps, it is
advisable not to alter user Vim option settings, register contents and marks.
Otherwise, the user will be surprised to see that some options are changed
after invoking a plugin provided map.

**Map leader**

If the key maps provided by all the Vim plugins start with a same key, then it
is easier for a user to distinguish between his own key maps and the ones
privided by plugins. To facilitate this, Vim provides a special keyword that
can be used in a map command.

- - - -

# Install vim-go

Add `Plugin 'fatih/vim-go'` into your .vimrc. Then type ':PluginInstall' in
vim.(vundle needed)

**Auto completion**

Auto completion is enabled by default via <C-x><C-o>, to get real-time
completion (completion by type) install YCM:

    $ cd ~/.vim/bundle
    $ git clone https://github.com/Valloric/YouCompleteMe.git
    $ cd YouCompleteMe
    $ ./install.sh

# New tab from existing mvim window

    $ mvim --remote-tab filename

MacVim first window is a Vim instance with `--servername` set to `VIM`. Each
subsequent instance is named `VIM1`, `VIM2` and so on. The command above opens
a new tab in the first instance by default, it's the equivalent of:

    $ mvim --servername VIM --remote-tab .profile

If you want to open a tab in a specific instance, check its name at the top of
the window. Assuming it's `VIM1`, do:

    $ mvim --servername VIM1 --remote-tab .profile

# MacVim Transparent

Add `set transparency=15` in `~/.gvimrc`

# Jumping to the start and end of a code block

To jump to the beginning/end of a code block (while, switch, if, function etc),
user the `[{`/`]}` command. (They will work from anywhere inside the code
block.)

To jump to the beginning/end of a parenthesis use the `[(`/`)]` command.

To jump to the beginning/end of a file use the `[[`/`]]`

- - - -

# Vimdiff

Reference to [amjith's blog](http://amjith.blogspot.com/2008/08/quick-and-dirty-vimdiff-tutorial.html)

![Vim diff](http://3.bp.blogspot.com/_Klq-3NKFe2s/SMfeDZ9gY9I/AAAAAAAAAaA/zHwt-XJqt0M/s400/vimdiff.png)

**Keyboard Shortcuts:**

`_do_` - Get changes from other windows into the current window.

`_dp_` - Put the changes from current window into the other window.

`]c` - Jump to the next change

`[c` - Jump to the previous change.

Update: Allan commented these two tips that I personally use quite often.

 * If you load up two files in splits (:vs or :sp), you can do _:diffthis_ on
   each window and achieve a diff of files that were already loaded in buffers
 * _:diffoff_ can be used to turn off the diff mode.

# Set working directory to the current file

Reference to [wikia](http://vim.wikia.com/wiki/Set_working_directory_to_the_current_file)

## Vim commands

The present working directory can be displayed in Vim with:

    :pwd

To change to the directory of the currently open file (this sets the current
directory for all windows in Vim):

    :cd %:p:h

You can alse change the directory only for the current window (each window has
a local current irectory that can be different from Vim's global current
directory):

    :lcd %:p:h

In these commands, `%` gives the name of the current file, `%:p` gives its full
path, and `%:p:h` gives its directory (the "head" of the full path).

## Automatically change the current directory

Sometimes it is helpful if your working directory is always the same as the
file you are editing. To achieve this, put the following in your vimrc:

    set autochdir

That's it! Unfortunately, when this option is set some plugins may not work
correctly if they make assumptions about the current directory. Sometimes, as
an alternative to setting `autochdir`, the following command gives better
results:

    autocmd BufEnter * silent! lcd %:p:h

This autocmd changes the window-local current directory to be the same as the
directory of the current file. It fails silently to prevent error messages when
you edit files via ftp or new files. It works better in some cases because the
autocmd is not nested, and will therefore not fire when switching buffers via
another autocmd. It will also work in older versions of Vim or versions
compiled without the 'autochdir' option. Note, however, that there is no easy
way to test for this autocmd in a script like there is for the 'autochdir'
option.

Either of these methods will "cd" to the directory of the file in the current
window, each time you switch to that window.

Using the autocmd method, you could customize when the directory change takes
place. For example, to not change directory if the file is in `/tmp`:

    autocmd BufEnter * if expand("%:p:h") !~ '^/tmp' | silent! lcd %:p:h |
    endif

**Caveats**

 * Either of these automatic methods will make loading and saving sessions work
   incorrectly.
 * Problems with 'autochdir' and netrw have been reported in the past, though
   they are fixed now.

## Alternatives

Mapping or command for quick directory change

Rather than automatically change the working directory, you can use a mapping
or a user command to easily change directory to the file being edited. The
mapping below maps the keystrokes `,cd`(comma c d) to do that.

    nnoremap ,cd :cd %:p:h<CR>

Alternatively, use:

    nnoremap ,cd :cd %:p:h<CR>:pwd<CR>

to print the directory after changing, so you know where you ended up.

If you prefer, use a command instead of a mapping. The following allows you to
enter `:CDC` to change directory (it also displays the new directory):

    " CDC = Change to Directory of Current file
    command CDC cd %:p:h

- - - -

# Search and replace

Reference to [wikia](http://vim.wikia.com/wiki/Search_and_replace)

Vim provides the `:s` (substitute) command for search and replace; this tip
shows examples of how to substitute. On some system, gvim has Find and Replace
on the Edit menu, however it is easier to use the `:s` command due to its
[command line history](http://vim.wikia.com/wiki/VimTip45) and ability to
insert text (for example, the word under cursor) into the search or replace
fields.

## Basic search and replace

The `:substitute` command searches for a [text
pattern](http://vim.wikia.com/wiki/VimTip188), and replaces it with a text
string. There are many options, but these are what you probably want:

 * `:%s/foo/bar/g`      Find each occurrence of 'foo' (in all lines), and replace it with 'bar'.

 * `:s/foo/bar/g`       Find each occurrence of 'foo' (in the current line only), and replace it
      with 'bar'.

 * `:%s/foo/bar/gc`     Change each 'foo' to 'bar', but ask for confirmation first.

 * `:%s/\<foo\>/bar/gc` Change only whole words exactly matching 'foo' to 'bar'; ask for
      confirmation.

 * `:%s/foo/bar/gci`    Change each 'foo' (case insensitive) to 'bar'; ask for confirmation. This
    may be wanted after using `:set noignorecase` to make searches case
    insensitive.

 * `:%s/foo/bar/gcI`    Change each 'foo' (case sensitive) to 'bar'; ask for confirmation. This
    may be wanted after using `:set ignorecase` to make searches case
    sensitive (the default).

The `g` flag means global - each occurrence in theline is changed, rather than
just the first. This tip assumes the default setting for the `'gdefault'` and
`'edcompatible'` option (off), which requires that the `g` flag be included in
`%s///g` to perform a global substitute. Using `:set gdefault` creates
confusion because then `%s///` is global, whereas `%s///g` is not (that is ,
`g` reverses its meaning).

When using the `c` flag, you need to confirm for each match what to do. Vim
will output something like: `replace with foobar (y/n/a/q/l/^E/^Y)?` (where
foobar is the replacement part of the `:s/.../.../` command. You can type `y`
which means to substitute this match, `n` to skip this match, `a` to substitute
this and all remaining matches ("all" remaining matches), `q` to quit the
command, `l` to substitute this match and quit (think of "last"), `^E` to
scroll the screen up by holding the Ctrl key and pressing E and `^Y` to scroll
the screen down by holding the Ctrl key and pressing Y. However, the last two
choices are only available, if your Vim is a normal, big or huge built or the
insert_expand feature was enabled at compile time (look for `+insert_expand` in
the output of `:version`).

Also when using the `c` flag, Vim will jump to the first match it finds
starting from the top of the buffer and prompt you for confirmation to perform
replacement on that match. Vim applies the `IncSearch` highlight group to the
matched text to give you a visual cue as to which match it is operating on (set
to `reverse` by default for all three term types as of Vim 7.3). Additionally,
if more than one match is found and you have search highlighting enabled with
`:set hlsearch`, Vim highlights the remaining matches with the `Search`
highlight group. If you do use search highlighting, you should make sure that
these two highlight groups are visually distinct or you won't be able to easily
tell which match Vim is prompting you to substitute.

See [wikia](http://vim.wikia.com/wiki/Search_and_replace) for more detail.
