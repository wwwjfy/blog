---
comments: true
date: 2012-01-07 14:27:28+00:00
layout: post
slug: compile-lbn-for-lua-5-1-4
title: Compile lbn for Lua 5.1.4
categories:
- Tips
tags:
- lbn
- lua
---

Lua is powerful. It's so small that some "advanced" features require 3rd party libraries.

I want to operate high precision integer, which Lua doesn't provide.

I got lbn at [http://www.tecgraf.puc-rio.br/~lhf/ftp/lua/](http://www.tecgraf.puc-rio.br/~lhf/ftp/lua/). lbn requires openssl only.

Download and unarchive it.

Edit Makefile to have correct environment vars, LUA, LUAINC, LUALIB, LUABIN. I installed Lua by [homebrew](http://mxcl.github.com/homebrew/), so the path is

    LUA= /usr/local/Cellar/lua/5.1.4
    LUAINC= $(LUA)/include
    LUALIB= $(LUA)/lib
    LUABIN= $(LUA)/bin

If Lua is compiled by self, use

    LUA= /path/to/lua-5.1.4
    LUAINC= $(LUA)/src
    LUALIB= $(LUA)/src
    LUABIN= $(LUA)/src

But, I got the error

    Undefined symbols for architecture x86_64:
    "_luaL_newmetatable", referenced from:
      _luaopen_bn in lbn.o
    "_lua_setfield", referenced from:
      _luaopen_bn in lbn.o
    "_luaL_register", referenced from:
      _luaopen_bn in lbn.o
    …

But the Lua library is already compiled by x86_64

    $ file liblua.5.1.4.dylib 
    liblua.5.1.4.dylib: Mach-O 64-bit dynamically linked shared library x86_64

The problem is the library is not used during compilation. (LUALIB is not used anywhere)

The solution is changing these lines in Makefile

    $T: $(OBJS)
    $(MAKESO) -o $@ $(OBJS) -lcrypto

to

    $T: $(OBJS)
    $(MAKESO) -o $@ $(OBJS) -lcrypto -L$(LUALIB) -llua

And it just works.

_Notice_: It works in Lua 5.1.4 now, and it should work in Lua 5.2, but some effort need to be made. (Lua 5.2 deprecated some API as I see)