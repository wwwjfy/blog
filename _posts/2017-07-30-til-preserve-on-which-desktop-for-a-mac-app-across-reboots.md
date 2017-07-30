---
date: "2017-07-30 20:56:29 +0800"
layout: post
slug: "til-preserve-on-which-desktop-for-a-mac-app-across-reboots"
title: "TIL: preserve on which desktop for a Mac app across reboots"
categories:
- Tips
tags:
- mac
---

I developed a small utility app which has one `NSWindow`. I usually put it on Desktop 2 (IIRC it was called workspace? But it seems Apple doesn't do it any more), but when I rebooted the OS, it went to show on Desktop 1, which is a bit annoying. I had failed to find the solution throughout Apple documents and random experiments (well, it turned out I have lame search skill).

- `Autosave name` property of NSWindow in Interface Builder saves the frame across the app launches, but not the Desktop across OS launches.

- Programmatically using `setFrameUsingName` works the same way.

  Actually, the property that work for it is `NSWindow`'s `isRestorable`, settable in Interface Builder as `Restorable` in the `Behavior` section of `NSWindow`.