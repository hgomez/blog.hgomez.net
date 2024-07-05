+++
title = 'VirtualBox 4.3.22 and network drives'
date = 2015-02-23T13:20:23+02:00
draft = false
tags = [ 'VirtualBox' ]
categories = [ 'Virtualization' ]
image = 'virtualbox.png'
+++

If you upgraded from VirtualBox 4.3.20 to 4.3.22 and use Vbox network shares, you may have noticed shares are reported as unmounted, even if they are available.

This is a well known issue :

[https://forums.virtualbox.org/viewtopic.php?f=2&t=66081](https://forums.virtualbox.org/viewtopic.php?f=2&t=66081) [https://forums.virtualbox.org/viewtopic.php?f=1&t=66067#p312765](https://forums.virtualbox.org/viewtopic.php?f=1&t=66067#p312765)

Solution for now is to revert to 4.3.20 (VB and extension) or wait for upcoming VirtualBox release