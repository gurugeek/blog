slug: "upgrading-debian-squeeze-to-wheezy-on-an-alix2"
title: Upgrading Debian Squeeze to Wheezy on an ALIX2.
categories:
  - Debian
  - Linux
published_date: "2013-05-01 10:47:39 +0000"
layout: default.liquid
data:
  comments: true
  author: admin
  layout: post
  wordpress_id: 550
---
The last machine on my home network still running Debian Squeeze was my [PC
Engines ALIX2][0] based router. The ALIX2 doesn't have any video output though
it does send all bios output to the serial port. I also configured grub, the
kernel and getty to all use the serial console for input/output by following the
instructions in the [Remote Serial Console HOWTO][1].  While I did make sure I
had a USB-to-serial adapter handy, I didn't require it and managed to complete
the upgrade via SSH.

I followed the Debian Wheezy upgrade instructions without incident. The only
tricky item was the kernel:

> Debian's 686 kernel configuration has been replaced by the 686-pae
> configuration, which uses PAE (“Physical Address Extension”). If your computer
> is currently running the 686 configuration but does not have PAE, you will
> need to switch to the 486 configuration instead.

You can check for PAE support by checking /proc/cpuinfo. The Debian instructions
provide a simple grep command you can copy and paste to look for the correct
flag. The ALIX2 doesn't support PAE so I removed the installed kernel and
replaced it with kernel-image-486 as instructed. The grub config was
automatically updated though I did double check. You can run update-grub to
update grub if required.

After a reboot the system came up with the new kernel running Debian Wheezy.
Debian Wheezy is still a couple of days away from official release however I
have been running it for weeks on a laptop, a kvm host and a bunch of virtual
machines without any issues.

[0]: http://www.pcengines.ch/alix2d3.htm
[1]: http://www.faqs.org/docs/Linux-HOWTO/Remote-Serial-Console-HOWTO.html
