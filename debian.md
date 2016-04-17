---
layout: default
title: "Configuring Debian"
---

I used the [netinst with firmware ISO](http://cdimage.debian.org/cdimage/unofficial/non-free/cd-including-firmware/) image.  The Atom N280 CPU in the t5740 is a 32 bit CPU, so I used the i386 version.  I used `dd` to copy the image onto a USB flash drive: the t5740 booted right into the Debian installer with no issues.

A few notes:

* The [SATA hard drive](hdmod.html) appeared as `/dev/sdb`.  I used all but 8 GB for a single root partition, and used the remaining 8 GB for swap space.  (I seriously doubt the swap space will ever be used, though.)  I did *not* use the DOM (`/dev/sda`) in any way other than installing GRUB on the master boot record.
* I used `eth2` as the interface to the external network.  This just happened to be the port that was connected to my home network.  Eventually, this will be the port connecting to the external network, which will be significant when configuring [shorewall](shorewall.html).
* I installed the LXDE desktop environment.  I have 2 GB of RAM, soon to be upgraded to 4GB, so I figured this would not tax the system resources unreasonably, and being able to run a web browser while configuring things is helpful.
* I also configured the ssh server.  I don't plan to allow ssh access to the router from outside the home network, but I do plan to do administration via ssh from wthin the home network.

## Post-install configuration

The only configuration task required after installing Debian is configuring the ethernet interface the router will use to connect to the local network.  In my case, this is `eth1`.  Here is the entry I added to `/etc/network/interfaces`:

    # DHH: this is the router's local network interface.
    # Make sure this stays up to date with the LOC_IF
    # and LOC_NET_CIDR variables in the shorewall params
    # file.
    auto eth1
    iface eth1 inet static
      address 192.168.6.1
      netmask 255.255.255.0

So, to all of the computers on the local network, the router will appear as `192.168.6.1`.

**Important**: Make sure that the address and netmask you use are consistent with the address range you will define for the local network in your Shorewall configuration.

<!-- vim:set wrap: ­-->
<!-- vim:set linebreak: -->
<!-- vim:set nolist: -->
