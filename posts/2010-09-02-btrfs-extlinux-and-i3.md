slug: "btrfs-extlinux-and-i3"
title: "BTRFS, EXTLINUX and i3."
categories:
  - Arch
  - Linux
published_date: "2010-09-02 05:24:56 +0000"
layout: default.liquid
data:
  wordpress_id: 351
  author: admin
  layout: post
  comments: true
---
Lately I have been experimenting with some of the netbook specific Linux distros
on my Dell Mini 9. There are several distos aimed at netbooks available.
I looked at [Moblin][0], [ChromeOS][1], [Ubuntu Netbook
Remix][2] and [Jolicloud][3]. They were all fairly impressive though I did miss
the simplicity and versatility of Arch Linux and decided to reinstall Arch
Linux. I installed Arch Linux to a single ext4 partition with a 512M swap
partition. I had been having some trouble with the Broadcom wireless adapter so
I replaced it with a spare Intel wireless adapter I had on hand. No more out of
tree kernel modules as the Broadcom card required the wl driver from AUR which
made kernel updates interesting if I didn't have an ethernet port handy.

I recently watched a webcast by Chris Mason the lead developer of btrfs and one
of the things covered was the ability to convert extX to btrfs via
btrfs-convert. I decided to give this a try and on a 16GB SSD, with 2GB used, it
only took a minute or so to convert. One nifty feature is that the btrfs is
created in the empty part of the extX filesystem with the extX filesystem being
saved as a sparse file inside a dedicated subvolume:

	[mike@mini|~] $ sudo btrfs subvolume list /
	ID 256 top level 5 path ext2_saved
	[mike@mini|~] $ ls -lh /ext2_saved/
	total 1.9G
	-r-------- 1 root root 14G Jan  1  1970 image

This makes it possible to revert to the extX filesystem at any point if btrfs
does not live up to expectations. The process is explained much more clearly on
the [btrfs wiki][4].

As usual of late I installed extlinux as a boot loader. Due to the COW nature of
btrfs extlinux installs in a slightly different manner to extX. Instead of
installing as a file inside the filesystem it installs in the slack space inside
the partition directly following the boot sector:

> btrfs has to install the ldlinux.sys in the first 64K blank area, which
is not managered by btrfs tree, so actually this is not installed as files.
since the cow feature of btrfs will move the ldlinux.sys every where

I normally use Xmonad as my WM however I wanted something a little slimmer
dependency wise for my netbook. I looked at [scrotwm][5] for a while and
eventually ended up with [i3][6]. So far I am impressed with it. It works pretty
much out of the box though I am intending to customize it a little more. i3 is
written in C, has great documentation and features a simple IPC interface for
control/status integration with external apps. It also has good multi-monitor
support though I haven't tried it yet. I may give it a trial on my main
workstation.

[0]: http://moblin.org/
[1]: http://chromeos.hexxeh.net/
[2]: http://www.ubuntu.com/netbook
[3]: http://www.jolicloud.com/
[4]: https://btrfs.wiki.kernel.org/index.php/Conversion_from_Ext3
[5]: http://www.scrotwm.org/
[6]: http://i3.zekjur.net/
