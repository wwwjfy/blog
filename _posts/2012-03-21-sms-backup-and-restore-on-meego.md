---
comments: true
date: 2012-03-21 04:59:23+00:00
layout: post
slug: sms-backup-and-restore-on-meego
title: SMS Backup and Restore on Meego
categories:
- Works
tags:
- backup
- export
- import
- meego
- n9
- restore
---

Recently I've got a N9 to play with. The boring thing is I cannot restore my old SMSs. So I wrote a small utility to backup and restore.

The repo is located at [GitHub](https://github.com/wwwjfy/meegoSMSBackupRestore). The deb file can also be downloaded [there](https://github.com/wwwjfy/MeegoSMSBackupRestore/downloads).

_Notice_ To compile from source code, follow the intruction on [Harmattan Platform SDK](http://harmattan-dev.nokia.com/platform-sdk/) to build the environment, and run

    apt-get install libcommhistory-dev

to have libcommhistory headers. (Thanks [Philipp Zabel](mailto:philipp.zabel@gmail.com) for helping me on that)



