---
comments: true
date: "2009-04-08"
layout: post
slug: visit-a-ticket-page-on-trac-quickly
title: Visit a ticket page on trac quickly
categories: ['Tips']
---

I visited ticket pages on trac frequently, but I found that I had not got a quick way to open one.

I thought I need a search engine like Google on firefox to visit what I want quickly.

The configuration file of Google search text box on firefox is in /path/of/firefox/searchplugins along with something else.

I wrote something like this, and save it as ticket.xml:

	<SearchPlugin xmlns="http://www.mozilla.org/2006/browser/search/">
	<ShortName>Ticket</ShortName>
	<Description>Ticket Search</Description>
	<InputEncoding>UTF-8</InputEncoding>
	<Image width="16" height="16">data:image/x-icon;base64,{base64 of icon file}</Image>
	<Url type="text/html" method="GET" template="https://localhost/trac/ticket/{searchTerms}">
	</Url>
	</SearchPlugin>

The icon will be encoded by base64 and pasted to the {base64 of icon file}. Of course, it's not necessary.

Then, restart firefox, and the ticket search engine appears. So easy, isn't it? :)
