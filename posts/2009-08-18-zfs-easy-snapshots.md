slug: "zfs-easy-snapshots"
title: ZFS easy automated snapshots
categories:
  - OpenSolaris
published_date: "2009-08-18 11:48:29 +0000"
layout: default.liquid
data:
  layout: post
  author: admin
  comments: true
  wordpress_id: 99
---
I have enabled auto snapshots on my OpenSolaris file server. This was easy due
to the already installed **SUNWzfs-auto-snapshot** package. First step is to
enable the required snapshot frequencies:

    mike@argon:~# svcadm enable svc:/system/filesystem/zfs/auto-snapshot:monthly
    mike@argon:~# svcadm enable svc:/system/filesystem/zfs/auto-snapshot:weekly
    mike@argon:~# svcadm enable svc:/system/filesystem/zfs/auto-snapshot:daily


I left the hourly and frequent snapshots disabled:

    mike@argon:~$ svcs -a | grep snapshot
    disabled       Jul_23   svc:/system/filesystem/zfs/auto-snapshot:frequent
    disabled       Jul_23   svc:/system/filesystem/zfs/auto-snapshot:hourly
    online         21:11:30 svc:/system/filesystem/zfs/auto-snapshot:monthly
    online         21:11:30 svc:/system/filesystem/zfs/auto-snapshot:weekly
    online         21:11:35 svc:/system/filesystem/zfs/auto-snapshot:daily


Check the snapshots have been created:

    mike@argon:~$ zfs list -t snapshot -r tank
    NAME                                                     USED  AVAIL  REFER  MOUNTPOINT
    tank@today                                                16K      -    22K  -
    tank@zfs-auto-snap:monthly-2009-08-18-21:11                 0      -    22K  -
    tank@zfs-auto-snap:weekly-2009-08-18-21:11                  0      -    22K  -
    tank@zfs-auto-snap:daily-2009-08-18-21:11                   0      -    22K  -
    tank/data@today                                         21.7M      -  65.9G  -
    tank/data@zfs-auto-snap:monthly-2009-08-18-21:11            0      -  66.1G  -
    tank/data@zfs-auto-snap:weekly-2009-08-18-21:11             0      -  66.1G  -
    tank/data@zfs-auto-snap:daily-2009-08-18-21:11              0      -  66.1G  -
    tank/multimedia@today                                    394K      -  47.4G  -
    tank/multimedia@zfs-auto-snap:monthly-2009-08-18-21:11      0      -  48.5G  -
    tank/multimedia@zfs-auto-snap:weekly-2009-08-18-21:11       0      -  48.5G  -
    tank/multimedia@zfs-auto-snap:daily-2009-08-18-21:11        0      -  48.5G  -

By default auto-snapshots are enabled on all zpools. I have a zpool named
backup1 that I don't want auto snapshots enabled on. They can be disabled by
setting the **com.sun:auto-snapshot** property to false for the zpool:

    mike@argon:~# zfs set com.sun:auto-snapshot=false backup1
    mike@argon:~$ zfs get com.sun:auto-snapshot backup1
    NAME     PROPERTY               VALUE                  SOURCE
    backup1  com.sun:auto-snapshot  false                  local

By default all file systems in this zpool will inherit this setting.

Done. Not bad for 5 minutes work. :)

