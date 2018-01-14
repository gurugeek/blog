title: "BeagleBone - Black and White"
published_date: "2014-10-12 19:13:33 +1000"
layout: default.liquid
data:
  comments: true
  layout: post
---
While the Raspberry Pi is the most well known of the ARM based single board
computers there are other alternatives available. One such alternative is the
BeagleBone. The BeagleBone comes in two varieties which differ in a couple
of key areas. See the below feature matrix:

<table>
    <tr>
        <th></th>
        <th>White</th>
        <th>Black</th>
    </tr>
    <tr>
        <th>Video</th>
        <td>N/A</td>
        <td>Micro HDMI</td>
    </tr>
    <tr>
        <th>Serial Console</th>
        <td>Yes</td>
        <td>No</td>
    </tr>
    <tr>
        <th>Boot</th>
        <td>Micro SD</td>
        <td>Micro SD or 2GB eMMC</td>
    </tr>
</table>

The lack of built in serial console on the BeagleBone Black caught me by
surprise in particular. The BeagleBone White's ability to use a single USB cable
for power and a serial console is very nice. You can use a serial terminal or
`screen` to connect to it:

    # screen /dev/ttyUSB0 115200
    Arch Linux 3.17.0-1-ARCH (ttyO0)

    alarm login:

If you require a serial console on the BeagleBone Black you will need to
purchase a [USB to 3.3V TTL cable][0].

While there are several Linux distributions available for the BeagleBone, Arch
Linux is one of the best due to the non-bloated, modern (systemd based) install:

    [root@alarm ~]# ps --ppid 2 -p 2 --deselect f
      PID TTY      STAT   TIME COMMAND
        1 ?        Ss     0:12 /sbin/init fixrtc
      105 ?        Ss     0:01 /usr/lib/systemd/systemd-journald
      116 ?        Ss     0:00 /usr/lib/systemd/systemd-udevd
      131 ?        Ssl    0:06 /usr/lib/systemd/systemd-timesyncd
      136 ?        Ss     0:00 /usr/lib/systemd/systemd-logind
      137 ?        Ss     0:00 /usr/bin/dbus-daemon --system --address=systemd: --nofork --nopidfile --systemd-activation
      138 ?        Ss     0:14 /usr/bin/haveged -w 1024 -v 1
      144 ?        Ss     0:21 /usr/lib/systemd/systemd-networkd
      155 ?        Ss     0:40 /usr/lib/systemd/systemd-resolved
      156 ?        Ss     0:00 /usr/bin/sshd -D
      162 ?        Ss     1:16  \_ sshd: root@pts/0
      167 pts/0    Ss     0:00      \_ -bash
    15391 pts/0    R+     0:00          \_ ps --ppid 2 -p 2 --deselect f
      157 tty1     Ss+    0:00 /sbin/agetty --noclear tty1 linux
      158 ttyO0    Ss+    0:00 /sbin/agetty --keep-baud 115200 38400 9600 ttyO0 vt102
      164 ?        Ss     0:00 /usr/lib/systemd/systemd --user
      165 ?        S      0:00  \_ (sd-pam)

I'm currently setting up my BeagleBone White as a wireless network monitor.

 * BeagleBone White
 * Arch Linux
 * Airodump-ng
 * D-Link DWA-131

![BeagleBone Wireless Monitor](/images/bbw_wlan.jpg)

[0]: http://elinux.org/Beagleboard:BeagleBone_Black_Accessories#Serial_Debug_Cables
