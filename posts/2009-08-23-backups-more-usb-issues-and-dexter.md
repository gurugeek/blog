slug: "backups-more-usb-issues-and-dexter"
title: "Backups, more USB issues and Dexter!"
categories:
  - OpenSolaris
published_date: "2009-08-23 11:36:28 +0000"
layout: default.liquid
data:
  comments: true
  wordpress_id: 113
  layout: post
  author: admin
---
With automatic snapshots working I spent time this weekend getting backups working. Initially I thought that a solution based on ZFS's send, receive commands would be the way to go however after thinking of how this would work I couldn't see a clean way of doing this. This is possibly due to my lack of understanding and experience with ZFS though I felt a little better when I read the [ZFS Best Practices Guide](http://www.solarisinternals.com/wiki/index.php/ZFS_Best_Practices_Guide#ZFS_Backup_.2F_Restore_Recommendations).



> The zfs send and receive commands are not enterprise-backup solutions. The receive operation is an all-or-nothing event, you can get all of a snapshot or none of it. If you store the output of zfs send on a file or on tape, and that file becomes corrupted, then it will not be possible to receive correctly and none of the data will be recoverable. See RFE CR6736794. Enterprise backup solutions, as well as other copying methods, such as cp, tar, rsync, pax, cpio, and so on, are more appropriate for backup/restore than zfs send/receive.



After giving it some thought I decided to go with rsync as my main backup solution. I rsync my data to a ZFS formatted USB external hard drive once a day and then snapshot the data. At first performing snapshots on the backup drive felt like a duplication of work. I have daily snapshots on my data zpool, why can't I use those via ZFS send, receive? I eventually came to the realisation that performing the snapshots on the backup zpool was the most versatile way to go as I could backup data from a non ZFS file system and still be able to use snapshots to efficiently store daily backups. In addition, snapshots are fast in ZFS so performing them on the backup drive in addition to the main data zpool isn't a major concern resource wise. I'm not convinced this is the best way to backup a ZFS system but it works well for me at the moment.

I had intended to use a couple of [250GB Seagate Portable Drives](http://www.seagate.com/www/en-us/products/external/expansion/expansion_portable/) in rotation to store my backups on but unfortunately they gave me nothing but grief on OpenSolaris. The first rsync would work fine however the second rsync would just hang. At this point the log files would show a _device disconnect_ error from memory. I have disabled the on-board USB and am using a supported PCI USB card due to [previous problems](http://blog.sambodata.com/?p=77) with the USB chipset on my motherboard. I started to have doubts about the drives themselves so tried a couple of backups to an old 320GB [Maxtor One Touch III](http://www.seagate.com/ww/v/index.jsp?locale=en-US&vgnextoid=cc03004f9f650110VgnVCM100000f5ee0a0aRCRD&vgnextchannel=dbe13d317972b010VgnVCM100000dd04090aRCRD) I had on hand. This drive worked perfectly. I'm not sure exactly what the issue with the Seagate drives is as they seem to work fine in Linux. Might need to look at one of the newer Maxtor One Touch 4s though as Maxtor is now owned by Seagate these may be significantly different to the III series.

Other than swearing at a couple of hard drives I spent the weekend working on an assignment on Knowledge Management. I did take a break at one point to watch the first episode of Dexter season 4. It looks like it is going to be a great season. A new serial killer is introduced played by an actor probably best known for his comedic talents though he has played psychotic characters before. Once opposite Denzel Washington and once opposite Sylvester Stallone.

