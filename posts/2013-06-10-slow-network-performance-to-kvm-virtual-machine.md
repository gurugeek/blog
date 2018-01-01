slug: "slow-network-performance-to-kvm-virtual-machine"
title: Slow network performance to KVM virtual machine.
published_date: "2013-06-10 10:10:53 +0000"
layout: default.liquid
data:
  author: admin
  wordpress_id: 554
  layout: post
  comments: true
---
I'm currently using a KVM virtual machine as my primary file/media server. Since
I have been using a virtual machine as my file server I have witnessed strange
stalls in media playback when accessing media files via a NFS share. Media files
would take a couple of seconds to load and occasionally playback would stall for
anything from a second to 20 seconds. In addition scp transfers would also
occasionally stall. With not much to go on I turned to Google which did turn up
an old Redhat bug report from 2009[^0]. One of the solutions was to disable TCP
offload in the KVM guest via:

    mfs@wvm1 # ethtool -K eth0 tx off

Even though this was an old bug report, once I did this the stalls went away.
Media playback is perfect. I haven't tried any scp transfers though I am
confident they will now complete without stalling as well. For the time being I
will be turning off TCP offload on all of my KVM guests.

[^0]: [https://bugzilla.redhat.com/show_bug.cgi?id=490266](https://bugzilla.redhat.com/show_bug.cgi?id=490266)
