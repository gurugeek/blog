title: Enabling SPDY in nginx on Debian Wheezy
published_date: "2014-04-26 13:35:45 +1000"
layout: default.liquid
data:
  comments: true
  layout: post
---
The default nginx packages on Debian Wheezy are 1.2.1 which don't include
[SPDY][5] support. There are a couple of options here:

## Options

### Install nginx from source

This is an option though less than ideal. I prefer to have all software
installed and managed using apt. If you want to go this way there are several
howtos available as well as nginx's own [documentation][0]. One advantage to
installing from source is that you can tailor the build to include only the
features you require.

### Use the wheezy-backports repo

Debian backports are a solid option. This is an official repo and will currently
give you access to [nginx 1.6.2][2]. If you simply want a version of nginx that
includes SPDY support this is probably the best option.

__Update__: at the time of writing wheezy-backports contained 1.4.6. The above
paragraph has been updated. In addition ensure you install nginx-full or
nginx-extras. nginx-light does not have SPDY support included.

### Use the dotdeb repo

[Dotdeb][3] are a third party repo that provide updated packages common in LAMP
setups. They currently have nginx 1.4.7 available.

### Use the nginx repo

Nginx also provides [deb packages][4] for the stable and mainline nginx builds. If
you don't just want SPDY support and either require or prefer a later version
this will give you the cutting edge. Currently nginx 1.6 and 1.7 are available
direct from nginx.

## Decisions, Decisions

To add SPDY support to this site I used the nginx repos. This is a non
production site and I'm willing to use cutting edge software. One thing to watch
when using the nginx repos is that the included `/etc/nginx/nginx.conf` does not
include the line `include /etc/nginx/sites-enabled/*;`. If you are using the
`sites-{available,enabled}` directories, backup your `/etc/nginx/nginx.conf` file
or add this line in manually. If you choose to backup your existing config note
that it may require tweaking to get it working in the new version of nginx.

Once you have a SPDY capable version of nginx installed, enabling SPDY is a
matter of modifying your listen directives:

    server {
        listen 443 ssl spdy;
        ...

To check that SPDY is working use [spdycheck.org][6].


[0]: http://nginx.org/en/docs/configure.html
[1]: https://wiki.debian.org/Backports
[2]: https://packages.debian.org/wheezy-backports/httpd/nginx-full
[3]: http://www.dotdeb.org/category/nginx/
[4]: http://nginx.org/en/linux_packages.html
[5]: http://en.wikipedia.org/wiki/SPDY
[6]: http://spdycheck.org/#blog.sambodata.com
