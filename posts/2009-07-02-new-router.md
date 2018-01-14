slug: "new-router"
title: "New Router, dnsmasq and Jeff Golblum!"
categories:
  - Debian
  - Linux
published_date: "2009-07-02 11:20:05 +0000"
layout: default.liquid
data:
  wordpress_id: 43
  author: mike
  comments: true
  layout: post
---
I'm currently running a combined router and file server. In the interests of security I'm in the process of setting up a new router. I'm installing [Debian](http://debian.org/) Lenny on the router as it doesn't get rebooted often (in the past it has gone over a year without being rebooted) and a rolling release distribution such as Arch doesn't suit this usage pattern.

I normally use [dhcpd](https://www.isc.org/downloadables/12) and [bind](https://www.isc.org/software/bind) for serving up dhcp and dns but this time tried [dnsmasq](http://www.thekelleys.org.uk/dnsmasq/doc.html). Dnsmasq is described on it's webpage as:



> Dnsmasq is a lightweight, easy to configure DNS forwarder and DHCP server. It is designed to provide DNS and, optionally, DHCP, to a small network. It can serve the names of local machines which are not in the global DNS. The DHCP server integrates with the DNS server and allows machines with DHCP-allocated addresses to appear in the DNS with names configured either in each host or in a central configuration file. Dnsmasq supports static and dynamic DHCP leases and BOOTP/TFTP/PXE for network booting of diskless machines. 



Setup is dead easy. Add your local machines to _/etc/hosts_ and check that you have a dns server listed in your _/etc/resolv.conf_ file. I used OpenDNS's servers for this. Once this is done you will be able to resolve global names and local names. Much easier than mucking around with bind zone files. I wish I had tried it earlier.

Earlier this week one of Australia's adopted, he is actually from New Zealand, prize idiots [Richard Wilkins](http://en.wikipedia.org/wiki/Richard_Wilkins_(TV_presenter)) reported the untimely death of [Jeff Goldblum](http://en.wikipedia.org/wiki/Jeff_Goldblum). Stephen Colbert's [response](http://www.colbertnation.com/the-colbert-report-videos/220019/june-29-2009/jeff-goldblum-will-be-missed) is fitting and quite funny.
