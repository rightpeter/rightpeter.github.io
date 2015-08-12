---
layout: post
title: "Emmet-Vim-Study"
subtitle: ""
date: 2015-8-10 10:00:00
author: "rightpeter"
header-img: ""
---

Reference to [mattn](https://raw.githubusercontent.com/mattn/emmet-vim/master/TUTORIAL)

# Expand an Abbreviation

Type the abbreviation as `div>p#foo$*3>a` and type `<c-y>,`.

    <div>
        <p id="foo1"><a href=""></a></p>
        <p id="foo2"><a href=""></a></p>
        <p id="foo3"><a href=""></a></p>
    </div>

- - - -

# Wrap with an Abbreviation

Write as below.

    test1
    test2
    test3

Then do visual select(line wise) and type `<c-y>,`.

Once you get to the `Tag:` prompt, type `ul>li*`.

    <ul>
        <li>test1</li>
        <li>test2</li>
        <li>test3</li>
    </ul>

If you type a tag, such as `blockquote`, then you'll see the following:

    <blockquote>
        test1
        test2
        test3
    </blockquote>

- - - -

# Balance a Tag Inwar

Type `<c-y>d` in insert mode.

- - - -

# Balance a Tag Outward

Type `<c-y>D` in insert mode.

- - - -

# Go to the Next Edit Point

Type `<c-y>n` in insert mode.

- - - -

# Go to the Previous Edit Point

Type `<c-y>N` in insert mode.

- - - -

# Update an `<img>`'s Size

Move cursor to the img tag.

    <img src="foo.png" />

Type `<c-y>i` on img tag

    <img src="foo.png" width="32" height="48" />

- - - -

# Merge Lines

Select the lines, which include `<li>`.

    <ul>
        <li class="List1"></li>
        <li class="List2"></li>
        <li class="List3"></li>
    </ul>

and then type `<c-y>m`

    <ul>
        <li class="List1"></li><li class="List2"></li><li class="List3"></li>
    </ul>

- - - -

# Remove a Tag

Move cursor in block

    <div class="foo">
        <a>cursor is here</a>
    </div>

Type `<c-y>k` in insert mode.

    <div class="foo">
        
    </div>

And type `<c-y>k` in there again

    - - - -

- - - -

# Split/Join Tag

Move the cursor inside block

    <div class="foo">
        cursor is here
    </div>

Type `<c-y>j` in insert mode.

    <div class="foo"/>

And then type `<c-y>j` in there again.

    <div class="foo">
    </div>

- - - -

# Toggle Comment

Move cursor inside the block

    <div>
        hello world
    </div>

Type `<c-y>/` in insert mode.

    <!-- <div>
        hello world
    </div> -->

Type `<c-y>/` in there again.

    <div>
        hello world
    </div>

- - - -

# Make an anchor from a URL

Move cursor to URL

    http://github.com/

Type `<c-y>A`

    <blockquote class="quote">
        <a href="http://github.com/">GitHub · Build software better, together.</a> <br>
        <p>Use at least one lowercase letter, one numeral, and seven characters. Sign up for GitHub By clicking...</p>
        <cite>http://github.com/</cite>
    </blockquote>

- - - -

# Installing emmet.vim for the language you are using:

    # cd ~/.vim
    # unzip emmet-vim/zip

Or if you are using pathogen.vim:

    # cd ~/.vim/bundle # or make directory
    # unzip /path/to/emmet-vim.zip

Or if you get the sources from the repository:

    # cd ~/.vim/bundle # or make directory
    # git clone http://github.com/mattn/emmet-vim.git

- - - -

# Enable emmet.vim for the language you using.

You can customize the behavior of the languages you are using.

    # cat >> ~/.vimrc
    let g:user_emmet_settings = {
    \   'php' : {
    \       'extends' : 'html',
    \       'filters' : 'c',
    \   },
    \   'xml' : {
    \       'extends' : 'html',
    \   },
    \   'haml' : {
    \       'extends' : 'html',
    \   },
    \}
