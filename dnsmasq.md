---
layout: default
title: "Configuing dnsmasq"
---

<div class="callout">
<b>Warning</b>: This section is somewhat preliminary.
</div>

[Dnsmasq](http://www.thekelleys.org.uk/dnsmasq/doc.html) is a nifty piece of software that provides DNS (name resolution) and DHCP (automatic assignment of network addresses) to clients on a local network.

Installation is easy: just do

    sudo apt-get install dnsmasq

## Basic configuration

There is nothing special you need to do to configure DNS.  Dnsmasq will automatically use the system DNS resolution mechanism to do DNS lookups on behalf of clients on the local network.  Usually, this will be via whatever nameserver your cable modem or DSL modem specifies.

Following the recommendation of the [Debian dnsmasq howto](https://wiki.debian.org/HowTo/dnsmasq), I configured dnsmasq to listen for DNS (and DHCP) requests from the local network only, not the external network.  As I mentioned in the [shorewall](shorewall.html) section, the router is connected to the local interface using `eth1`.  So I changed the line in `/etc/dnsmasq.conf` reading

    #interface=

to

    interface=eth1

## Configuring DHCP

To configure DHCP, I edited the line reading

    #dhcp-range=192.168.0.50,192.168.0.150,12h

to read

    dhcp-range=192.168.6.2,192.168.6.199,12h

This tells dnsmasq to hand out IP addresses in the range `192.168.6.2` to `192.168.6.199`.

I have one host on my local network (our laser printer) that I want to have a static IP address.  I added the following `dhcp-host` option in `/etc/dnsmasq.conf`:

    dhcp-host=30:05:5C:20:A9:32,192.168.6.121

This causes dnsmasq to always assign the IP address `192.168.6.121` to the printer (based on the printer's ethernet MAC address.)

I also added the following entry in `/etc/hosts`:

    192.168.6.121   printserver

This will cause all lookups of the hostname `printserver` on the local network to resolve to the printer's static IP address.  (Any hostname/address pairs you define in `/etc/hosts` will automatically be used by dnsmasq in answering DNS lookup requests from the local network.)
