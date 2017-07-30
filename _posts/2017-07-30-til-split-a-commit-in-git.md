---
date: "2017-07-30 20:16:39 +0800"
layout: post
slug: "til-split-a-commit-in-git"
title: "TIL: split a commit in Git"
categories:
- Tips
tags:
- git
---

Today I created a commit in git, which contained changes related to two different issues. If I realised that just after the commit, I can use

```shell
git reset HEAD~
```

to reset the commit history to the previous state, and redo it.

Unfortunately, I found that after a few other commits.

Of course I can still reset to the earliest commit and cherry-pick all later commits, but the idea dawned on me: Can I do that with git rebase?

It turned out I can. It's quite simple:

1. git rebase -i \<commit hash\>
2. copy and paste the commit I want to split and make both lines "edit"
3. edit by disgarding unrelated changes and commit
4. repeat, but for the other part
5. done

After the successful try, it seemed intuitive, and I'm glad to learn something. :)