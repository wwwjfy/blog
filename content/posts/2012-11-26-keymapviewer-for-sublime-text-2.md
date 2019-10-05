---
comments: true
date: "2012-11-26"
layout: post
slug: keymapviewer-for-sublime-text-2
title: KeymapViewer for Sublime Text 2
categories: ['Works']
tags: ['keymap', 'sublime text 2']
---

It's a problem that sometimes one or some of the key bindings in muscle memory not working, that we don't know which one of the newly-installed or newly-updated plugins/packages uses that.

My example is that [Emmet](https://github.com/sergeche/emmet-sublime)(Previously Zen-coding) used Ctrl-D to do something I don't care, but I could not delete the next character because of that.

So I have to go to the Sublime Text 2's packages path and grep all .sublime-keymap file to find out which file "ctrl+d" is in. Then I got Emmet. Then I found the function and disabled it.

I think it's handy that I can do all that in Sublime Text 2, no bothering to open a terminal. Package Control gave me [KeymapManager](https://github.com/welefen/KeymapManager), which triggers a command by its package name (or command name? I don't remember exactly). But this is not what I want, and I found it a bit buggy and imperfect, and it's not a manager.

Inspired by that, I wrote my own one: [KeymapViewer](https://github.com/wwwjfy/KeymapViewer).
Two commands provided in Command Palette:

- Search by key: Search the command by key strokes. Enter "ctrl+super+p" in the panel and you can see

		Package: Default
		Command: show_overlay
		Args: {u"overlay": u"command_palette"}

- Show by packages: Show panel with all packages except the ignored packages.

Hit Return in both panels and the corresponding .sublime-keymap file will open.
