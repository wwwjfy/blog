---
comments: true
date: "2009-04-02"
layout: post
slug: make-usb-available-for-virtualbox-on-ubuntu
title: Make usb available for VirtualBox on Ubuntu
categories: ['Linux']
tags: ['ubuntu', 'virtualbox']
---

I chose VirtualBox as VM, for it's much smaller than VMware, and works fine.

Now I need to use U盾 of ICBC in Windows in VM. (What a pity there is not a driver for Linux to use ActiveX and U盾.)

I search the solution, and found this: [http://forum.ubuntu.org.cn/viewtopic.php?t=89142](http://forum.ubuntu.org.cn/viewtopic.php?t=89142).
It seems we are in the same situation. So I did everything as the post said. But it didn't work.
Though the U盾 had appeared in the list, it was grey, could not be chosen.

Another solution is on the official wiki page:

[http://www.virtualbox.org/wiki/USB_on_Ubuntu_7.04](http://www.virtualbox.org/wiki/USB_on_Ubuntu_7.04).

It's simpler. I just followed it, and it worked.

May be I should search everything on the official page first instead of Google from now on. :)
