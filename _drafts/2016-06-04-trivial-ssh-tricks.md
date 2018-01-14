title: Trivial SSH tricks
categories:
  - Linux
  - SSH
published_date: "2016-06-04 20:49:44 +1000"
layout: default.liquid
data:
  comments: true
  layout: post
---
As a System Administrator I use SSH all the time. These are a couple of
tricks/techniques I use which I wasn't always aware of. SSH can do many things,
the man page is worth a read.

## Ciphers for CPU Limited Systems.

SSH can be used to transfer files via `scp` and `sftp`. SSH is also used as a
transport medium via other utilities such as `rsync` and backup utilities. When
transferring files over a local network it's not difficult to become CPU
limited when transferring to/from low performance CPU machines. For example
micro servers and embedded systems. By default SSH uses AES for encryption
though there are alternatives. Previously arcfour was useful for increasing
transfer speed when CPU limited though some distrobutions disable it by default.
An alternative that is almost as fast as arcfour is
chacha20-poly1305@openssh.com. This is a modern cipher and hash designed by DJB.
To use use the `-c` option.

```
$ ssh -c chacha20-poly1305@openssh.com ...
```

## Socks Proxy

SSH can do TCP port forwarding which comes in handy for accessing local services
such as database servers. It can also act as a local SOCKS proxy. By connecting
to a remote host via SSH and telling your browser to use local host as a SOCKS
proxy you can tunnel your web traffic via the remote system. This is often
useful for testing, particularly GeoIP. Use the -D argument and pass in a port.

```
$ ssh -D 5000 server.example.com
```





## SSH Escape Character


