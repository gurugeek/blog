slug: "gpt-raid1-setup"
title: GPT RAID1 setup.
categories:
  - Hardware
  - Linux
published_date: "2011-02-09 08:33:00 +0000"
layout: default.liquid
data:
  wordpress_id: 387
  author: admin
  comments: true
  layout: post
---
My home partition hit 90% recently and I decided it was time to upgrade my storage. I had been running 4x250GB hard disks in a RAID1/RAID10 setup and switched to a 2x1TB RAID1 setup. The drives were Western Digital Advanced Format drives with 4kB physical sectors and 512B logical sectors. It is important when partitioning these drives to ensure that partitions start on a logical sector number that is divisible by 8 otherwise performance will suffer as file system clusters will not be aligned to the underlying 4kB physical sector. If the partition is not aligned then writes turn into a read-modify-write. I also decided to use a GPT instead of an MBR partition table. I used gdisk to create boot, swap, root and home partitions:


    
    $ sudo sgdisk -p /dev/sda
    Disk /dev/sda: 1953525168 sectors, 931.5 GiB
    Logical sector size: 512 bytes
    Disk identifier (GUID): 0658FE0C-AE06-4FD9-8CCC-7DDB178BB0CA
    Partition table holds up to 128 entries
    First usable sector is 34, last usable sector is 1953525134
    Partitions will be aligned on 2048-sector boundaries
    Total free space is 3437 sectors (1.7 MiB)
    
    Number  Start (sector)    End (sector)  Size       Code  Name
       1            2048          264191   128.0 MiB   FD00  Linux RAID
       2          264192        12847103   6.0 GiB     8200  Linux swap
       3        12847104       147064831   64.0 GiB    FD00  Linux RAID
       4       147064832      1953523711   861.4 GiB   FD00  Linux RAID
    



I actually aligned the start of all my partitions to 1MiB (2048 sectors) which seems to be somewhat of an industry standard and was suggested by gdisk as well.

I had done some research into how to exactly boot Linux on a GPT disk and was happy to find that my boot loader, extlinux, works out of the box. Installing extlinux was as simple as:


    
    # extlinux --raid --install /boot/extlinux
    # sgdisk /dev/sda --attributes=1:set:2
    # sgdisk /dev/sdb --attributes=1:set:2
    # cat /usr/lib/syslinux/gptmbr.bin > /dev/sda
    # cat /usr/lib/syslinux/gptmbr.bin > /dev/sdb
    



These commands install ldlinux.sys, set the bootable attribute for partition 1 of both disks and install the gptmbr.bin boot code into the first sector of both disks.

Once my file systems were copied onto my new disk setup I recreated my initrd images with a new /etc/mdadm.conf and edited /etc/fstab and /boot/extlinux/extlinux.conf. The Arch Linux Wiki article [Installing with Software RAID or LVM](https://wiki.archlinux.org/index.php/Installing_with_Software_RAID_or_LVM#Activate_existing_RAID_devices_and_LVM_volumes) proved handy for assembling the arrays.

I now have plenty of disk space again:


    
    $ pydf / /boot /home
    Filesystem Size Used Avail Use%                               Mounted on
    /dev/md1    63G  31G   29G 48.6 [#############..............] /         
    /dev/md0   124M  17M  101M 13.6 [####.......................] /boot     
    /dev/md2   861G 373G  488G 43.3 [############...............] /home



A quick check with Bonnie++ showed that disk performance was as expected which confirmed that I hadn't made any mistakes with partition alignment.


