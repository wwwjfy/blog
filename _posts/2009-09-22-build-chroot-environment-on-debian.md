---
comments: true
date: 2009-09-22 17:45:57+00:00
layout: post
slug: build-chroot-environment-on-debian
title: Build chroot environment on Debian
categories:
- Linux
---

**Reference**:

[http://www.debian-administration.org/articles/356](http://www.debian-administration.org/articles/356)

[http://alioth.debian.org/docman/view.php/30192/21/debian-amd64-howto.html#id292205](http://alioth.debian.org/docman/view.php/30192/21/debian-amd64-howto.html#id292205)

The architecture is amd64, so some 32-bit applications could not work well, even with ia32 libs. One solution is to build 32-bit chroot environment. Not only for 32-bit, of course.

Follow the official tutorial.

- Install needed packages for building

        sudo apt-get install debootst

make a directory to place the new system.

- Build the basic system

	    debootstrap --arch i386 sid /chroot http://ftp.debian.org/debian/

sid is the version, and /chroot is the directory created just now, and the mirror URL follows. Change the i386 into any architecture you like, e.g., amd64.

- Mount /dev and /proc

	    #/etc/fstab
	    /dev            /chroot/dev     none    bind            0       0
	    /proc           /chroot/proc    proc    defaults        0       0
	    /sys            /chroot/sys     none    bind            0       0

run 'mount -a' to mount. And other directories like $HOME can be mounted as you like.

- chroot

Now, we can use

    sudo chroot /chroot

to change the environment into /chroot

- schroot

schroot is a more powerful tool to chroot.

    sudo apt-get install schroot
    sudo vi /etc/schroot/schroot.conf

uncomment the first part. change the users, groups, root-groups to your own.

now sudo is not needed any more.

There is a parameter, '-p', which introduces the environment vars of the external system, while I prefer to configure the env vars myself.

## Problems ##

- X window cannot be opened.

The system should know which display to show the window. It prompts

	unable to open display ""

So

	export DISPLAY=":0.0"

sometimes it doesn't work, please try

	export DISPLAY="0:0"

I don't know the reason, but it actually works on mine.

Then run

	sudo gdmsetup

uncheck 'Deny TCP connections to Xserver' on the 'Security' tab.

add

	xhost +localhost

to $HOME/.profile to make sure localhost is permitted to use X.

- copy hosts/resolve.conf to the chroot automatically

all files' name should be put in /etc/schroot/copyfiles-defaults, extra files, like .vimrc, .bashrc, can be added on demand. Add

	run-setup-scripts=true
	run-exec-scripts=true

to schroot.conf. So that schroot will deal with the file copying, mounting, proc killing when logout, and so on.

All the scripts are located in /etc/schroot.ap