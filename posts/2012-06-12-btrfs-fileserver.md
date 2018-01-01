slug: "btrfs-fileserver"
title: BTRFS Fileserver
categories:
  - btrfs
  - Hardware
  - Linux
published_date: "2012-06-12 10:37:19 +0000"
layout: default.liquid
data:
  layout: post
  wordpress_id: 484
  comments: true
  author: admin
---
## Introduction


For the last 9 months or so I have been using a HP N36L Microserver as a home Xen server running a mail relay for my local network. It wasn't doing much else and I figured it was time to rectify that. I wanted to turn it into a backup file server as well as be able to use it as a test bench for things like lxc. The server comes standard with one sata drive with room for three more and a gig of ram. I upgraded the RAM to the maximum of 8GB and filled the drive bays with four 1TB drives I scrounged up. Two of the drives were canabalized from a couple of cheap USB 2 external drives. The server also has two PCIe half height slots so I filled them with an extra gigabit NIC and a USB 3 card.

I decided I wanted to use btrfs as the main filesystem. For anyone else thinking of using btrfs at this stage keep in mind that btrfs is still under heavy development. The btrfs wiki states:



> Btrfs is under heavy development, but every effort is being made to keep the filesystem stable and fast. Because of the speed of development, you should run the latest kernel you can (either the latest release kernel from kernel.org, or the latest -rc kernel.



For this reason I will be using the latest rc kernel and the latest btrfs-progs. For something different I went with Funtoo Linux as the operating system. I had installed and used Gentoo several years ago and had always wanted to check out Funtoo. I used sysrescuecd as my install medium booted from an external USB flash drive.

<!-- more -->



## Partition and file system preparation


Partition each of the 1TB drives using gdisk, creating a single partition per disk:


    
    # sgdisk -p /dev/sda
    Disk /dev/sda: 1953525168 sectors, 931.5 GiB
    Logical sector size: 512 bytes
    Disk identifier (GUID): F56AF812-C74B-4E23-8E9F-5293BB60F3F9
    Partition table holds up to 128 entries
    First usable sector is 34, last usable sector is 1953525134
    Partitions will be aligned on 2048-sector boundaries
    Total free space is 2014 sectors (1007.0 KiB)
    
    Number  Start (sector)    End (sector)  Size       Code  Name
       1            2048      1953525134   931.5 GiB   8300  Linux filesystem




Then create a btrfs RAID10 array across all drives and mount it:


    
    # mkfs.btrfs -d raid10 -m raid10 /dev/sda1 /dev/sdb1 /dev/sdc1 /dev/sdd1
    # mkdir /mnt/btrfs
    # mount /dev/sda1 /mnt/btrfs



If the mount fails try a 'btrfs device scan' before mounting. 

I wanted separate subvolumes for / and /home. I also wanted to keep my subvolumes organized and so adopted the same btrfs layout that Ubuntu has been using for recent releases. The root subvolume will only contain other subvolumes which will be mounted as needed by specifing subvol=* in the mount options.


    
    # cd /mnt/btrfs
    # btrfs create subvolume @
    # btrfs create subvolume @home



I wanted a separate ext2 boot partition though I didn't want it on the main SATA drives. Thankfully the N36L presented an elegant solution through the inclusion of an internal USB port. I used a 4G USB thumb drive to house my boot partition.


    
    # sfdisk -l /dev/sdf
    
    Disk /dev/sde: 3824 cylinders, 64 heads, 32 sectors/track
    Units = cylinders of 1048576 bytes, blocks of 1024 bytes, counting from 0
    
       Device Boot Start     End   #cyls    #blocks   Id  System
    /dev/sde1          1     512     512     524288   83  Linux
    /dev/sde2          0       -       0          0    0  Empty
    /dev/sde3          0       -       0          0    0  Empty
    /dev/sde4          0       -       0          0    0  Empty



and create the ext2 filesystem:


    
    # mkfs.ext2 /dev/sdf1



We can now mount the file systems in preparation for installing Funtoo linux.


    
    # mkdir /mnt/funtoo
    # mount -o subvol=@ /dev/sda1 /mnt/funtoo
    # mkdir /mnt/funtoo/{boot,home}
    # mount /dev/sdf1 /mnt/funtoo/boot
    # mount -o subvol=@home /dev/sda1 /mnt/funtoo/home





## OS install


From this point I followed the Funtoo install guide. Once the system was installed I installed a couple of packages I would need later on, a statically compiled busybox and the btrfs-progs utilities. To install a statically compiled busybox use 'USE='static' emerge busybox'. To ensure we get the latest btrfs-progs to match our kernel unmask the git version of btrfs-progs.


    
    # echo 'sys-fs/btrfs-progs **' >> package.accept_keywords
    # echo 'sys-fs/btrfs-progs' >> package.unmask
    # emerge btrfs-progs



When configuring the kernel make sure to enable btrfs to be compiled in the kernel and enable devtmpfs. devtmpfs will create our device nodes during initramfs execution. Include drivers for your hardware as modules or compiled in. I compiled the radeon module into the kernel along with the cards firmware to enable KMS as early as possible. It's nice to have a native resolution console.



## initramfs


I tried to get away without an initramfs however the kernel would simply not mount a multi device btrfs root file system reliably without running 'btrfs device scan' first. I tried a few things mentioned on the btrfs wiki without luck so an initramfs we will have.

I looked at [dracut](http://fedoraproject.org/wiki/Dracut) and [better-initramfs](http://slashbeast.github.com/better-initramfs/) however both simply did much more than required in this case. Using the [Initramfs page](http://en.gentoo-wiki.com/wiki/Initramfs) on the Gentoo Wiki I crafted a minimal initramfs that performed the device scan, mounted the root file system and switched to it. It contains a statically linked busybox, btrfs and its dependencies and a simple init script.

This is the layout of the initramfs:


    
    # tree initramfs
    initramfs
    ├── bin
    │   └── busybox
    ├── dev
    ├── etc
    ├── init
    ├── lib64
    │   ├── ld-2.13.so
    │   ├── ld-linux-x86-64.so.2
    │   ├── libc-2.13.so
    │   ├── libc.so.6
    │   ├── libpthread-2.13.so
    │   ├── libpthread.so.0
    │   ├── libuuid.so.1
    │   └── libuuid.so.1.3.0
    ├── mnt
    │   └── root
    ├── proc
    ├── root
    ├── sbin
    │   └── btrfs
    └── sys



and the init script:


    
    # cat initramfs/init
    #!/bin/busybox sh
    
    # Mount the /proc and /sys filesystems.
    mount -t proc none /proc
    mount -t sysfs none /sys
    mount -t devtmpfs none /dev
    
    # Let devices settle
    sleep 3s
    
    # Activate multi device btrfs
    /sbin/btrfs device scan
    
    # Mount the root filesystem.
    mount -t btrfs -o ro,subvol=@ /dev/sda1 /mnt/root || recovery_shell
    
    # Clean up.
    umount /proc
    umount /sys
    umount /dev
    
    # Boot the real thing.
    exec switch_root /mnt/root /sbin/init
    
    recovery_shell() {
    	echo "Error mounting root fs."
    	busybox --install -s
    	/bin/sh
    }



The sleep 3s is a bit of a hack to ensure that device node creation has completed before the btrfs device scan takes place. Without this delay some kernels failed to mount the root file system. Output from dmesg showed that this was because device nodes were still being created while the btrfs device scan was executing. I modified /etc/boot.conf and executed boot-update to use my custom kernel and initramfs and rebooted.



## Management


As mentioned earlier, to manage the subvolumes and any snapshots created I need to mount the root subvolume. Only then will I be able to see the subvolumes containing my root and home file systems.


    
    # mkdir /mnt/btrfs-vault
    # mount /dev/sda1 /mnt/btrfs-vault
    # ls -lh /mnt/btrfs-vault
    total 0
    drwxr-xr-x 1 root root 232 Jun  4 13:20 @
    drwxr-xr-x 1 root root  62 Jun  3 16:11 @home



From here I can create new subvolumes or snapshot existing subvolumes all the while keeping things neat.

