slug: "exams-over-for-another-semester"
title: Exams over for another semester.
categories:
  - Arch
  - Linux
published_date: "2010-06-23 12:20:22 +0000"
layout: default.liquid
data:
  author: admin
  layout: post
  wordpress_id: 327
  comments: true
---
I haven't had a chance to post much in the last month as study has kept me fairly busy. My last exam was last Friday and I am now waiting for my results due out early next month. In the mean time I am enjoying having some time to relax.

The Arch Linux devs have been busy of late with a couple of significant updates coming down the pipe, most notably kernel 2.6.34 and Xorg 1.8. As usual the upgrade was painless thanks to the developers and [testing] users. I have been looking forward to 2.6.34 due to it's [new btrfs capabilities](http://kernelnewbies.org/LinuxChanges#head-98c8fb9d359cfbdd4a10bdc0c9d2d168b4c9ebb3). I also took the opportunity with the Xorg 1.8 update to disable hal and reduce my Xorg.conf down to the minimum required to work with my nVidia 8600GT using the nVidia binary blob with twinview enabled. I managed to get it down to a single "Screen" section of 15 lines and may be able to reduce it a bit more.

In the little spare time I have had I have been playing with [pyglet](http://www.pyglet.org/), a Python based OpenGL framework. I had not heard of pyglet until recently and am impressed by it. The documentation, made up of the [programming guide](http://www.pyglet.org/doc/programming_guide/index.html) and [api reference](http://www.pyglet.org/doc/api/index.html) are top quality. While pyglet is aimed at game development I am using it to make a prototype OpenGL based image viewer. This is coming along nicely though it is a little sluggish when loading an image, especially when I am used to feh which displays images very quickly. It appears that if I want to take this beyond the prototype stages I will need to switch to C/C++ using GLX to improve performance.

Pyglet makes displaying text very easy and it is one area of OpenGL that I don't have to much experience in. To this end I have also been playing with [freetype](http://freetype.sourceforge.net/index2.html) and was surprised at how simple it was to load a font and render it to a bitmap. At the moment I am rendering single glyphs but am working up to rendering strings with and without kerning. The [freetype tutorials](http://freetype.sourceforge.net/freetype2/documentation.html) have been a big help.
