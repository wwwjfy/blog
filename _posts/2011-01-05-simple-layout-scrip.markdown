---
comments: true
date: 2011-01-05 14:30:15+00:00
layout: post
slug: simple-layout-script
title: 简单的窗口布局的脚本
categories:
- Tips
tags:
- linux
- windows
---

现在都用大显示器了，而且宽屏，很多时候程序用不了整个屏幕的宽度，就把同时显示两个窗口，节约空间。

Mac OS X 下有个叫 SizeUp 的软件，可以方便地控制当前窗口的位置，屏幕的左半部分，右半部分等。一键搞定，很方便，就这样就$13了。

Linux 下可以用很简单的脚本搞定，在 Gnome/Ubuntu 下没问题。

首先用apt-get install wmctrl装上wmctrl.

居左：

    wmctrl -r:ACTIVE: -bremove,maximized_horz,maximized_vert; wmctrl -r:ACTIVE: -e`wmctrl -d | awk '/\*/ {print $6,$8,$9}' | awk '{split($2, wa1, ","); split($3, wa2, "x")} END {print 0","wa1[1]","wa1[2]","int(wa2[1]/2)","wa2[2]}'

居右：

    wmctrl -r:ACTIVE: -bremove,maximized_horz,maximized_vert; wmctrl -r:ACTIVE: -e`wmctrl -d | awk '/\*/ {print $6,$8,$9}' | awk '{split($2, wa1, ","); split($3, wa2, "x")} END {print 0","int(wa1[1]+wa2[1]/2)","wa1[2]","int(wa2[1]/2)","wa2[2]}'

很简单，可以根据需要设成任意比例。

我把这两个功能分别设为Win+<-，Win+->。就很方便地改变窗口布局了，至少对我是够用了。
