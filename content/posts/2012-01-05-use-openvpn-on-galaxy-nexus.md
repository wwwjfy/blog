---
comments: true
date: "2012-01-05"
layout: post
slug: use-openvpn-on-galaxy-nexus
title: Use OpenVPN on Galaxy Nexus
categories: ['Tips']
tags: ['android', 'galaxy nexus', 'openvpn']
---

I recently got a Galaxy Nexus to play with. What I hate is Android doesn't provide native proxy support, or OpenVPN support, because I'm in China.

## First step: Root the device

I followed the instruction on [GalaxyNexusRoot.com](http://galaxynexusroot.com/galaxy-nexus-root/how-to-root-galaxy-nexus/).

## Next: Install openvpn

- Install OpenVPN Installer on [Android Market](https://market.android.com/details?id=de.schaeuffelhut.android.openvpn.installer&hl=en). Run the application and install openvpn and ifconfig and route to /system/xbin.

- This step is important: making ifconfig and route in /system/xbin/bb, because openvpn will execute /system/xbin/bb/ifconfig to create tun0 interface, otherwise, the device will connect to the server without an interface to send packets.

	- Install SSHDroid on [Android Market](https://market.android.com/details?id=berserker.android.apps.sshdroid&hl=en). SSH to the device and run
    
		    mkdir /system/xbin/bb
		    ln -s /system/xbin/ifconfig /system/xbin/bb/
		    ln -s /system/xbin/route /system/xbin/bb/

    - If it says "Read-only file system". Run the following command first to make it read/write, and then run the above commands.
    
			mount -o remount,rw -t ext4 /dev/block/platform/omap/omap_hsmmc.0/by-name/system /system
			
		**Update**: a simplified version

			mount -o remount,rw /system

	- Later, you can choose to make to readonly again by

			mount -o remount,ro -t ext4 /dev/block/platform/omap/omap_hsmmc.0/by-name/system /system

		**Update:** A simplified version:

			mount -o remount,ro /system

## Run it

Now it'll be fine to run OpenVPN by [OpenVPN Settings](https://market.android.com/details?id=de.schaeuffelhut.android.openvpn&hl=en). Just copy OpenVPN configuration files into /mnt/sdcard/openvpn and run it in [OpenVPN Settings](https://market.android.com/details?id=de.schaeuffelhut.android.openvpn&hl=en).

## The problem I met

**Update:** [OpenVPN Installer](https://market.android.com/details?id=de.schaeuffelhut.android.openvpn.installer&hl=en) 0.2.4 fixed the problem by providing the option with openvpn using /system/xbin/ifconfig and /system/xbin/route

At the beginning, I tried to connect using [OpenVPN Settings](https://market.android.com/details?id=de.schaeuffelhut.android.openvpn&hl=en) but failed, but from logs by adb mixed with system logs, I couldn't find useful information. I didn't find the tun.ko on the device, and I searched and viewed a ton of pages and found nothing.

Though from [TUN.ko Installer](https://market.android.com/details?id=com.aed.tun.installer) I saw _Tun module is loaded_, and _Path to tun.ko is not found!_, I thought there was some problem with the software that showing inconsistent information. I gave a last try to [DroidVPN](https://market.android.com/details?id=com.aed.droidvpn), and I succeeded to connect!

Before that, I even downloaded the source and was ready to compile tun.ko myself. The lesson is to try every possible way, a problem can be caused by any possible reason.