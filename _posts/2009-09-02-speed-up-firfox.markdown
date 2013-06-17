---
comments: true
date: 2009-09-02 02:26:00+00:00
layout: post
slug: speed-up-firfox
title: Speed up Firfox
categories:
- Tips
---

I read news on Solidot, and found SpeedyFox can speed up firefox by optimizing profile.

I'm using swiftfox as replacement, which is optimized for specified architecture. It makes firefox much faster, indeed. But I won't refuse to have a faster one, of course. :)

Unfortunately, only Windows version is available. Since it's to optimize the sqlite file, we can do it by ourselves.

From
[http://webupd8.blogspot.com/2009/07/increase-firefox-3-perormance-by.html?dsq=12464538](http://webupd8.blogspot.com/2009/07/increase-firefox-3-perormance-by.html?dsq=12464538)

I found the command

	for f in ~/.mozilla/firefox-3.5/*/*.sqlite; do sqlite3 $f 'VACUUM;'; done

OK, the profile directory is smaller, though I cannot feel the faster speed at startup.