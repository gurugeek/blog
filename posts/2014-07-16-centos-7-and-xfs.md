title: CentOS 7 and XFS
published_date: "2014-07-16 19:20:19 +1000"
layout: default.liquid
data:
  comments: true
  layout: post
---
CentOS 7 has been out for a while now and brings a couple of notable new
features. Two of the main changes to previous versions is systemd and XFS as the
default file system. If you are like me you probably just did a default install
of CentOS 7 to see what it is like. If you find you like it you may want to
adjust the partitioning/LVM layout from the default after the fact.  The default
partitioning scheme is to allocate half the disk to the home LV and the other
half of the disk to the root LV. In my case I didn't want a separate home LV and
I wanted to shrink the root LV. Removing the home LV is easy:

    # umount /home
    # lvremove /dev/centos/home

In addition, remove the `/etc/fstab` entry for `/home`.

Shrinking the root LV is a little more difficult as, unlike ext4, it is not
possible to shrink an XFS file system, mounted or otherwise. What we can do is
create a new root LV and copy the existing root file system to it. First create
our new LV and format it:

    # lvcreate -L 8G centos -n root_new
    # mkfs.xfs /dev/centos/root_new

While it is possible to copy the root file system while the system is live, it
is much easier to do this next step from a livecd such as SystemRescue CD.

First mount the file systems:

    # mkdir /mnt/{root,root_new}
    # mount /dev/centos/root /mnt/root
    # mount /dev/centos/root_new /mnt/root_new

Then copy the root file system to the new LV. Note the flags passed to rsync! -A
and -X ensure that ACLs and extended attributes are copied. Without the later
you will have issues with SELinux.

    # rsync -axvAX /mnt/root/ /mnt/root_new

Then unmount the file systems and move the original root LV out of the way and
rename the new root LV:

    # umount /mnt/root
    # umount /mnt/root_new
    # lvrename centos root root_old
    # lvrename centos root_new root

You should now be able to reboot. Delete the `root_old` LV once your are
confident your new root file system is complete and functioning correctly.

