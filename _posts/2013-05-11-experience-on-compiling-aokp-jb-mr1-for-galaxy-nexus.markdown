---
comments: true
date: 2013-05-11 08:40:39+00:00
layout: post
slug: experience-on-compiling-aokp-jb-mr1-for-galaxy-nexus
title: Experience on compiling AOKP jb-mr1 for Galaxy Nexus
categories:
- Misc
tags:
- android
- aokp
- openpdroidpatches
---

The other day, I heard about [OpenPDroidPatches](https://github.com/OpenPDroid/OpenPDroidPatches), which adds permission management to Android.

This supports AOKP, the rom I'm using, and permission management in Android has always been a pain in ass. I now have reasons to try to compile AOKP myself. Plus, it'd be fun.

## Environment

### Failed: on Mac OS X

As I'm using the Macbook Air, so at first I tried to compile under OS X.

That turned out tragic, where I saw some random errors every time and no idea where the problems were. Well, I gave up soon as it seems that the compilation is not designed to be done on OS X. (or maybe I'm too inexperienced.)

### EC2 vs DigitalOcean

They were all running Ubuntu 12.04, and no errors during compilation, while the performance differed a lot.

It took about 3 hours and 40 mins to compile on EC2, and about only 45 mins to compile on DigitalOcean.

EC2 configuration: (High-CPU Medium Instance) 1.7 GB of memory, 5 EC2 Compute Units (2 virtual cores with 2.5 EC2 Compute Units each), 350 GB of local instance storage, $0.145/hr.

DigitalOcean configuration: 16GB of memory, 8 Cores CPU, 160GB **SSD**, $0.238/hr.

I have to say, SSD does boost the performance a lot.

## Compilation

Basically, I followed the official [[TUTORIAL] Building AOKP [Ubuntu 12.04+]](http://rootzwiki.com/topic/31166-tutorial-building-aokp-ubuntu-1204/).

One thing is easy to be missing is that do not download all the kernel, but only what is needed. In my case, edit kernel\_manifest.xml to delete all <project .../> except

    <project path="kernel/samsung/tuna" name="imoseyon/leanKernel-galaxy-nexus" remote="aokp" revision="lk-jb-mr1" />

And then

    . build/envsetup.sh; brunch maguro

As to [OpenPDroidPatches](https://github.com/OpenPDroid/OpenPDroidPatches), the patches are totally fine, without any conflict, and after applying, there is a "Permissions" item in Settings.
