slug: "configs-and-testing"
title: "Configs and [testing]."
published_date: "2009-12-15 10:23:55 +0000"
layout: default.liquid
data:
  author: admin
  layout: post
  comments: true
  wordpress_id: 235
---
As mentioned [earlier](http://blog.sambodata.com/?p=227) I have finally put most of my configs into a public git repo. After using it for a couple of weeks or so I don't know how I lived without it. It is so handy to have the same aliases, vimrc, xmonad.hs, etc on every machine I use. I use a simple BASH script to manage a set of symbolic links to the actual configs. Thanks to this setup I can download and install my configs using two simple commands:

`git clone <url>
./cmgr link`

I'm currently travelling for work so decided to kill some time by upgrading my laptop to [testing]. I knew the kernel upgrade to 2.6.32 was going to be a little tricky as I use an out of tree kernel module for my wireless interface. The same interface I was using for network access. I didn't have easy access to a wired connection. I knew I would need to install kernel26-headers to allow me to build my wireless module. Unfortunately I installed kernel-headers instead. kernel-headers was already installed and this should have tipped me off that I had installed the wrong package. Unfortunately, I missed it. Fortunately, I had my work laptop with me and used it to download the kernel26-headers package to a USB thumb drive. Apart from that the upgrade went well.
