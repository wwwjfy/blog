---
comments: true
date: 2009-03-23 13:39:00+00:00
layout: post
slug: problem-when-using-pyinotify-with-crontab
title: Problem when using pyinotify with crontab
categories:
- Linux
tags:
- crontab
---

I write a script by Python as a reminder, and want it to run every half an hour. So I use crontab to complete the task.

run

	crontab -e

and ad

	0,30 * * * * /path/to/reminder.py

but it did not work.

I want to know what the problem is. So change it to

	0,30 * * * * /path/to/reminder.py 2> /path/to/err


and I got it

	GtkWarning: could not open display

The problem was that cron has not the environment variable DISPLAY.

The way is to set it on the script

	import os
	os.environ['DISPLAY'] = ':0.0'

and problem solved.
