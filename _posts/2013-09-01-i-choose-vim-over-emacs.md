---
comments: true
date: "2013-09-01 23:42:11 +0800"
layout: post
slug: "i-choose-vim-over-emacs"
title: "I choose Vim over Emacs"
categories:
- Misc
tags:
- Vim
- Emacs
---
I've been using Vim as my main editor since a few years ago when I got to work
under Linux. Recently, I use [Sublime Text](http://sublimetext.com) more and
more, but Vim is still the go-to editor, especially on terminal. But I always
have the desire to try Emacs, as I've heard much about it, and actually I use
the basic navigation keys every day.

#### Likes ####

- What I like about Emacs at the first impression is that I can directly input
  what I'm going to write down and the navigation is simple, just the same way
  what I use daily. Ctrl-A, Ctrl-E, Ctrl-N, Ctrl-P, etc. Unlike Vim, I don't
  need to know what mode I'm in (yes, there are modes in Emacs, in a different
  way).

- And Elisp seems more like a modern programming language than Vim script. I
  may not need to look up the meaning of the weird syntax because it's just
  lisp. I don't worry about the ability as it's a mature language for a long
  time.

#### Dislikes ####

- It's heavy. I don't like it takes several seconds to load .emacs and other
  packages to edit a small, plain-text file. So later I tried the daemon mode,
  which starts a "server" in the background, and only a lightware client is
  needed to edit a file. It seemed acceptable for me at first. But when I edit
  files in two projects and I want to switch to another buffer, I have to go
  through a long mixed list to get there. That's a real pain in ass. Any
  workaround need extra step which I hate to do when it's supposed I shouldn't
  need to.

- It's a bit difficult to get it work as I like. I had to spend quite some time
  learning how it works to get something to follow my mind. And I cannot work
  some out still.

  - Why the hell can't I manully increase/decrease the indent level. <TAB> and
    <Shift-TAB> are supposed to do that, but their work in Emacs is to indent
    the line according to the syntax table (well, it's to calculate the
    _correct_ indent) if it doesn't. Well, sounded reasonable to me at first,
    that I should have to do it manually. But sometimes the syntax table is
    just _wrong_. Now I can do nothing but use backspace to delete the white
    space one by one.

  - There is a feature called auto-save. It generates files named
    #\<filename\>#, just like \<filename\>~ in Vim. I want to have it in one
    unique directory, instead of spreading all around. No, setting
    `backup-directory-alist` doesn't work, though it claims to.

  - There are some other small but annoying details.

- My left little finger is protesting, even though I've mapped Caps Lock to
  Ctrl.

### Choice ###

I think that let me just stick to Vim, just to add more maps to make it easier
to navigation in edit mode to make it less frequent to jump between modes.
