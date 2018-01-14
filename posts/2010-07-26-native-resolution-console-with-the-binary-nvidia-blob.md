slug: "native-resolution-console-with-the-binary-nvidia-blob"
title: Native resolution console with the binary nVidia blob.
categories:
  - Arch
  - Hardware
  - Linux
published_date: "2010-07-26 11:42:38 +0000"
layout: default.liquid
data:
  layout: post
  comments: true
  author: admin
  wordpress_id: 339
---
My first serious graphics card was a Canopus Pure3D which was based on [3dfx's Voodoo](http://en.wikipedia.org/wiki/3dfx) chipset. It was a strange beast as it installed in addition to the 2D graphics card and interfaced via a pass-through VGA cable. I purchased it solely to play id's GLQuake. When it came time to upgrade I went for a Diamond Viper V770 which was based on the [nVidia RIVA TNT2](http://en.wikipedia.org/wiki/Tnt2) chipset. Since that day I have always used nVidia graphic cards, except for laptops where nVidia was not an option, and am currently using a Geforce 8600GT.

While I have always been happy with nVidia there are some downsides to using nVidia cards in Linux. If you want decent 3D performance you need to use the propriety nVidia binary blob which means you miss out on KMS and have to deal with an out of tree kernel module and a tainted kernel. For the moment I am willing to do this though when it comes time to update again it may be time to look at ATI or perhaps the nouveau project will have attained decent 3D performance by then.

Having seen KMS in action on my Intel based notebook I do miss having a native resolution console on my main workstation and, until recently, thought it was impossible without switching to the nouveau driver. The solution was to install uvesafb. This is a more recent frame buffer than the standard vesafb frame buffer. I followed this Arch Linux [wiki article](http://wiki.archlinux.org/index.php/Uvesafb) and was up and running in no time with a native 1680x1050 console.

While rebooting to test out my new console settings I had a play with [HDT (Hardware Detection Tool)](http://www.hdt-project.org/) which I had just recently added to my boot menu. While using HDT to look at the VESA information I noticed that there was a 1680x1050 graphics mode listed though I didn't make a note of the mode number. I can't help but wonder if I would have been able to get a native resolution console by just using a vga=xxx kernel switch? Uvesa is working fine so I will probably just stick with it.
