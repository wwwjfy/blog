---
comments: true
date: "2014-06-03 16:15:11 +0800"
layout: post
slug: "some-solutions-to-some-problems-in-yosemite-beta"
title: "Some solutions to some problems in Yosemite beta"
categories:
- Tips
tags:
- Yosemite
- PCKeyboardHack
- dnsmasq
---

This morning, Apple released new OS X, Yosemite, to developers. I'm so excited
that I'm now using it. :D

I've met some problems after the upgrade.

### _Stuck_ in the installation when 4 mins left ###

For me, it lasted around half an hour for the last "4 mins". Checking the logs
by ⌘-L showed it actually was doing something, which comforted me.

### PCKeyboardHack ###

It asked you to reboot the computer to make it work, which couldn't. According
to [PCKeyboardHack issues 31](https://github.com/tekezo/PCKeyboardHack/issues/31)
and I've tried, this command actually works.

```
sudo kextload /Applications/PCKeyboardHack.app/Contents/Library/PCKeyboardHack.10.9.signed.kext/
```

Before official support released, I'll live with this trick.

### dnsmasq ###

I installed `dnsmasq` by [homebrew](http://brew.sh) and it was daemonized by
launchd. In Yosemite, launched was updated, and a new concept added (if not
added before), which is domain. Also, new subcommands are replacing the old
ones.

Whatsoever, somehow, dnsmasq didn't get started at load, and the usual command
to start it doesn't work: `launchctl load /path/to/homebrew.mxcl.dnsmasq.plist`.
The workround is to use subcommand `kickstart`. To reload,

```
sudo launchctl kickstart system/homebrew.mxcl.dnsmasq
```

`system` is the domain, which is needed for processes to be run as root.

### Homebrew ###

Some weird issue after fixing the ruby version and installing command line tool,
saying things are missing.

Actually the path is not complete, without pathes with brew. I don't know how it
works before, but adding argument `--env=std` fixed the problem.

### DNS cache ###

Previously, I used `sudo killall -HUP mDNSResponder` to flush dns caches, but
somehow mDNSResponder doesn't get started, so I found the new tool
`discoveryutil`, and to flush dns caches, use
`sudo discoveryutil mdnsflushcache`.
