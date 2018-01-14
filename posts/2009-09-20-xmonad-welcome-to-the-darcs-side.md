slug: "xmonad-welcome-to-the-darcs-side"
title: XMonad. Welcome to the Darcs side.
categories:
  - Arch
  - Linux
published_date: "2009-09-20 10:50:28 +0000"
layout: default.liquid
data:
  wordpress_id: 138
  layout: post
  author: admin
  comments: true
---
I have been using [xmonad](http://xmonad.org/) as my window manager for quite a
while now. I use it on a dual screen workstation, my laptop and a netbook and in
each case it does the job flawlessly. It is insanely stable and for this reason
I am still using it though I do struggle with Haskell and would like to try
[stumpwm](http://www.nongnu.org/stumpwm/) one day as I would be more at home
with Lisp.

My configuration hasn't changed much for the last 6 months or so, not because my
current setup is perfect, but because it does everything I need. Having said
that I'm sure there are a few productivity boosting modules available in
xmonad-contrib that I would find useful. As a first step today I switched to
XMonad from Darcs instead of the Arch Linux packages. There are packages for
[xmonad-darcs](http://aur.archlinux.org/packages.php?ID=12483) and
[xmonad-contrib-darcs](http://aur.archlinux.org/packages.php?ID=13652) in the
AUR however installing pre release applications from SCMs via ABS feels a little
weird to me. I find it easier to keep a copy of the repository in my home
directory and re-sync and build as needed. While I wanted to install
xmonad-darcs manually I also didn't want to have files installed into my root
file system that were not managed by pacman. Thankfully the xmonad README file
contains instructions for installing xmonad into your home directory. This is a
as simple as:

    runhaskell Setup.lhs configure --user --prefix=$HOME
    runhaskell Setup.lhs build
    runhaskell Setup.lhs install --user

xmonad-contrib can be installed in a similar way. Add ~/bin to the **end** of
your PATH or the configure, compile, restart command normally bound to mod-q
will not work. Now that I am running xmonad-darcs I am free to have a play with
some of the modules that are only available in the current darcs version.
Modules that I am currently configuring or wanting to look at include:


  * XMonad.Actions.GridSelect
  * XMonad.Actions.Submap
  * XMonad.Actions.Search
  * XMonad.Layout.HintedGrid
  * XMonad.Layout.IM
  * XMonad.Layout.LayoutHints
  * XMonad.Layout.PerWorkspace
  * XMonad.Util.NamedScratchpad
  * XMonad.Layout.Tabbed
  * XMonad.Prompt.Man

So far I have only looked at XMonad.Actions.GridSelect, XMonad.Layout.HintedGrid
and XMonad.Actions.Search. Next up will be XMonad.Util.NamedScratchpad.

[Last post](http://blog.sambodata.com/?p=133) I mentioned a pdf viewer called [apvlv](http://code.google.com/p/apvlv/). Recently [another post](http://bbs.archlinux.org/viewtopic.php?id=80458) sprung up about yet another minimalist pdf viewer called [zathura](http://zathura.neldoreth.net/). It looks interesting as well and worth checking out.
