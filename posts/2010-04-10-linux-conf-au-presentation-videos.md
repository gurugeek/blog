slug: "linux-conf-au-presentation-videos"
title: linux.conf.au presentation videos
categories:
  - Linux
published_date: "2010-04-10 06:51:28 +0000"
layout: default.liquid
data:
  wordpress_id: 312
  comments: true
  author: admin
  layout: post
---
I recently came across some videos for [2010's linux.conf.au presentations][0].
I haven't watched all of them however a couple of them look interesting. One
that I have watched is **Going mad with MDADM**. I learnt a few things from this
presentation about software raid, most of which are covered in the [md.txt][1]
file distributed with the kernel tree. The most useful piece of info was how to
request a raid array check:

    [root@mercury ~]# echo check > /sys/block/md1/md/sync_action

You can then check the status of the check via `/proc/mdstat` as normal:

    [mike@mercury|~] $ cat /proc/mdstat
    Personalities : [raid1] [raid10]
    md1 : active raid10 sda3[0] sdd3[3] sdc3[2] sdb3[1]
          484231040 blocks 64K chunks 2 near-copies [4/4] [UUUU]
          [=>...................]  check =  6.3% (30890816/484231040) finish=73.7min...

    md0 : active raid1 sda1[0] sdd1[3] sdc1[2] sdb1[1]
          128384 blocks [4/4] [UUUU]

    unused devices: <none>

Checking a modestly sized array can take some time. In the case of a large array
and a RAID level that uses parity such as 5 or 6 it is reported that it can take
a very long time to do a check. Resyncs are even worse as they involve writing
as well as reading.

Check the log file for the status of the completed check:

    [mike@mercury|~/data/scans] $ grep 'kernel: md' /var/log/messages.log | tail -n 5
    Apr 10 16:27:58 mercury kernel: md: data-check of RAID array md1
    Apr 10 16:27:58 mercury kernel: md: minimum _guaranteed_  speed: 1000 KB/sec/disk.
    Apr 10 16:27:58 mercury kernel: md: using maximum available idle IO bandwidth (but not more than 200000 KB/sec) for data-check.
    Apr 10 16:27:58 mercury kernel: md: using 128k window, over a total of 484231040 blocks.
    Apr 10 18:07:10 mercury kernel: md: md1: data-check done.

[0]: http://mirror.linux.org.au/linux.conf.au/2010/
[1]: http://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/tree/Documentation/md.txt
