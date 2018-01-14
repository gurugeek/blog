slug: "bugger-poor-zfs-send-recieve-performance"
title: Poor ZFS send recieve performance.
categories:
  - OpenSolaris
published_date: "2009-07-16 17:18:48 +0000"
layout: default.liquid
data:
  comments: true
  wordpress_id: 77
  layout: post
  author: mike
---
Before installing OpenSolaris on my file server while I was running Linux I
noticed weird behaviour with the on board USB 2.0 controller of my ASUS A7V333.
For some reason I was only getting USB 1.1 transfer speeds with an external USB
hard drive. I didn't think much of it at the time however after trying a ZFS
send - receive I was getting awful performance on OpenSolaris too. A check of
the log files found this:

    Jul 16 21:34:26 argon usba: ... (ehci0): Due to recently discovered incompatibilities
    Jul 16 21:34:26 argon usba: ... (ehci0): with this USB controller, USB2.x transfer
    Jul 16 21:34:26 argon usba: ... (ehci0): support has been disabled. This device will
    Jul 16 21:34:26 argon usba: ... (ehci0): continue to function as a USB1.x controller.
    Jul 16 21:34:26 argon usba: ... (ehci0): If you are interested in enabling USB2.x
    Jul 16 21:34:26 argon usba: ... (ehci0): support please, refer to the ehci(7D) man page.
    Jul 16 21:34:26 argon usba: ... (ehci0): Please also refer to www.sun.com/io for
    Jul 16 21:34:26 argon usba: ... (ehci0): Solaris Ready products and to
    Jul 16 21:34:26 argon usba: ... (ehci0): www.sun.com/bigadmin/hcl for additional
    Jul 16 21:34:26 argon usba: ... (ehci0): compatible USB products.

The fact I was experiencing trouble with this controller in Linux as
well doesn't give me a good feeling about using this workaround. I think I will
look around tomorrow for a USB PCI card.
