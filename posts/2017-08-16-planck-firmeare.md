title: Planck Firmware
published_date: "2017-08-16 20:50:00 +1000"
layout: default.liquid
data:
  layout: post
---
As mentioned in my previous [post](/posts/2017-08-01-planck-build.html) I have
recently started using a [Planck][0] keyboard. The Planck uses Open Source
[firmware][1] and customizing the firmware is a key part of making a keyboard
that suits the user.

## Firmware Building

Compiling QMK on Linux requires the installation of a couple of packages. Below
is a screen cast of this process. This was done on an AWS Lightsail instance
running Debian Linux. The default Lightsail Debian image has the
`build-essential` package pre-installed. Add this to the `apt-get install ...`
line if required. The screen cast shows:

 - Installing required packages
 - Cloning the QMK repo
 - Building the default rev4 planck firmware

## Screencast

<script type="text/javascript" src="https://asciinema.org/a/133365.js"
id="asciicast-133365" async></script>

[0]: https://olkb.com/planck
[1]: http://qmk.fm
