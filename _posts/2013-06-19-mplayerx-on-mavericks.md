---
comments: true
layout: post
title: "MPlayerX on Mavericks"
categories:
- Works
tags:
- mplayerx
- mavericks
- os x
- 10.9
---

I recent excitedly upgrade to OS X 10.9 Mavericks. One of the biggest problems is the MPlayerX crashed on Mavericks.

Thanks to its open-source, I checked out the source.

So, the problem is that a value was used before it was initialized, and it was divided as zero. It's the time slider's max value, being used to calculate the knob.

The patch is [here](https://github.com/wwwjfy/MPlayerX/commit/b163a10a9e2ed2a0b13557727a7b3832b100052e), also I committed some possible fixes in the repository.

The workable version can be downloaded [here](https://copy.com/vKS7FuBIpJIZ ), with the [dSYM](https://copy.com/GJblo3iSUahy).
