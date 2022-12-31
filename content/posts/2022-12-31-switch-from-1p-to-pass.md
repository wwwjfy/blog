---
date: "2022-12-31"
layout: post
slug: switch-from-1p-to-pass
title: 'Switch from 1Password to pass'
categories: ['Misc']
tags: ['1password', 'pass', 'password']
---

I had been using 1Password since version 3, from 2011, and paid for upgrades and desktop/mobile versions. And I had been pretty happy with it managing my passwords, with its easy to use UI and right amount of features and **scope** (it knew what it should do and what it should not).

Until later it turned to subscription mode, offering its own cloud storage. While I understand everyone is starting subscription mode to get more dollars, I don't think passwords makes sense to be hosted without my full control, even in the world without all those password management services being hacked.

Well, you do you, and I vote with my feet.

---

I have turned to [pass](https://www.passwordstore.org) and been pretty happy with it. It's simple but enough for me. I have full control over everything and am totally fine with its disadvantages.

It's [opensourced](https://git.zx2c4.com/password-store/), and its core file has fewer than 1k lines, accompanied with platform specific functions (clipboard revocation etc.). Some supporting utils are also available, shell completion, editor support, scripts to import from other password management tools, and so on.

It took me around half an hour to set up and import (There were some issues importing 1Password, and I had to look at the code and fix them because of some weird values. And it's also the beauty of opensource).

- **Pros**
  - opensourced. If something is not working the way I expect, I can just make the change and no need to wait for anyone to approve and implement it. It's not complex, just a bunch of bash functions calling utils.
  - file based standard encrypted password. I use command line as I prefer not leaving keyboard if possible. And because it's just a bunch of GPG encrypted files, there are quite some apps in various platforms.
  - full control. Back it up everywhere as you want, have the files under git to have version control, to start with. And all these are totally free.

- **Cons**
  - less smooth browser integration. 1Password supports the main browers with plugins or extensions, making it easy to do auto-fill. I have not yet found / developed a tool to do the same on macOS. On iOS there is (to be covered below).
  - no out of box support. Except the basic feature, itself has no out of box integration with any other tool. I need to put that in my tool chain. It's in cons, but that's the nature of most (all?) unix tools, that you the users decide how to link them to work the best for you.
  - grep is slow. GPG encryption and decryption for one file has no delay, but that's not the case for hundreds of files. So either find a way to cache the info somewhere, or organize it properly (like putting url in the filename).

---

For reference, I'm sharing my setup.

- Core. Instead of the original [pass](https://www.passwordstore.org) written in bash, I'm using [gopass](https://github.com/gopasspw/gopass). It's a re-implementation in Go, with features making it easier to use. I only need its auto-sync-with-remote-for-each-operation feature, as I always forget to do git push, causing other devices not able to get updates. And at least for me, Go is much more friendly as a general-purpose programming language than Bash.
- iOS. I'm using also opensource project [passforios](https://mssun.github.io/passforios/). And as passwords are sensitive, I compiled and installed it myself. It can do CRUD, but can't resolve conflicts. Luckily I don't need to do that often.
- Alfred integration. I'm using [alfred-pass](https://github.com/CGenie/alfred-pass) to save a little time to switch to terminal and open new tab. It's applicable if only password is needed. (Yes, I should add another command to copy username/email.)
- GPG.
  - I installed it by `brew install gnupg`.
  - I don't want macOS keychain to save my passphrase, so disabled it.
```bash
defaults write org.gpgtools.common UseKeychain NO
```
  - I set the default timeout, i.e. how long I don't need to input passphrase again, to 10 min.
```bash
$ cat ~/.gnupg/gpg-agent.conf
default-cache-ttl 600
pinentry-program /Users/tony/bin/pinentry.sh
```
  - I'd like to have terminal prompt for passphrase, but Alfred won't be able to do that. I learned from [here](https://github.com/CGenie/alfred-pass/blob/master/pinentry.sh) to have own pinentry script to trigger pinentry-mac and pinentry-tty for GUI and TUI respectively.
    Note: need to set PINENTRY_USER_DATA to `gui` where pinentry-mac is needed, and something else, anything but empty string, otherwise, it'll be the value from first user of gpg-session.

---

While there are definitely a lot of places to improve in the workflow, but I'm now pretty happy with what's already working and I don't mind a little trouble copy/paste usernames when it's needed, to have my own control over everything.
