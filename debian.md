---
layout: default
title: "Installing Debian"
---

# Installing Debian

I used the [netinst with firmware ISO](http://cdimage.debian.org/cdimage/unofficial/non-free/cd-including-firmware/) image.  The Atom N280 CPU in the t5740 is a 32 bit CPU, so I used the i386 version.  I used `dd` to copy the image onto a USB flash drive: the t5740 booted right into the Debian installer with no issues.

A few notes:

* The [SATA hard drive](hdmod.html) appeared as `/dev/sdb`.  I used all but 8 GB for a single root partition, and used the remaining 8 GB for swap space.  (I seriously doubt the swap space will ever be used, though.)  I did *not* use the DOM (`/dev/sda`) in any way other than installing GRUB on the master boot record.
* I installed the LXDE desktop environment.  I have 2 GB of RAM, soon to be upgraded to 4GB, so I figured this would not tax the system resources unreasonably, and being able to run a web browser while configuring things is helpful.
* I also configured the ssh server.  I don't plan to allow ssh access to the router from outside the home network, but I do plan to do administration via ssh from wthin the home network.
