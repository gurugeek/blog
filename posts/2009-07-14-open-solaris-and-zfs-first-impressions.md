slug: "open-solaris-and-zfs-first-impressions"
title: Open Solaris and ZFS First Impressions
categories:
  - OpenSolaris
published_date: "2009-07-14 12:56:59 +0000"
layout: default.liquid
data:
  wordpress_id: 67
  author: mike
  layout: post
  comments: true
---
Managed to finally copy my data off my file server with no errors and install OpenSolaris 06.2009. Previously I was using an [Adaptec AAR-1210SA](http://www.adaptec.com/en-US/support/raid/sata/AAR-1210SA/) sata controller as the mainboard pre dates SATA. Unfortunately this card is not supported by OpenSolaris and had to be replaced with a [Sunix SATA4000](http://www.sunix.com.tw/it/en/Product_Detail.php?cate=2&class_a_id=34&sid=367) SATA controller. Out of the box this card does not work with OpenSolaris but if you re-flash the cards BIOS with the base 5.4.0.3 BIOS available [here](http://siliconimage.com/support/searchresults.aspx?pid=28&cat=15&os=0) as described at Sun's [BigAdmin](http://www.sun.com/bigadmin/hcl/data/components/details/2997.html) Portal it seems to work fine.

My file server is an aging Athlon XP 2100 1.7GHz single cored CPU. It took quite a while to install OpenSolaris and boot times are less than impressive. Once it is running it seems fine though. OpenSolaris is a strange beast compared to Linux. Same ideas but implemented differently. Even a simple task such as configuring a static IP address on a network interface is completely different to Arch or Debian Linux. The [OpenSolaris Bible](http://www.amazon.com/OpenSolaris-Bible-Wiley-Nicholas-Solter/dp/0470385480/ref=pd_bbs_sr_1?ie=UTF8&s=books&qid=1232485930&sr=8-1) has been a great help.

Once the OS was installed I setup my main data zpool. I mainly followed [A Home Fileserver using ZFS](http://breden.org.uk/2008/03/02/a-home-fileserver-using-zfs/) for this. I copied all my data back and exported my file systems via NFS and mounted them on my Arch workstation. It's difficult to be sure but I think I am taking a performance hit especially on writing. I should have performed some benchmarks before reformatting to compare with the performance figures I am getting now. There are quite a few settings that can be tweaked with ZFS so I may be able to improve the speed a bit. ZFS looks impressive. I have yet to play around with snapshots or volumes though I intend to do so. ZFS's snapshot feature and send and receive commands should make implementing backups fairly simple.

I have also finished configuring [Postfix](http://www.postfix.org) on my mail server. This is the first time I have configured a mail server for anything but basic local delivery. The Postfix web page is a wealth of information.
