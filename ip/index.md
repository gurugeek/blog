layout: default.liquid
---
## iproute2 cheat sheet


#### Interfaces

Set interface up:

	ip link set em1 up

Set interface down:

	ip link set em1 down

Set interface mtu:

	ip link set em1 mtu 9000

#### Addresses

Add an address to an interface:

	ip addr add 10.0.0.1/24 dev em1

Delete an address from an interface:

	ip addr del 10.0.0.1/24 dev em1

#### Routes

Add default route:

	ip route add default via 10.0.0.10

Add route:

	ip route add 10.1.0.0/24 via 10.0.0.10

Delete route:

	ip route del 10.1.0.0/24 via 10.0.0.10

#### ARP

View ARP table:

	ip neigh

View ARP cache for interface:

	ip neigh show dev em1

Add entry to ARP table:

	ip neigh add 10.0.0.15 lladdr 00:11:22:33:44:55 dev em1

Delete entry in ARP table:

	ip neigh del 10.0.0.15 dev em1

A much more detailed iproute2 cheatsheet can be found
[here](http://baturin.org/docs/iproute2/).


