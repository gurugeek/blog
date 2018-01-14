slug: "macbook-efi-gpt-and-grub2"
title: "Macbook, EFI, GPT and Grub2"
categories:
  - Arch
  - Hardware
  - Linux
published_date: "2009-11-07 08:01:38 +0000"
layout: default.liquid
data:
  comments: true
  author: admin
  layout: post
  wordpress_id: 179
---
As mentioned [earlier](http://blog.sambodata.com/?p=172) I am currently using a MacBook (3.1) as my main laptop. Everything is working fine except there is a noticeable delay when the machine is turned on until the boot loader, grub 0.97, appears. It is not a huge hassle but it is a little irritating. I wanted to setup lvm on my laptop so I took the opportunity to play around to try to remove this delay. Unfortunately I was not successful.

I found an interesting [blog post](http://blog.christophersmart.com/2009/07/23/linux-on-an-apple-xserve-efi-only-machine/) describing the Mac EFI boot process. Using this as a guide I used parted to repartition sda as a GPT disk. I then created an EFI System Partition formatted as fat32. Inside this partition I created the required /efi/boot directory structure. I downloaded the latest Grub2 and compiled it as an EFI executable and installed it and all it's required modules along with a linux kernel into the /efi/boot directory.

The result was the machine successfully booted into Grub 2 and loaded the linux kernel however there was still a significant delay between power on and Grub 2 appearing.

I'm guessing that this delay is caused by one of two things:




	
  * Apple uses a [hybrid](http://www.rodsbooks.com/gdisk/hybrid.html), non standard, MBR/GPT partitioning scheme. Maybe this is causing an issue for the Apple firmware?

	
  * The EFI System Partition on a MacBook is actually empty. Apple's firmware can boot OS X directly from the hfs+ system partition. Perhaps the firmware is looking for a hfs+ OS X system partition first?



Both of these are guesses and could both be incorrect. I don't understand why either of these would take as long as the delay I am seeing. For the moment I am putting up with the delay as there doesn't seem to be an easy way around it without installing OS X and/or rEFIt alongside Linux. I might have another go using an external hard drive at some point. I am at least now using lvm. I haven't used lvm before and am looking forward to playing around with it. As I understand it, lvm's snapshots have major performance issues which is a shame as this is something I would have used.


