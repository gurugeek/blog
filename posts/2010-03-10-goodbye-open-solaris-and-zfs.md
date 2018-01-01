slug: "goodbye-open-solaris-and-zfs"
title: Goodbye Open Solaris and ZFS.
categories:
  - Arch
  - Hardware
  - Linux
  - OpenSolaris
published_date: "2010-03-10 13:42:54 +0000"
layout: default.liquid
data:
  comments: true
  wordpress_id: 276
  author: admin
  layout: post
---
As the title of the post states I have recently decommissioned my Open Solaris ZFS based file server. I have now combined my file server with my workstation. There are several reasons why I have done this:




  * I'm trying to be a little bit greener in regards to my energy usage. I really only connected to my file server from my workstation and lately I had been leaving my workstation on for one reason or another often racking up uptime measured in days. Having two full sized tower machines running seemed wasteful. This was the main reason I decided to combine my file server and workstation.


  * OpenSolaris is a great product however I never really felt comfortable in it. Open Solaris and ZFS make for a rock solid combination. It has been sitting in the corner of my spare bedroom closet and I haven't needed to really touch it. ZFS makes filesystem management and backups ridiculously easy. My backup system consisted of a 12 line bash script that backed up my data to an external usb based zpool which was snapshotted after every run giving me 6 months of daily backups in very little disk space. Due to the stability of this system along with it's low maintenance I slowly forgot all my Open Solaris knowledge, what little there was, that I had gleaned from setting the machine up.



As I run Arch Linux on my workstation and did not want to resort to using ZFS through fuse I had to say good bye to ZFS. I will definitely miss ZFS. Managing storage is really a no brainer with ZFS compared to setting up a comparable Linux based layered software raid > LVM > file systems type setup. I salvaged the disks from my file server, after backing  up the data, and along with the disk from my workstation and a spare I had on hand, this gave me 4 x 250G drives. I partitioned each drive with a 128M boot partition, 2G swap partition and a 248G main partition.


    
    [mike@mercury|~] $ sudo parted /dev/sda print
    Model: ATA ST3250820AS (scsi)
    Disk /dev/sda: 250GB
    Sector size (logical/physical): 512B/512B
    Partition Table: msdos
    
    Number  Start   End     Size    Type     File system     Flags
     1      32.3kB  132MB   132MB   primary  ext2            boot, raid
     2      132MB   2130MB  1999MB  primary  linux-swap(v1)
     3      2130MB  250GB   248GB   primary                  raid
    



The four boot partitions were combined into a RAID1 array while the 4 large partitions were combined into a RAID10 array.


    
    [mike@mercury|~] $ cat /proc/mdstat
    Personalities : [raid1] [raid10] 
    md1 : active raid10 sda3[0] sdd3[3] sdc3[2] sdb3[1]
          484231040 blocks 64K chunks 2 near-copies [4/4] [UUUU]
          
    md0 : active raid1 sda1[0] sdd1[3] sdc1[2] sdb1[1]
          128384 blocks [4/4] [UUUU]
          
    unused devices: <none>
    



I was tossing up between RAID5 or RAID10 and finally came down on the side of RAID10. With RAID10 you sacrifice a drive, storage wise, though you do get the benefit of no parity calculations and no RAID5 write hole. Having said that I'm not sure how much of an issue the parity calculations would have been after seeing ZFS perform fine on some very modest hardware. Linux can perform RAID10 as a single layer which gives you a few more options when setting things up and also allows you to do things like setup a RAID10 array on 3 drives.

On top of the RAID10 array I setup LVM with a separate home logical volume. The final file system layout looks like:


    
    [mike@mercury|~] $ pydf / /boot /home
    Filesystem           Size Used Avail Use%                            Mounted on
    /dev/mapper/vg0-root  63G  24G   36G 38.3 [#########...............] /         
    /dev/md0             121M  16M   99M 13.2 [###.....................] /boot     
    /dev/mapper/vg0-home 350G 176G  173G 50.4 [############............] /home     
    



I chose the following file systems:


    
    [mike@mercury|~] $ mount | grep 'root\|home\|boot' | cut -d ' ' -f 1,3,5  | column -t
    /dev/mapper/vg0-root  /      ext4
    /dev/md0              /boot  ext2
    /dev/mapper/vg0-home  /home  xfs
    



There was no real reasoning behind the selection of ext4 and xfs for root and home respectively. I have used both before and found them to be solid. I left around 50G spare in my volume group which I will probably end up adding to my home file system. Xfs will make this easy as it allows online resizing.

I didn't want to have to reinstall Arch Linux so I used my usual technique of transfering a Linux install using the Arch Linux live CD:




  1. Backing up my root file system to external storage using cp -axv /mnt/src/* /mnt/dest


  2. Adding new drives, partition drives, setup raid arays, setup lvm and create filesystems.


  3. Restore my root file system using another cp -axv /mnt/src/* /mnt/dst



I wanted things to go smoothly so before I backed up my root files system I made sure I had added mdadm to my mkinitcpio hooks array, I already had lvm in there, and recreated my initrd. I also changed my /etc/fstab and /boot/grub/menu.lst to reflect the new filesystem layout. The last step was to chroot into my Arch Linux install and reinstall GRUB and generate a /etc/mdadm.conf. I hadn't realised I needed a /etc/mdadm.conf until trying to boot. I had been roughly following the Arch Linux wiki article [Installing with Software RAID or LVM](http://wiki.archlinux.org/index.php/Installing_with_Software_RAID_or_LVM) and it set me straight. It also included a neat trick with sfdisk to make partitioning multiple disks easy.

All in all it went well. I managed to get the whole thing done during the Oscar's broadcast. I did catch bits and pieces though. Steve Martin and Alec Baldwin did a fantastic job hosting and it was great to see an underdog get up in the form of Hurt Locker.

Next I need to work out a backup system. I would really like something similar to the rsync/ZFS snapshot system I used in Open Solaris. I will probably end up going with something similar using [rsync and hard links](http://www.mikerubel.org/computers/rsync_snapshots/) or I might try btrfs and rsync. Btrfs seems to be coming along and it may be stable enough for use as a backup file system.

