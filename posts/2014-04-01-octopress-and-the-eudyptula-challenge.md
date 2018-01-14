title: Octopress and the Eudyptula Challenge
categories:
  - Linux
published_date: "2014-04-01 21:56:07 +1100"
layout: default.liquid
data:
  comments: true
  layout: post
---
I have been really slack with updating my Blog. Three posts last year and this
is my first post this year. I have been quite busy with a new job, getting used
to a new city, etc. I don't feel *too* guilty.

## Octopress!

A couple of weekends ago I converted my blog from Wordpress to [Octopress][1].
Octopress is a static blog generator which uses [Jekyll][4]. The conversion went
fairly smoothly. I used [exitwp][2] to export my posts and switched to
[Disqus][5] for comments. There are some formatting issues I need to fix, mainly
with code blocks and links, however overall things look pretty good. I like
being able to use markdown for my posts.

Octopress has a development server builtin which makes previewing content easy.
Just run `rake preview` and this will spawn a Webrick server that watches your
source files for changes and automatically regenerates the preview when
required.

Apart from cleaning up the imported posts I would like to look at tweaking the
theme a little. The default theme is quite nice and looks great on any size
screen though it would be nice to make it a little distinctive.

## Eudyptula Challenge

I came across the [Eudyptula Challenge][6]  on Greg K. Hartman's [Google+][3]
page. Not sure who is organising it though I am finding it fun. Unfortunately my
C is quite rusty[^1]. I'm currently on Task 15 and have been busy compiling
kernels, writing modules and kernel patches. Kernel patches have included adding
files to `/proc/<PID>/` and adding a new syscall.

For one of the tasks I found myself wanting to be able to quickly boot a kernel
with a minimal initramfs for testing. I wrote a couple of simple wrapper scripts
for generating the initramfs and booting the kernel using kvm with console
output going directly into the terminal for easy cut and paste. These scripts
were based on a [blog post][7] by Stefan Hajnoczi and can be found [here][8].

[1]: http://octopress.org
[2]: https://github.com/thomasf/exitwp
[3]: https://plus.google.com/111049168280159033135/posts/6FtxJoaexro
[4]: http://jekyllrb.com
[5]: http://disqus.com/
[6]: http://eudyptula-challenge.org/
[7]: http://blog.vmsplice.net/2011/02/near-instant-kernel-development-cycle.html
[8]: https://github.com/mfs/kernel-dev

[^1]: Rusty as in, I am out of practice. Not that it looks like it was coded by [Rusty](http://rusty.ozlabs.org/).
