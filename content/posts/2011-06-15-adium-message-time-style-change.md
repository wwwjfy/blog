---
comments: true
date: "2011-06-15"
layout: post
slug: adium-message-time-style-change
title: Adium message time style change
categories: ['Tips']
tags: ['adium', 'font', 'time']
---

I'm using Adium now. But the problem is when I enlarge the font of messages in "Messages" tab of Preferences, the font of time doesn't change accordingly. So I've got big enough message text, and half size time text. That's really uneasy to get used with.

Common sense, that it's hard to expect the developer to deal with your requirement in high priority, they always get more important things to do. I have to help myself.

Luckily, it's controlled by a css file. As my Adium is installed in /Application, and I'm using "yMous" style, the css file is located as

    /Applications/Adium.app/Contents/Resources/
    Message Styles/yMous.AdiumMessageStyle/Contents/Resources/Styles/Base.css

There is a "font-size: 0.5em" in a .x-time, change that to 1em, and restart Adium, and everything looks beautiful!

**Update**: It's boring to change it every time when Adium updates, so I moved it to ~/Library/Application Support/Adium 2.0/Message Styles, and changed its name to yMous-big.AdiumMessageStyle, and changed the Info.plist accordingly. Open the new folder in Finder or "open", "new" message style installed.



