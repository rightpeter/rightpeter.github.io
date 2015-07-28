---
layout: post
category: note
tagline: "Working Effectively With iTerm2"
tags: [iTerm, terminal, mac]
---
{% include JB/setup %}

**Reprinted From [http://teohm.com/blog/2012/03/22/working-effectively-with-iterm2/](http://teohm.com/blog/2012/03/22/working-effectively-with-iterm2/)**

## Install iTerm2

If you haven't heard of [item](http://www.iterm2.com/), it's popular open
source alternative to Mac OS X Terminal. Give it a try, download and install it
from http://www.iterm2.com.

- - - -

### Fine-Tune Settings

Launch iTerm, open **iTerm > Preferences** or just `Cmd + ,`.

#### Open tab/pane with current working directory

Under **Profiles** tab, go to **General** subtab, set **Working Directory** to
_"Reuse previous session's directory"_

#### Enable Meta key

To enable Meta key for [Bash readline editing](http://teohm.com/blog/2012/01/
04/shortcuts-to-move-faster-in-bash-command-line) e.g. `Alt + b` to move to
previous word, under **Profile** tab, go to **Keys** subtabs, set **Left option
key acts as:** to _"+Esc"_.

#### Hotkey to toggle iTerm2

Under **Keys** tab, in **Hotkey** section, enable _"Show/hide iTerm2 with a
system-wide hotkey"_ and input your hotkey combination, e.g. I use `Ctrl +
Shift + L`.

#### Switch pane with mouse cursor

Under **Pointer**, in **Miscellaneous Settings** section, enable _"Focus
follows mouse"._

- - - -

### Handy Shortcut Keys

Here's a set of **shortcut keys I commonly use**. You can always look for other
shortcut keys in the iTerm menu.

#### Tab navigation

>* open new tab `Cmd + t`
>* next tab `Cmd + Shift + ]`
>* previous tab `Cmd + Shift + [`

#### Pane navigation

>* split pane left-right `Cmd + d`
>* split pane top-botton `Cmd + Shift + d`
>* next pane `Cmd + ]`
>* previous pane `Cmd + [`

#### Search

>* open search bar `Cmd + f`
>* find next `Cmd + g`

#### Input to all panes

>* input to all panes in current cab `Cmd + Alt + i`

#### Clear screen

>* clear buffer `Cmd + k`
>* clear lines(Base command) `Ctrl + l`

#### Zooming/Font Resize

>* toggle maximize window `Cmd + Alt + =`
>* toggle full screen `Cmd + Enter`
>* make font larger `Cmd + +`
>* make font smaller `Cmd + -`

iTerm lovers, did I miss anything out?
