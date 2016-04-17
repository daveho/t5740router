---
layout: default
title: "Configuring Shorewall"
---

<div class="callout">
<strong>Caution</strong>: This content is somewhat preliminary.  I have Shorewall working in a test environment, but I have not put it into full production yet.
</div>

[Shorewall](http://shorewall.net/) is a set of scripts that configures [netfilter](http://www.netfilter.org/) and other Linux networking components to act as a firewall/router.  The idea is that you edit a few high-level configuration files, and Shorewall orchestrates the configuration of the low-level networking components to put your configuration into effect.

## Why Shorewall?

There are lots of options for setting up a PC as a firewall/router.

One possibililty is to use a [router distribution](https://en.wikipedia.org/wiki/List_of_router_and_firewall_distributions).  These are mostly customized versions of Linux or BSD with some additional software to implement firewall/router functionality.  The main reason I decided against using any of these is that none of them impressed me as being sufficiently like a "real" Linux or BSD distribution.  In general, I find that excessive customization of Linux (or BSD) goes against the Unix philosophy, which is to provide a set of excellent general-purpose tools that can solve problems in a wide variety of environments.  Another concern that I had with the router distributions is that many are relatively small projects, raising the possibility of abandonment and lack of long-term support.  Finally, some of the router distributions are controlled by commerical entities whose interests don't necessarily align completely with the interests of the users of the software.

Shorewall appealed to me because it's simply a set of scripts that can run on pretty much any Linux distribution.  I'm a big fan of Debian, and there is a Debian package for Shorewall, so it seemed like a sensible choice.

## Configuring Shorewall

To configure Shorewall, you start with a set of example configuration files, and then edit based on details of your home network.  I started from the [basic two interface firewall](http://shorewall.net/two-interface.htm) configuration, which is the one that is appropriate for a basic home network.

The official Shorewall documentation is pretty good as a reference, but not particularly helpful as a tutorial.  Shorewall is very configurable, and the documentation exhaustively documents every possible option, but that's not all that helpful if you're just trying to get *something* working.  What I really wanted was a concise summary of what configuration details I would need to change.

Fortunately, the [Arch Linux wiki article on Shorewall](https://wiki.archlinux.org/index.php/Shorewall) provides a concise summary of how to set up a two-interface configuration.  Even though I am using Debian, the information in the article still applies.

## Why doesn't anyone use the `params` file?

A Shorewall configuration has a file called `params` in which you can define variables which may be referenced in the other configuration files.  This is a great idea!  For example, if the ethernet port connected to the external network is `eth2`, you can set a variable in params like so:

    NET_IF=eth2

and then refer to `$NET_IF` in the other configuration files which need to know how to reach the external network.  In practice, when you are dealing with a common situation like a home network, *most people's configurations are going to be very similar*, so it should be possible to set things up by *only changing the details that are actually likely to be different*.  In fact, I would argue that there are only three pieces of information that should be necessary to provide to a home router, at least in the most common case (where the router will receive its external IP address by DHCP):

* Which ethernet port connects to the external network
* Which ethernet port connects to the local network
* What address block should be allocated for hosts on the internal network

Here's the thing, though: *no one seems to use the params file*.  The Shorewall example configurations don't use it.  The Arch Linux wiki example configuration doesn't use it.  Everyone seems to be happy modifying details in each individual configuration file in a way that is both redundant and error-prone.

In my configuration, I have set things up so that `params` is the only file that really needs to be modified to have a working configuration.  See *My configuration* below for details.

## My configuration

My configuration is the "two interface" configuration from `/usr/share/doc/shorewall/examples/two-interface`, with some minor modifications.  Specifically:

* I allow programs running on the firewall to connect to the external internet.
* I use the `params` file to define details that are expected to be specific to my installation.  So, if you want to use my configuration, this is quite possibly the only file you would need to change.

Here is my preliminary shorewall configuration (these files should go in `/etc/shorewall`):

> [shorewall-etc-v01.zip](config/shorewall-etc-v01.zip)

Here is my `params` file (minus some boilerplate comments).  As mentioned above, this is the only file you should need to change if you use my configuration:

    # On my HP t5740, I have an HP NC360T PCIe dual gigabit ethernet card,
    # which appears under debian as eth1 and eth2.  eth2 is the connection
    # to the net (cable modem), and eth1 is connected to the local network.
    # (eth0 is the integrated Broadcom ethernet, which I'm not using
    # for anything.)
    
    # Interface to internet
    NET_IF=eth2
    
    # Interface to local network
    LOC_IF=eth1
    
    # This is the IP range used on the local network.
    LOC_NET_CIDR=192.168.6.0/24

Once you have copied the Shorewall configuration files into `/etc/shorewall` and edited the `params` file as appropriate, you will then need to edit `/etc/default/shorewall` to change the value of the `startup` variable from `0` to `1`.  Once you've done this, reboot, and you should be ready to route packets!

<!-- vim:set wrap: ­-->
<!-- vim:set linebreak: -->
<!-- vim:set nolist: -->
