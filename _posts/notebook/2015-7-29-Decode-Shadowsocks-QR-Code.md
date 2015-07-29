---
layout: post
title: "Decode Shadowsocks QR Code"
subtitle: ""
date: 2015-7-29 10:30:00
author: "rightpeter"
header-img: ""
---

# Shadowsocks Config and QR Code

Reference to
[shadowsocks.org](http://shadowsocks.org/en/config/quick-guide.html)

## Config File

QR Code is encode from ss.json, config of shadowsocks looks like:

    {
        "server": "my_server_ip",
        "server_port": 8388,
        "local_port": 1080,
        "password": "barfoo!",
        "timeout": 600,
        "method": "table"
    }

Explanation of each field:

 * `server`: your hostname or server IP (IPv4/IPv6).

 * `server_port`: server port number.

 * `local_port`: local port number.

 * `password`: a password used to encrypt transfer.

 * `timeout`: connections timeout in seconds.

 * `method`: encryption method, "bf-cfb", "aes-256-cfb", "des-cfb", "rc4", etc.
  Default is table, which is not secure. "aes-256-cfb" is recommended.

## URI and QR code

Shadowsocks for Android / iOS also accepts BASE64 encoded URI format configs:

    ss://BASE64-ENCODED-STRING-WITHOUT-PADDING

Where the plain URI should be:

    ss://method:password@hostname:port

For example, we have a server at `192.168.100.1:8888` using `bf-cfb` encryption
method and password `test`. Then, with the plain URI
`ss://bf-cfb:test@192.168.100.1:8888`, we can generate the BASE64encoded URI:

    ss://YmYtY2ZiOnRlc3RAMTkyLjE20C4xMDAuMTo40Dg4

This URI can also be encoded to QR code. Then, just scan it with your
Android/iOS devices

# Decode QR Code

    1. Use any QR Code scanner decode the QR Code into URI like
       `ss://encode_string`

    2. Use `echo encode_string | base64 --decode` to decode the URI

    3. Done!
