---
comments: true
date: 2011-03-02 10:47:19+00:00
layout: post
slug: compile-vim-7-3-in-mac-os-x-10-6-6
title: Compile vim 7.3 in Mac OS X 10.6.6
categories:
- Tips
tags:
- mac
- vim
---

**Update:** I can now compile vim without any tweak on OS X 10.7.4 with Homebrew without Macports. Also check the comments from jay and jefk, which make the compilation as easy.

The default vim of Max OS X is 7.2 without +python. I need pyflakes plugins, so I have to compile it. I like using terminal, so I don't use MacVim.


configure and make with simple options:


    ./configure --prefix=/usr --enable-perlinterp=yes --enable-pythoninterp=yes --with-features=huge
    make


I got such error:


    Undefined symbols for architecture x86_64:
    "_iconv", referenced from:
    _buf_write_bytes in fileio.o
    _readfile in fileio.o
    _my_iconv_open in mbyte.o
    _string_convert_ext in mbyte.o
    (maybe you meant: _my_iconv_open)
    "_iconv_close", referenced from:
    _buf_write in fileio.o
    _readfile in fileio.o
    _my_iconv_open in mbyte.o
    _convert_setup_ext in mbyte.o
    "_iconv_open", referenced from:
    _my_iconv_open in mbyte.o
    (maybe you meant: _my_iconv_open)
    ld: symbol(s) not found for architecture x86_64
    collect2: ld returned 1 exit status
    make[1]: *** [vim] Error 1
    make: *** [first] Error 2


From google, I know that libiconv from macports doesn't support x86_64.
**Update:** Thanks to jay's remind, the libiconv comes with system should be fine. So, to specify the library, use the following to configure


    LDFLAGS=-L/usr/lib ./configure --enable-perlinterp=yes --enable-pythoninterp=yes --with-features=huge



~~The solution is to download and compile and install libiconv~~. Download from http://www.gnu.org/software/libiconv/, compile and make & make install

   
    CFLAGS='-arch i386 -arch x86_64' CCFLAGS='-arch i386 -arch x86_64' CXXFLAGS='-arch i386 -arch x86_64' ./configure
    make
    sudo make install


After install, it should be OK to compile vim. But I failed, and spent a whole afternoon. Finally, ~~I removed the src, and use "hg up -C" to recover to get a clean copy, and then succeed! Amazing, maybe there were some "cache" files to record the old libiconv library path.~~ Anyway, I could use vim7.3 in Mac OS X with +python now. :D

**Update:** Just remove all files exclude configure in src/auto, and reconfigure without cache.

**Update:** It works on Lion, too.
