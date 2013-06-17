---
comments: true
date: 2009-03-14 23:38:00+00:00
layout: post
slug: solution-on-guifications-plugins-for-carrier
title: Solution on guifications plugins for carrier
categories:
- Linux
tags:
- carrier
- pidgin
---

The IM client I'm using is Fun Pidgin(http://funpidgin.sourceforge.net/). It improves pidgin, and fixes some bugs, so I can get good user experience on it.
But I met a problem when I wanted to install pidgin-guifications, the well-known plugin for pidgin.

I used


	sudo apt-get install pidgin-guifications


but it did not work. I cannot find it in carrier(Fun Pidgin)'s plugin list.

so I downloaded the source (http://plugins.guifications.org/trac/downloads) to compile.

Then ran configure as usual.

It said


	No package 'pidgin' found


Indeed, I didn't install pidgin, but carrier.

edit the configure.
replace 'pidgin purple' with 'carrier'.

then configure, make, and make install.

Surprisingly, I found there are /pidgin, /pixmap, and /locale. It must have considered prefix as '/', but not something like '/usr/lib'. It's the configure's problem.
search 'pidgin' word by word, and found that


	pkg-config --variable=libdir pidgin


replace such 'pidgin' with 'carrier'.
And problem resolved.

I can now use carrier well. :)
