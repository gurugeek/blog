extends: default.liquid
author: admin
comments: true
date: 2013-02-15 09:50:08+00:00
layout: post
slug: installing-bitmap-fonts-on-fedora-1718
title: Installing bitmap fonts on Fedora 17/18
wordpress_id: 536
categories:
- Fedora
- Linux
---

For the second time in a month I have had to go searching to find how to install bitmap fonts on Fedora 17/18. This is covered in the Fedora documentation for older versions of Fedora though appears to have been dropped at some point. To install bitmap fonts do the following:





  1. Create a directory under /usr/share/fonts/ e.g. /usr/share/fonts/local


  2. Copy font files to this directory and run mkfontdir and mkfontscale


  3. Create a symlink /etc/X11/fontpath.d/local -> /usr/share/fonts/local



