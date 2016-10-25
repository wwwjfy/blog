---
comments: true
date: 2011-08-06 16:31:22+00:00
layout: post
slug: notes-on-postgresql-9-0-high-performance-1
title: Notes on Postgresql 9.0 High Performance (1)
categories:
- Learning
tags:
- performance
- postgresql
---

Regarding the hardware, there are 3 parts basically: CPU, Memory and Disk.

For CPU:

There are two choices: Faster cores or more cores. It depends on the application. Postgresql runs queries by processes, so if the application need single complicated queries, faster cores should be the one, and if the application does the small update quite frequently, like real-time games, more cores can help more.

For Memory:

It doesn't worth upgrading memory if: 1) the database is small enough that it can already be totally read into memory. 2) the tables are much bigger that it's better to enhance CPU and disk. However, reading part of frequently accessed data into memory will be helpful.

For Disk:

SSD is a bit small, the others are a bit slow. Need to consider much, including different disk type (SATA, SAS), RAID, disk write caches, etc, to choose disk(s). Well, I don't care about that much, leave it now.
