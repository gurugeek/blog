title: Planck Build
published_date: "2017-08-01 20:50:00 +1000"
layout: default.liquid
data:
  layout: post
---
I have recently become interested in compact keyboards. In particular the
[Planck][0]. The Planck is unusual in many ways compared to standard 104 key
keyboard. In summary:

 * It is a 40% keyboard, much smaller than typical keyboards. With only 47/48
   keys, many keys are accessed via layer switching. The two keys next to the
   space bar activate alternate layers.
 * It is an ortholinear keyboard, the keys are arranged in a grid, not staggered
   rows as a normal keyboard.
 * It runs open source firmware called [Quantum][1] which is fully customizable.
   It supports Qwerty, Colemak, Dvorak and a
   [Plover](http://www.openstenoproject.org) layout out of the box.
 * It is built from either a kit or from scratch.

## Parts

There are multiple ways to build a Planck. Some users hand wire the key switches
though an easy way is to buy a pre-made PCB. I went the pre-made PCB route. I
sourced the PCB, top plate and case from [olkb.com](https://olkb.com). I sourced
the key switches and key caps from [matias](https://matias.store).

## Construction

As mentioned above, a Planck has to be constructed. This involves inserting the
key switches into the top plate and then into the PCB. I chose to insert all the
switches in one go ensuring the switches were all flush on the top plate and all
switch pins were inserted into the PCB. Before inserting the switches I tested
each switch with a multimeter. With 47 switches there are 94 connections to
solder. This does take a while though the pads are large and laid out in a grid
which makes it natural to run through them one row at a time. If you have
soldered anything before, a Planck PCB is pretty easy and probably wouldn't be a
bad first soldering project.

Below are some photos of the process.

<a href="/images/planck_build/00.jpg">
<img class="thumb25" src="/images/planck_build/00.jpg"
    alt="Parts" title="Parts">
</a>
<a href="/images/planck_build/01.jpg">
<img class="thumb25" src="/images/planck_build/01.jpg"
    alt="Switch Testing" title="Switch Testing">
</a>
<a href="/images/planck_build/02.jpg">
<img class="thumb25" src="/images/planck_build/02.jpg"
    alt="Placing the first switches" title="Placing the first switches">
</a>
<a href="/images/planck_build/03.jpg">
<img class="thumb25" src="/images/planck_build/03.jpg"
    alt="Test fitting case before soldering" title="Test fitting case before soldering">
</a>
<a href="/images/planck_build/04.jpg">
<img class="thumb25" src="/images/planck_build/04.jpg"
    alt="Switches soldered" title="Switches soldered">
</a>
<a href="/images/planck_build/05.jpg">
<img class="thumb25" src="/images/planck_build/05.jpg"
    alt="Case assembled" title="Case assembled">
</a>
<a href="/images/planck_build/06.jpg">
<img class="thumb25" src="/images/planck_build/06.jpg"
    alt="Switch caps laid out" title="Switch caps laid out">
</a>
<a href="/images/planck_build/07.jpg">
<img class="thumb25" src="/images/planck_build/07.jpg"
    alt="Half way" title="Half way">
</a>
<a href="/images/planck_build/08.jpg">
<img class="thumb25" src="/images/planck_build/08.jpg"
    alt="Half way" title="Complete">
</a>

## Use

Using a Planck keyboard is an interesting experience. I have only been using it
three or four days and have already noticed a couple of things.

Your hands don't really move. Your palms tend to just rest in the one place.
I quite like this.

Your thumbs are quite busy. Between the space bar and the layer switch keys they
get a workout.

Learning the layout isn't that difficult. In addition, some keys end up in very
convenient locations by default. E.g. both `-` and `_` end up on the J
key. As a Linux Sys Admin these keys get plenty of use and having them on the
home row works well. Function keys are also easy to access on or below the left
home row. Having easy access to F5 makes refreshing webpages much easier. When I
look at a standard keyboard now my eyes are instantly drawn to the distance
between the home row and the F5 key.

One major pain point is password entry. For me password entry relies on muscle
memory. While I know the characters in my passwords, at some point I stop using
the actual characters and instead recall the key positions instead. Changing the
entire keyboard layout plays havoc with this system. Entering passwords is quite
slow at the moment though I am improving.

## Next Steps

Once I'm comfortable with the keyboard I'm going to look at making some tweaks
to the firmware. My keyboard has an older firmware loaded and is also missing a
couple of keys included in the most recent firmware. Notably the Home, End, Pg
Up and Pg Down keys. I haven't really missed them to be honest. I'll blog about
that when I get around to it.

[0]: https://olkb.com/planck
[1]: https://qmk.fm
