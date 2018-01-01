slug: "mplayer-and-vdpau"
title: mplayer and vdpau
published_date: "2009-09-28 06:41:00 +0000"
layout: default.liquid
data:
  layout: post
  wordpress_id: 164
  author: admin
  comments: true
---
Just compiled mplayer with vdpau to make use of my nVidia 8600GT. On Arch Linux this is as simple as using ABS to rebuild the stock mplayer from extra. mplayer autodetects all supported features during configuration. The only issue I had was a linking error with libgif which was installed. Adding --disable-gif to the PKGBUILD fixed this. Then I added:

`vo=vdpau,xv,
vc=ffh264vdpau,ffmpeg12vdpau,`

to my _.mplayer/config_. I'm haven't checked how much of a difference it has made to CPU usage however one added benefit is that mplayer would sometimes freeze the video when in fullscreen mode in xmonad when changing workspaces around. This is no longer happening. Bonus!

**EDIT: Hmmm, it does still freeze but only now and then. Not sure if there is a pattern to it.**

Just watched the first episodes of the new seasons of Dollhouse and The Mentalist. The Mentalist has a new character played by Terry Kinney who played Tim McManus in Oz. Oz was a fantastic show. Meanwhile in Dollhouse, Dr Saunders is acting quite strangely and Jamie Bamber guest stars. 

