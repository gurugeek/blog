slug: "arch-linux-net-tools-depreciated"
title: "Arch Linux net-tools depreciated."
categories:
  - Arch
  - IPv6
  - Linux
published_date: "2011-06-10 08:49:46 +0000"
layout: default.liquid
data:
  layout: post
  comments: true
  author: admin
  wordpress_id: 460
---
Arch Linux has depreciated the usage of net-tools in `/etc/rc.conf` in favor of
iproute2. The new syntax in `/etc/rc.conf` makes it simple to setup a single
interface though anything complicated is best done through netcfg. I didn't
really want to use netcfg for my main workstation as it's network setup never
changes and it seemed like overkill. Unfortunately I did need to configure IPv6
related settings and this was no longer easily done in rc.conf. The two items I
needed to set were the IPv6 address and the interface MTU. (If I don't set an
MTU of 1280-1480 I have trouble with my IPv6 tunnel.)

I decided to remove the IPv6 address setting completely and rely on radvd
running on my router. This took care of my workstation's IPv6 address and
default route. I really should have done this earlier.

I started patching the `/etc/rc.d/network` script to take an mtu variable but
started to have second thoughts. Most users would not need it and it seemed a
shame to complicate the `network_up()` function. There was a good chance the
patch would not be accepted. In the end I just added the line:

	ip link set dev eth0 mtu 1280

to `/etc/rc.local`. This worked fine.
