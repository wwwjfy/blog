---
comments: true
date: "2014-08-14 09:19:04 +0800"
layout: post
slug: "use-autoload-in-emacs"
title: "Use autoload in Emacs"
categories:
- Tips
tags:
- Emacs
- Elisp
- Autoload
---
Not liking leaving things imperfect,  I recently switched back to Emacs for some
tasks. It's great to do typing, without worrying about mode switching like vim.

A while ago, I wrote [a elisp script](https://github.com/wwwjfy/emacs-fish/) as
a major mode for [fish](http://fishshell.com/). This is a simple version, with
keyword highlight and basic indent.

There was an issue [here](https://github.com/wwwjfy/emacs-fish/issues/4). It's
about to use `autoload` to speed up Eamcs's notorious slow start-up, also to
save manual `require`, and `auto-mode-alist`, `interpreter-mode-alist` settings.

There are two ways to use `autoload`: explicitly execute that, or with a magic
comment prepending the call.
[Check out the document](http://www.gnu.org/software/emacs/manual/html_node/elisp/Autoload.html).

The magic comment doesn't work itself, which bothered me once because I didn't
read the document carefully, which is

    Magic comments are the most convenient way to make a function autoload, for
    packages installed along with Emacs.

    These comments do nothing on their own, but they serve as a guide for the
    command update-file-autoloads, which constructs calls to autoload and
    arranges to execute them when Emacs is built.

So for manual usage, you have to execute `update-file-autoloads`, or
`update-directory-autoloads` to generate/update the autoloads file, and load the
file in emacs configuration, to make it work.

Well, if the package is a common one, make it into a package repository would
save the work, as the package manager would automatically do that. Check out a
package in `~/.emacs.d/elpa/`, there would be files like
`fish-mode-autoloads.el`.

_PS_: To make changes and take effect in files in the directory, execute
`byte-compile-file` or `byte-recompile-directory` after changes, because Emacs
will load the .elc file.
