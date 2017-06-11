extends: default.liquid
author: admin
comments: true
date: 17 Jan 2010 09:12:45 +0000
layout: post
slug: nokia-n95-on-linux
title: Nokia N95 on Linux
wordpress_id: 257
categories:
- Hardware
- Linux
---

I'm currently using an Nokia N95 mobile phone. One thing that always bugged me
was that when plugging it in to a Linux machine via USB to copy files, it would
always complain when it was disconnected about the possibility that data may
have been lost, even though the file system was unmounted. I have just put up
with it however today I found a solution:

    # eject /dev/sdb

I don't know why I didn't try that earlier.
