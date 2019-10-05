---
comments: true
date: "2013-04-24"
layout: post
slug: solve-scrachbox-is-not-properly-set-up
title: Solve "Scrachbox is not properly set up"
categories: ['Tips']
tags: ['maemo', 'meego', 'scrachbox']
---

Recently, a patch is proposed to stabilize smsbackuprestore by limiting the queried item number each time to 1000.

That sounds cool. So I set up a new Linode VPS to get it done. The problem is once I rebooted VPS after scratchbox is installed, I got this message:

	Scratchbox is not properly set up.

After googling, this command didn't help:

	sudo /scratchbox/sbin/sbox_ctl start

Well, let the code talk. It turned out some mounts were not there any more, which were set up the first time starting scratchbox.

So, let me make sbox_ctl think it's the first time by

	sudo touch /scrachbox/.run_me_first_done

And then run the sbox_ctl command, and now it's fine.