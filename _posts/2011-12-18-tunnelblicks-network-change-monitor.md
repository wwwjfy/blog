---
comments: true
date: 2011-12-18 10:13:32+00:00
layout: post
slug: tunnelblicks-network-change-monitor
title: Tunnelblick's network change monitor
categories:
- Misc
tags:
- mac
- tunnelblick
---

[Tunnelblick](http://code.google.com/p/tunnelblick) is the OpenVPN client under Mac OS X.

It's easy to use, but I've got some special requirement for that: I want to have my own DNS settings and need Tunnelblick's network settings change monitor. However, currently, it only works with default name server setting.

So I read part of the source code to find the possibility to do some hack.

All the setting interfaces are related to SettingsSheetWindowController.m, which interacts with the UI, and saves the values in global gTbDefaults. VPNConnection.m is the one who parses the configurations and passes them to openvpnstart. Almost all configurations can be found in method argumentsForOpenvpnstartForNow: in VPNConnection.m, surely including the one I'm tracking: "doNotRestoreOnDnsReset".

openvpnstart is another executable to actually run openvpn, also up and down scripts. The up script will start leasewatch if necessary, i.e. "Monitor network settings" is checked in Tunnelblick settings. Leasewatch is the daemon monitoring network changes, and it's run by launchd.

The plist used by launchd includes a key called WatchPaths. Leasewatch uses that to get notified to do any changes, and the old values are saved in system configurations accessed by scutil.

Fine, I think it works, but too complicated, though I now cannot get a smarter way yet. And I still need read more to find a way to have customized DNS settings monitored for changes.
