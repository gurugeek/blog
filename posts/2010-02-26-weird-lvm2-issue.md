slug: "weird-lvm2-issue"
title: Weird lvm2 issue.
published_date: "2010-02-26 07:47:59 +0000"
layout: default.liquid
data:
  comments: true
  author: admin
  layout: post
  wordpress_id: 273
---
Booted my machine today and it failed to find my LVM filesystems. Took a while to track down the issue. I didn't have lvm2 in my HOOKS array.

Pacman's logs show that the lvm2 hook was not processed when I upgraded to kernel26 (2.6.32.9-1) on 2010-02-25, however it was when kernel26 (2.6.32.8-1) was installed on the 2010-02-13.

I'm not sure what happened in between. I may have deleted the wrong file when cleaning up pacnew files perhaps.
