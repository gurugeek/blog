slug: "sudo-vim-vs-sudoedit"
title: sudo vim vs sudoedit
published_date: "2009-11-11 07:25:27 +0000"
layout: default.liquid
data:
  wordpress_id: 187
  layout: post
  author: admin
  comments: true
---

Normally when I need to edit a system file I use `sudo vim <filename>`. I was
aware that there was a command called `sudoedit` that could be used to do the
same task as in, `sudoedit <filename>`. I always assumed the later was just a
convenient way to do the former however a post on the Vim mailing list prompted
me to check out the documentation.

It turns out that the later command is much preferred from a security
standpoint. `sudo vim <filename>` actually runs vim, and any scripts loaded, as
root whereas `sudoedit <filename>` runs vim as your user. All editing is done on
a temp file which is copied into position when editing is completed. This means
that any bugs in vim, or loaded scripts, don't execute as root, limiting the
damage they can do.

From now on I am going to try to break the habit of using `sudo vim <filename>`.
