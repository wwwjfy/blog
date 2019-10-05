---
comments: true
date: "2009-03-24"
layout: post
slug: problem-when-using-pygtk-with-crontab
title: Problem when using pygtk with crontab
categories: ['Linux']
tags: ['crontab']
---

since I successfully used pynotify with crontab, a new problem occured.

I wanted to create a gtk.Window() to display something, but comes some error.

	GtkWarning: Screen for GtkWindow not set; you must always set a screen for a GtkWindow before using the window

that wierd. I've set DISPLAY=:0.0 in the python script.

use an alternative way, define the DISPLAY in crontab:

	* * * * * env DISPLAY=:0.0 /path/to/reminder.py

and the window can display normally.

If I have time, I'll find out the reason about it.
