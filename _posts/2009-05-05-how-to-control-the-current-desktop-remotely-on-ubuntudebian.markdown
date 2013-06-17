---
comments: true
date: 2009-05-05 11:51:00+00:00
layout: post
slug: how-to-control-the-current-desktop-remotely-on-ubuntudebian
title: How to control the current desktop remotely on Ubuntu/Debian
categories:
- Linux
---

I need to connect to the current desktop in the company remotely to deal with some urgent things. And I can now ssh to my computer.

I thought of vnc, but it would create a new desktop instead of the current one. Finally I found vino-server. It's used to view/control the current desktop.

But it was not easy for me to enable and configure it to meet the needs.

The configure file is $HOME/.gconf/desktop/gnome/remote_access. Use

	gconftool-2 -s -t bool /desktop/gnome/remote_access/enabled true

to enable it. Then you'll see your 5900 port is open now. If not, manually run

	/usr/lib/vino/vino-server --display=:0.0

to check if any error occurs.

Now you can connect to the machine using

	vncviewer <machine-name>

But what you see is the black screen. On the host computer, a dialogue will show to refuse/allow the connection.

Run

	gconftool-2 -s -t bool /desktop/gnome/remote_access/prompt_enabled false

to cancel the confirmation.

More can be configure via System(Desktop on Debian)/Preferences/Remote Desktop after you connect to the desktop.

P.S. the password is saved in /desktop/gnome/remote_access/vnc_password base64-encoded.
