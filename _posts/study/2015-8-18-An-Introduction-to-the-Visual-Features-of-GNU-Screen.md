---
layout: post
title: "An-Introduction-to-the-Visual-Features-of-GNU-Screen"
subtitle: ""
date: 2015-8-18 14:00:00
author: "rightpeter"
header-img: ""
---

Reference to [Debian Administratio](https://www.debian-administration.org/article/560/An_introduction_to_the_visual_features_of_GNU_Screen)

Many people here use GNU Screen, and I've not seen extensive coverage of the
things you can do with the status-line in the past, so I thought a brief
overview of a couple of visual settings wouldn't be amiss. Read on for more
details.

GNU screen, **which we've previously introduced**, is a smaill utility which
may be used to leave sessions running when you've logged out, or ru multiple
console applications in only one terminal. There are many things you can do
with it which we'll not cover here, instead we'll look at the status bar.

 * A "caption" line.
 * A "hardstatus" line

Whats the difference between them? Well the `hardstatus` line is used for
status messages from screen - for example to alert you to activity, or other
similar messages. The caption line is usually only show if there is more than
one window open, and allows you to view details of them.

As an example add this to your ~/.screenrc file:

    caption string "%w"
    hardstatus alwayslastline "This is a test..."

This will give your screen sessions two lines at the bottom of the screen - the
first showing the open windows, the second showing a static message.

But what if you don't want to have those messages on the screen at all times?
Well that's not a problem:

    #
    #   Toggle 'fullscreen' or not.
    #
    bind f eval "caption splitonly" "hardstatus ignore"
    bind F eval "caption always"    "hardstatus alwayslastline"

With these lines in place `Ctrl-a` `f` will remove them, and `Ctrl-a` `F` will
restore them.

The next neat thing to do is show the output of commands in one of the two
lines, here we'll use the hardstatus area to show the output of uptime. There
are two things you need to do for this to work:

 * Define a command to run, with a refresh period.
 * Allocate a space on the relevant line to display it.

We'll do this as follows:

    backtick 1 5 5 uptime   
    hardstatus alwayslastline "%1`"

Here we've defined a command with ID 1, which is valid for 5 seconds and should
auto-refresh after 5 seconds. The command is uptime.

Additionally we've included `%1\`` in format string for the relevant part of
the display. This will be substituted with the output of the first command.

The important thing to notice is that the command we've chosen only outputs a
single line of text - that must always be the case.

To complete our discussion of this area of GNU screen we should now mention
some of the other facilities which are available to users who wish to customize
heir status bars.

The key to all customization is the use of format strings, strings which you'll
enter in the display lines which will be replaced upon display. For example the
following lines have extensive format string:

    #
    #   look and feel for the bottom two lines.
    #
    caption     always          "%{+b rk}%H%{gk} |%c %{yk}%d.%m.%Y | %72=Load: %l %{wk}"
    hardstatus  alwayslastline  "%?%{yk}%-Lw%?%{wb}%n*%f %t%?(%u)%?%?%{yk}%+Lw%?"

Below is a brief summery of the options, copied almost entirely from "man
screen". The excape character is '%' with one exception: inside a window's
hardstatus line '^%' is used instead.

 * %        the escape character itself
 * a        either 'am' or 'pm'
 * A        either 'AM' or 'PM'
 * c        current time HH:MM in 24h format
 * C        current time HH:MM in 12h format
 * d        day number
 * D        weekday name
 * f        flags of the window
 * F        sets %? to true if the window has the focus
 * h        hardstatus of the window
 * H        hostname of the system
 * l        current load of the system
 * m        month number
 * M        month name
 * n        window number
 * s        seconds
 * t        window title
 * u        all other users on this window
 * w        all window numbers and names. With '-' quailifier: up to the current window; with '+' qualifier: starting with the window
 * W        all window numbers and names except the current one
 * y        last two digits of the year number
 * Y        full year number

With taht list there is plenty of scope for creating simple updates to the
display. For example displaying the current date, time, and system load would
be as simple as:

    hardstatus alwayslastline "Host: %H Date: %d-%M-%Y: Load: %1 "

If you'd like to experiment yourself, there are many more format strings giving
a lot of choice for customization (such as the use of colours). To see the full
list run "man screen" and search for the section entitled STRING EXCAPES.
