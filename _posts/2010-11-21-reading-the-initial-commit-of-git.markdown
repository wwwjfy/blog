---
comments: true
date: 2010-11-21 12:39:08+00:00
layout: post
slug: reading-the-initial-commit-of-git
title: Reading the initial commit of Git
categories:
- Learning
tags:
- git
---

I began to learn and use git recently, for personal use and work management. And I get curiosity about how git is implemented. Well, the source code is so huge now, which makes me decide to read the initial git code.

There are only a few files, but I could see the basic framework of git.

#### init-db.c

This file simply create ".dircache" directory in current directory, or according to environment variable SHA_FILE_DIRECTORY, which will make all tracked files be stored in a global place. However, as mentioned in the comments, it'll make things messier, maybe that's the reason it's got rid of.

also, it creates objects directory and "00" to "ff" 256 directories for storage.

#### read-cache.c

It contains common cache functions.

##### int read_cache(void)k

read .dircache/index, and loads cache_header and cache entries information.

##### char *sha1_file_name(unsigned char *sha1)

get file name with path for the given sha1

#### update-cache.c

all arguments, which are file/directory names, will be added/removed to cache.

The cache entry contains type("blob"), file size, and file content, which are compressed by zlib. The SHA1 is also got by all these three. The whole thing will be written in objects directory, so everything could be recovered. And of course, refresh the cache file ".dircache/index".

#### cat-file.c

simply read, decompress and write the content to temp file temp_git_file_XXXXXX.

#### read-tree.c

simple too. It reads a "tree" cache, and print all files contained by the tree.

#### show-diff.c

check all cache entries, to see if it's changed, which will check mtime, ctime, owner, mode, inode and file size.

#### write-tree.c

as the name suggests, it writes "tree" to .dircache/objects

#### commit-tree.c

it generates a "commit" cache, with tree cache information, author, committer, commit comment, and parent information.

Yes, not so easy to use, but usable, as file management system, without many convenient commands.
