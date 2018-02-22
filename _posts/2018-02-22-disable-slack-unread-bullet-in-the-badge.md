---
date: "2018-02-22 14:41:45 +0800"
layout: post
slug: "disable-slack-unread-bullet-in-the-badge"
title: "Disable Slack unread bullet in the badge"
categories:
- Tips
tags:
- Slack
- mac
---

I like things clean. So I don't want Slack's lacking settings to remove the
unread bullet in the badge. (Unlike the mentioned/DM messages which show a
number in the badge, unread messages in channels show `•`)

Because Slack's Mac App is built using [Electron
framework](https://electronjs.org), I was thinking of hacking the source code
to remove the "feature".

I went to `/Applications/Slack.app/Contents/Resources`, and uncompressed
`app.asar` using `asar e app.asar <tmp dir>`. I tried to hack around and
re-packed the code, but it seemed not to work. I suspect Slack downloads latest
javascript files from their own server. I didn't want to take time to do the
MitM, which could be tedious.

Then I saw `electron.asar` in the same directory of `app.asar`. It turned out
to be a much easier way to achieve my goal. The diff is quite straightforward:

```diff
diff --git a/browser/api/app.js b/browser/api/app.js
index 381a2a8..0d0cdad 100644
--- a/browser/api/app.js
+++ b/browser/api/app.js
@@ -44,7 +44,13 @@ if (process.platform === 'darwin') {
     },
     cancelBounce: bindings.dockCancelBounce,
     downloadFinished: bindings.dockDownloadFinished,
-    setBadge: bindings.dockSetBadgeText,
+    //setBadge: bindings.dockSetBadgeText,
+    setBadge (label) {
+        if (label === '•') {
+            label = ''
+        }
+        return bindings.dockSetBadgeText(label)
+    },
     getBadge: bindings.dockGetBadgeText,
     hide: bindings.dockHide,
     show: bindings.dockShow,
```

Tip: to get console output, add `console.info` as usual, and start the
application in the terminal, like
`/Applications/Slack.app/Contents/MacOS/Slack` and the output will be in the
stdout.
