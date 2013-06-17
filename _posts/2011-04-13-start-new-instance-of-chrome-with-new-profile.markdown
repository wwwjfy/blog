---
comments: true
date: 2011-04-13 02:31:29+00:00
layout: post
slug: start-new-instance-of-chrome-with-new-profile
title: Start new instance of Chrome with new profile
categories:
- Tips
tags:
- chrome
- mac
---

To start a new Chrome, using the same or different version bundle.

    cd /Application/Google\ Chrome.app/Contents
    ./MacOS/Google\ Chrome --user-data-dir="/path/to/profiles"

P.S. For all command line switches, check a bit outdated [reference](http://www.ericdlarson.com/misc/chrome_command_line_flags.html), or from [source code](http://src.chromium.org/svn/trunk/src/chrome/common/chrome_switches.cc).
