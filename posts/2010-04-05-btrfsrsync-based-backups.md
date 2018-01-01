slug: "btrfsrsync-based-backups"
title: BTRFS/RSYNC based backups.
published_date: "2010-04-05 03:15:41 +0000"
layout: default.liquid
data:
  author: admin
  layout: post
  wordpress_id: 304
  comments: true
---
As mentioned previously I have recently converted my file server to Linux from Open Solaris and ZFS. I was using ZFS's snapshot capability extensively. My main storage zpool used snapshots to provide read only previous versions of files and my backup zpool used snapshots to provide space efficient read only backups. I wanted something similar in Linux using either rsync with hard links or rsync with btrfs. I decided to try rsync with btrfs.

Btrfs is still far from complete though progress is being made quickly. The btrfs wiki does however state:



> Btrfs is under heavy development, but every effort is being made to keep the filesystem stable and fast. As of 2.6.31, we only plan to make forward compatible disk format changes, and many users have been experimenting with Btrfs on their systems with good results. Please email the Btrfs mailing list if you have any problems or questions while using Btrfs.




I setup a btrfs file system on a 1TB external USB drive.

With error checking removed my backup script is simply:


    
    #!/bin/bash
    
    EXCLUDE_FILE=/home/mike/backup-script/rsync-exclude
    
    rsync -axv --stats --exclude-from=${EXCLUDE_FILE} \
           --delete-after /home/mike/ /mnt/BTRFS/mike
    
    btrfsctl -s /mnt/BTRFS/snapshots/mike-$( date +%Y%m%d-%H%M ) /mnt/BTRFS/mike
    



This results in the following file system layout on the external drive:


    
    
    [mike@mercury|~] $ tree /mnt/BTRFS -L 1
    /mnt/BTRFS
    |-- mike
    `-- snapshots
    
    2 directories, 0 files
    [mike@mercury|~] $ tree /mnt/BTRFS/snapshots -L 1
    /mnt/BTRFS/snapshots
    |-- mike-20100314-1827
    |-- mike-20100314-2337
    |-- mike-20100315-1727
    |-- mike-20100316-0405
    |-- mike-20100317-0621
    |-- mike-20100318-0405
    |-- mike-20100319-0415
    `-- mike-20100320-0411
    



The top level mike directory contains a mirror of my home directory with the snapshots directory containing daily snapshots performed after that days rsync completes. While there are several days worth of backups on the drive, total file system usage is roughly the same as the home directory.


    
    [mike@mercury|~] $ pydf /home /mnt/BTRFS
    Filesystem           Size Used Avail Use%                 Mounted on
    /dev/mapper/vg0-home 350G 177G  173G 50.7 [#######......] /home     
    /dev/sde1            932G 178G  754G 19.1 [##...........] /mnt/BTRFS
    



While btrfs will eventually support read only snapshots through per snapshot block quotas it has not been implemented as of yet. This was one of ZFS's features that I really liked as it was comforting to know that a daily snapshot could not be modified intentionally or unintentionally. Due to btrfs lacking read only snapshots at this point in time it could be argued that using rsync with hardlinks on a proven filesystem such as xfs would be a better way to go.


