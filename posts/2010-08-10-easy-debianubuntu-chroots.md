slug: "easy-debianubuntu-chroots"
title: Easy Debian/Ubuntu chroots.
categories:
  - Arch
  - Debian
  - Linux
published_date: "2010-08-10 10:56:08 +0000"
layout: default.liquid
data:
  wordpress_id: 344
  author: admin
  layout: post
  comments: true
---
Until recently I had been using Adobe's 64 bit Flash Player plugin.
Unfortunately the 64 bit Flash Player has been [removed from Adobe's site][0]
with no clear indication of exactly when it will return. For a while I continued
to use the last released version however this version contains major security
vulnerabilities and I eventually removed it. I don't use Flash that much but I
did miss it every now and then so decided to do something about it. I'm not a
fan of multi-lib setups or nspluginwrapper so went with a 32 bit chroot. At
first I tried an Arch Linux 32 bit chroot however I experienced issues with some
GUI applications not working correctly and segfaulting. While I look into this I
installed a Ubuntu Lucid 32 bit chroot using debootstrap.

Debootstrap is a handy way to install a Debian/Ubuntu base system into a
directory. It is available in the [AUR][1]. Installing a base Lucid 32 bit
system is as easy as:

	[mike@mercury|~] $ sudo mkdir /opt/lucid32
	[mike@mercury|~] $ sudo debootstrap --arch=i386 lucid /opt/lucid32/

To manage entering the chroot I installed schroot. Schroot allows a normal user
to enter a chroot. It also handles copying files from the host into the chroot
as well as mounting any required filesystems inside the chroot. This comes in
handy for keeping `/etc/{resolv.conf,hosts,passwd,shadow}` and so on in sync as
well as mounting `/{proc,dev,tmp}` inside the chroot. I also mount my
`/usr/share/fonts` directory this way so Chromium has access to my fonts. I still
need to do something similar with my mouse cursor icons. Once configured schroot
allows me to launch my 32 bit web browser via:

	[mike@mercury|~] $ schroot -c lucid32 -- chromium http://www.archlinux.org

I now have flash working again.

[0]: http://labs.adobe.com/technologies/flashplayer10/64bit.html
[1]: http://aur.archlinux.org/packages.php?ID=2970

