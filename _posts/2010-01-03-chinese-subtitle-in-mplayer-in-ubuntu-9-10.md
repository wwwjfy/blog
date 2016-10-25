---
comments: true
date: 2010-01-03 14:44:31+00:00
layout: post
slug: chinese-subtitle-in-mplayer-in-ubuntu-9-10
title: Chinese subtitle in MPlayer in Ubuntu 9.10
categories:
- Tips
tags:
- mplayer
- subtitle
---

The solutions comes from [http://wiki.ubuntu.org.cn/MPlayer](http://wiki.ubuntu.org.cn/MPlayer)

The Chinese subtitle couldn't show correctly in mplayer in Ubuntu 9.10, and the old solutions don't work.

Today, I found solutions in the wiki of Ubuntu.org.cn.

There are 3 solutions:

1. modify LC\_CTYPE: set environment LC\_CTYPE=zh\_CN.utf8.

2. ass=yes in mplayer configure, or check 'SSA/ASS subtitle rendering' in tab 'Subtitles & OSD' in gmplayer.

3. use the font name like 'uming', instead of the path of the font file.

Good to know that. :)

P.S. And what a snow in Beijing! It's so beautiful, and cold, too. ;)