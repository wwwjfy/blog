---
comments: false
date: "2010-09-18"
layout: post
slug: dual-monitor-notification-position-fix-for-notify-osd-in-ubuntu
title: dual monitor notification position fix for notify-osd in ubuntu
categories: ['Works']
tags: ['notify-osd', 'ubuntu']
---

It's good to have two monitors when developing, but there is a problem in ubuntu that the notify-osd notification shows in the top right corner of the desktop, but not active monitor, which makes me missed some notifications when paying attention to the left or the low monitor.

The basic flow that notify-osd gets the position:

	get panel window;
	if (panel window)
		monitor = get panel monitor;

	if (follow focus) { //follow focus is set in gconf
		monitor = get monitor where mouse cursor in;
		get active window;

		if (active window)
			monitor = active window;

		if (panel window && panel_monitor == monitor)
			add panel height;
		
		else if (! panel_window) // which needs one more condition: && ! follow focus
			use the top right corner of the whole desktop;

The patch works for me, I've sent the patch to the authors.
