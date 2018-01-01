slug: "lvm-setup-and-desktop-virtualisation"
title: LVM setup and desktop virtualisation.
categories:
  - Arch
  - Linux
published_date: "2009-12-22 06:12:08 +0000"
layout: default.liquid
data:
  wordpress_id: 240
  layout: post
  author: admin
  comments: true
---
I have been running lvm on my laptop for a while now and have had no problems with it so decided to transfer my main workstation to it. I decided to have a separate home logical volume with the option of trying out btrfs at some point. Still using ext4 for the time being. Everything went smoothly except I forgot to add the home logical volume to my fstab. It was interesting to see what happens when your listed home directory doesn't exist. :)

At work I have been using the Windows 7 RC since it was released. I haven't had a chance to update to Windows 7 proper which hasn't bothered me as we are rolling out new laptops during January 2010. I have found Windows 7 RC to be a fairly solid OS however I know I would be more productive in Linux for many tasks. I do much of my work either via SSH or web interfaces. There are some things where I do need Windows though. I am giving serious thought to running Arch Linux on my work laptop with Windows 7 running in VirtualBox. There are disadvantages to this however I think the advantages will outweigh them. While I have been impressed with Windows 7(RC)'s stability, sooner or later all Windows installs seem to suffer bit rot and require reformatting and reinstalling. This does not seem to happen with Linux. By running Windows in a VM, this will allow me to keep a snapshot of the VM in a pristine state in case a reformat is required. If for some reason a reformat is still required then at least I still have Arch Linux installed and usable while Windows 7 reinstalls into a VM.

I have been reading through the VirtualBox manual and am impressed at just what it is capable of doing. Two useful features of the non open source version are the built in RDP server and iSCSI initiator. These allow you to move the storage used by the VM and the VirtualBox instance off your local machine and onto a server. The RDP server also allows you to forward local USB devices, such as thumb drives, over the connection. I will probably be keeping everything on the laptop but may make use of the RDP server. One idea is to use VBoxHeadless to start up my Windows 7 VM and maybe a Windows XP VM when I login. I could then connect to them as needed using RDP.

First step is to test out Windows 7 in VirtualBox on my personal laptop and see how it fares performance and reliability wise. Will do that this week.
