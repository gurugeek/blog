slug: "postfix-reject_rbl_client-and-google-dns"
title: "Postfix, reject_rbl_client and Google DNS"
published_date: "2010-02-12 11:01:34 +0000"
layout: default.liquid
data:
  author: admin
  comments: true
  layout: post
  wordpress_id: 270
---
Just spent 30 minutes trying to work out why Postfix was not rejecting mail using the zen.spamhaus.org DNSBL. I finally found the answer in my postfix-users mailing list archives.

I was using [Google Public DNS](http://code.google.com/speed/public-dns/). [Spamhaus.org](http://www.spamhaus.org/) simply returns NXDOMAIN for all queries from Google's Public DNS servers. I quickly setup [pdns-recursor](http://wiki.powerdns.com/trac) and the test message was correctly blocked.

I wasn't familiar with pdns-recursor, it was mentioned in the same postfix-users mailing list thread, however it was easy to install and worked out of the box. I was using dnsmasq for resolving local machine names so I will need to fix that at some point. It looks like pdns can do that as well through pdns-server or I might just bite the bullet and install BIND. I did like the ease of configuration of dnsmasq compared to BIND though. Hopefully pdns-server is somewhere in between the two.
