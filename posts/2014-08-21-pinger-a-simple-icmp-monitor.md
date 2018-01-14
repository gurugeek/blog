title: "Pinger - A simple ICMP monitor."
published_date: "2014-08-21 21:17:23 +1000"
layout: default.liquid
data:
  comments: true
  layout: post
---
When it comes to monitoring hosts, most people go with [Nagios][0] or similar.
Nagios is great, can monitor pretty much most standard services out of the box
and is easily extendible. For my home network I really just wanted a simple ICMP
check. For this, Nagios was overkill. This is what I came up with.

## Introduction

[Pinger][1] is a simple Python based ICMP monitor with [Pushover][2]
integration. What is Pushover? From their website:

> In short, Pushover is a service to receive instant push notifications on your
> phone or tablet from a variety of sources.

One of these sources is a simple REST API. There are no monthly fees and the app
costs $5.00 after a 5 day free trial.

Pinger is fast. All hosts are pinged asynchronously in parallel. Pinging ten
hosts takes the same amount of time as pinging one host. Each host is pinged
five times with a one second interval. As long as at least one packet is
returned the host is considered up.

As mentioned above. I use pinger to monitor my home network including my
router. To ensure I receive notification of issues that may effect network
traffic I run pinger from one of my VPS instances hosted off site. This machine
connects to my home network via an OpenVPN link.

## Installation

A `requirements.txt` file is included to make installation easy using a virtual
environment. Execute the following commands:

    git clone https://github.com/mfs/pinger.git
    cd pinger
    virtualenv pinger_env
    . pinger_env/bin/activate
    pip install -r requirements.txt

You should then be able to execute pinger:

    $ ./pinger -h
    usage: pinger [-h] config

    positional arguments:
      config      config file

    optional arguments:
      -h, --help  show this help message and exit

Setup a cronjob that calls pinger every 5 minutes or so. Don't forget to
activate the virtual environment. A simple wrapper script may be handy:

    #!/bin/bash
    cd /root/pinger
    . pinger_env/bin/activate
    ./pinger config.yml


## Configuration

Configuration is simple and in the form of a yaml file:

    pushover:
            user: <PUSHOVER_USER_KEY>
            token: <PUSHOVER_API_KEY>

    nodes:
            - name: host1
              ip: 10.0.0.1

            - name: host2
              ip: 10.0.0.2

            - name: host3
              ip: 10.0.0.3

Add your pushover user key, application token and define some hosts. You are
then good to go.


[0]: http://www.nagios.org
[1]: https://github.com/mfs/pinger
[2]: https://pushover.net
