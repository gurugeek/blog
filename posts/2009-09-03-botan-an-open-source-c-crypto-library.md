slug: "botan-an-open-source-c-crypto-library"
title: "Botan - an open source C++ crypto library."
categories:
  - Linux
published_date: "2009-09-03 12:16:41 +0000"
layout: default.liquid
data:
  comments: true
  wordpress_id: 124
  layout: post
  author: admin
---
Currently working on an assignment for my Internet Security unit and needed an
open source crypto library. Had a look at a couple I already had installed on my
workstation and a couple of others including:

  * [Crypto++](http://www.cryptopp.com/)
  * [libmcrypt](http://freshmeat.net/projects/libmcrypt/)
  * [libgcrypt](http://directory.fsf.org/project/libgcrypt/)
  * [polarssl](http://www.polarssl.org/)
  * [Botan](http://botan.randombit.net/news/)

Polarssl looked interesting though in the end I have decided on the Botan C++
library for a couple of reasons:

  * It's a C++ library and the application must be written in C++.
  * The included documentation is great and includes build instructions, and a tutorial.
  * Examples included.
  * It builds clean, i.e. no warnings, with -Wall. I don't understand why more developers
  don't pay attention to this.
  * Actively maintained. Last release was the 13th of August, 2009.
  * Project has a good website.

Just downloaded it and built the library on Arch x86_64 with no problems. Final
target machine is also x86_64 so it looks good. Not sure if I should go with a
dynamic or static library. I don't have root access to the final target machine
so would need to use LD_LIBRARY_PATH to allow a dynamic library to load from a
non standard directory.
