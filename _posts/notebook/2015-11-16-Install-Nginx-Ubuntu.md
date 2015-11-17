---
layout: post
title: "How To Install Nginx on Ubuntu 14.04 LTS"
subtitle: ""
date: 2015-11-16 15:00:00
author: "rightpeter"
header-img: ""
---

[Referenced to digitalocean](https://www.digitalocean.com/community/tutorials/how-to-install-nginx-on-ubuntu-14-04-lts)

# Install Nginx

    sudo apt-get update
    sudo apt-get install nginx

# Check Web Server

You should see the default Nginx landing page, which should look something like this:

![Welcome to nginx](https://assets.digitalocean.com/articles/nginx_1404/default_page.png)

# Manage the Nginx Process

To stop your web server, you can type:

    sudo service nginx stop

To start the web server when it is stopped, type:

    sudo service nginx start

To stop and then start the service again, type:

    sudo service nginx restart

We can make sure that our web server will restart automatically when the server is rebooted by typing:

    sudo update-rc.d nginx defaults

This should already be enabled by default, so you may see a message like this:

    System start/stop links for /etc/init.d/nginx already exist.

This just means that it was already configured correctly and that no action was necessary. Either way, your Nginx service
is now configured to start up at boot time.
