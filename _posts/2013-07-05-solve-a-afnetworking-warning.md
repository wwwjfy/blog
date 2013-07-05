---
comments: true
date: "2013-07-05 23:47:13 +0800"
layout: post
slug: "solve-a-afnetworking-warning"
title: "Solve a AFNetworking warning"
categories: 
- Tips
tags:
- mac
- osx
- objc
- AFNetworking
- CFNetwork
---

As Google Reader shut down on July 1st, I moved to use [Stringer](https://github.com/swanson/stringer), which is in Ruby and provides Fever-compatible API.

Also, disappointing Reeder's Mac version is still in development and no ETA. I need a simple client to browse RSS items every day. I'm making my own.

It's going all right, when suddenly this error kept occurring out of nowhere:

	ERROR: unable to get the receiver data from the DB!

Google helped zero.

This happens when [AFNetworking](https://github.com/AFNetworking/AFNetworking) was doing net requests.

After [grep](https://github.com/ggreer/the_silver_searcher) all the frameworks (right, it was a stupid way), it turned out to be CFNetwork. Oops, so the best guess is the cache.

I was right. After cleaning the app cache directory in ~/Library/Caches, it works well now.

No idea if it's a usual error, but it's definately a bad way to show the warning. Either recover it yourself, or specify what the hell is wrong explicity.