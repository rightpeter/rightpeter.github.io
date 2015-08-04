---
layout: post
title: "Shell Note"
subtitle: ""
date: 2015-7-24 12:00:00
author: "rightpeter"
header-img: ""
---

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

# powerline-shell

[powerline-shell](https://github.com/milkbikis/powerline-shell)

- - - -

# Solve Ubuntu: Sudo Unable to Resolve Host

Edit: /etc/hosts

    # Add the codes below to /etc/hosts
    127.0.0.1   `Your host name`

- - - -

# Add and Delte Users on Ubuntu

Reference to [digitalocean](https://www.digitalocean.com/community/tutorials/how-to-add-and-delete-users-on-an-ubuntu-14-04-vps)

## Introduction

One of the most basic tasks to that you should know how to do on a fresh Linux
server is add and remove users. When you create a new server, you are only
given the `root` account by default.

While this gives you a lot of powe and flexibility, it is also dangerous and
can be destructive. It is almost always a better idea to add an additional,
unprivileged user to do common tasks. You also should create additional
accounts for any other users you may have on your system. Each user should have
a different account.

You can still acquire administrator privileges when you need them through a
mechanism called `sudo`. In this guide we will cover how to create user
accounts, assign sudo privileges, and delete users.

### How To Add a User

If you are signed in as the `root` user, you can create a new user at any time
by typing:

    adduser **newuser**

If you are signed in as a non-root uer who has been given `sudo` privileges, as
demonstrated [in the initial server setup guide](https://www.digitalocean.com/community/articles/initial-server-setup-with-ubuntu-14-04),
you can add a new user by typing:

    sudo adduser newuser

either way, you will be asked a series of questions. The procedure will be:

    * Assign and confirm a password for new user

    * Enter any additional information about the new user. This is entirely
      optional and can be skipped by hitting "ENTER" if you don't wish to
      utilize these fields.

    * Finally, you'll be asked to confirm that the information you provided was
      correct. Type "Y" to continue.

Your new user is now ready for use! You can now log in using the password you
set up.

**Note**: Continue on if you need your new user to have access to
administrative functionality.

### How To Grant a User Sudo Privileges

If your new user should have the ability to execute commands with root
(administrative) privileges, you will need to give the new user access to
`sudo`.

We can do this by using the `visudo` command, which opens the appropriate
configuration file in your editor. This is the safest way to make these
changes.

If you are currently signed in as the root user, type:

    visudo

If you are signed in using a non-root user with sudo privileges, type:

    sudo visudo

Search for the line that looks like this:

    root    ALL=(ALL:ALL) ALL

Below this line, copy the format you see here, changing only the word "root" to
reference the new user that you would like to give sudo privileges to:

    root    ALL=(ALL:ALL) ALL
    **newuser** ALL+(ALL:ALL) ALL

You should add a new line like this for each user that should be given full
sudo privileges. When you are finished, you can save and close the file by
hitting `CTRL-X`, followed by "Y", and then hit "ENTER" to confirm.

Now, your new user is able to execute commands with administrative privileges.

When signed in as the new user, you can execute commands ad your regular user
by typing commands as normal:

    some_command

You can execute the same command with administrative privileges by typing
`sudo` ahead of the command:

    sudo some_command

You will be prompted to enter the password of the regular user account you are
signed in as.

### How To Delete a User

In the event that you no longer need a user, it is best to delete the old
account. You can delete the user itself, without deleting any of his or her
files by typing this as root:

    deluser **newuser**

If you are signed in as another non-root user with sudo privileges, you could
instead type:

    sudo deluser newuser

If, instead, you want to delete the user's home directory when the user is
deleted, you can issue the following command as root:

    deluser --remove-home newuser

If you're rnning this as a non-root user with sudo privileges, you would
instead type:

    sudo deluser --remove-home newuser

If you had previously configured sudo privileges for the user you deleted, you
may want to remove the relevant line again by typing:

    visudo

Or use this if you are a non-root user with sudo privileges:

    sudo visudo

    

    root    ALL=(ALL:ALL) ALL
    newuser ALL=(ALL:ALL) ALL   # DELETE THIS LINE

This will prevent a new user created with the same name from being accidentally
given sudo privileges.

### Conclusion

You should now have a fairly good handle on how to add and remove users from
your Ubuntu system. Effective user management will allow you separate users and
give them only access that they are required to do their job.

For more information about how to configure `sudo`, check out our guide on [how
to edit the sudoers file](https://www.digitalocean.com/community/articles/how-to-edit-the-sudoers-file-on-ubuntu-and-centos) here.
