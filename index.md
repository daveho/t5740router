---
layout: default
title: "HP t5740 Home Router"
---

<div style="text-align: center; float: right; margin-left: 40px; margin-bottom: 20px;">
<a href="img/t5740.jpg"><img style="width: 120px;" src="img/t5740.jpg"></a> <br>
My HP t5740
</div>

TL;DR I am working on setting up an HP t5740 thin client to serve as a home router, using Debian 8 (Jessie).  This is very much a work in progress!  I will be updating this website as the project progresses.

## News

* **April 17th, 2016** &mdash; [Shorewall](shorewall.html) is working in a test environment.  Next step is setting up dnsmasq.
* **April 15th, 2016** &mdash; Hard drive is installed, and the system is running Debian.  Next step is setting up [Shorewall](shorewall.html).

## Background

For a long time I have been using an [ASUS RT-N16](https://www.asus.com/us/Networking/RTN16/) router running [Tomato USB](http://tomatousb.org/) for my home network.  In addition to serving as a router, it also serves as a light-duty NAS using a USB hard drive.  (I know that in principle the router and NAS should be separate devices, but whatever, it works for us.)  In general, this setup has worked extremely well.

*However*...there is one thing that I find problematic about home routers, which is that you are dependent on either the device vendor or the maintainers of the OSS firmware to provide timely updates.  Neither vendors nor firmware maintainers have a great track record of providing updates.  Tomato USB, for example, isn't even maintained at this point.

  I will speculate that part of the reason for this is the limited hardware (RAM, CPU, and flash storage) in home routers, which makes wholesale replacement of the entire firmware image the only practical update mechanism, impeding the flow of updates.   Contrast this with a full OS such as [Debian](https://www.debian.org/), where incremental updates are provided for all software on a regular basis.  I want to be able to do

    apt-get update && apt-get upgrade

on my router!

Another issue is that for open source firmware, the firmware image must be customized for each supported hardware model.  Again, this complicates the task of distributing timely updates.  Contrast this with a mainstream PC OS, where a single generic OS image can run on any PC.

This website describes my project to set up an HP t5740 thin client as a home router.

## Content

* [Thin clients](thinclients.html) &mdash; Why a thin client makes a good home router
* [Hardware used](hardware.html) &mdash; This is the hardware I used
* [Hard drive mod](hdmod.html) &mdash; Describes how I installed a hard drive in the t5740
* [Configuring Debian](debian.html) &mdash; Installing and configuring Debian Linux
* [Configuring Shorewall](shorewall.html) &mdash; Configuring the Shorewall firewall scripts
* [Configuring dnsmasq](dnsmasq.html) &mdash; Configuring dnsmasq to provide DNS and DHCP
* Configuring samba &mdash; Coming soon!

<!-- vim:set wrap: ­-->
<!-- vim:set linebreak: -->
<!-- vim:set nolist: -->
