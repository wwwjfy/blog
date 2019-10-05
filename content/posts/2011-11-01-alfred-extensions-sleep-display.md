---
comments: true
date: "2011-11-01"
layout: post
slug: alfred-extensions-sleep-display
title: '[Alfred Extensions] Sleep Display'
categories: ['Works']
tags: ['Alfred', 'Display', 'mac']
---

[Alfred](http://www.alfredapp.com) [Powerpack](http://www.alfredapp.com/powerpack/) allows users to use extensions. And I do need a shortcut to sleep my displays. That's how it comes from.

A simple extension to sleep display quickly. It's a wrapper of a binary file.
The code to generate the binary is from [Command to Sleep Display OSX](http://stackoverflow.com/questions/1239439/command-to-sleep-display-osx)

	#include <CoreFoundation/CoreFoundation.h>
	#include <IOKit/IOKitLib.h>

	/* Returns 0 on success and 1 on failure. */
	int display_sleep(void)
	{
	    io_registry_entry_t reg = IORegistryEntryFromPath(kIOMasterPortDefault, "IOService:/IOResources/IODisplayWrangler");
	    if (reg) {
	                IORegistryEntrySetCFProperty(reg, CFSTR("IORequestIdle"), kCFBooleanTrue);
	                IOObjectRelease(reg);
	        } else {
	                return 1;
	        }
	        return 0;
	}

At the beginning, I cannot run the binary inside the extension directory, but my home or my previous session's directory. And that's because I didn't check the "Silent" option, which makes the default startup directory is the extension's.

Well, it's easy to use: just type the keyword, and hit Enter.
~~What I hope is Alfred 1.0 to support Global Hotkeys for extensions.~~

Oh, forgot to say, [Download it here](http://wwwjfy.net/tools/SleepDisplay.alfredextension).

**Update** I've seen some searches for a version for Alfred 2 directed here, so [here is the one](http://wwwjfy.net/tools/SleepDisplay.alfredworkflow).

PS, [PadLock](http://itunes.apple.com/us/app/padlock/id415667090?mt=12) is another app doing the same thing costing $2.99. The other feature of the app is to give you the buffer to cancel the lock operation, useful also.
