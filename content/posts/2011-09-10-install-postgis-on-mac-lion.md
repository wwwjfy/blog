---
comments: true
date: "2011-09-10"
layout: post
slug: install-postgis-on-lion
title: Install postgis on Lion
categories: ['Tips']
tags: ['geos', 'Lion', 'macport', 'postgis']
---

Updated to Lion recently, I need to install postgis with MacPort.
Postgis depends on geos, which I failed to install with the error

    :info:build Undefined symbols for architecture x86_64:
    :info:build   "__ZNSt8auto_ptrIN4geos4geom8EnvelopeEEcvSt12auto_ptr_refIT_EIS2_EEv", referenced from:
    :info:build       virtual thunk to geos::geom::GeometryCollection::computeEnvelopeInternal() constin GeometryCollection.o
    :info:build   "std::auto_ptr<geos::geom::envelope>::auto_ptr(std::auto_ptr_ref<geos::geom::envelope>)", referenced from:
    :info:build       virtual thunk to geos::geom::GeometryCollection::computeEnvelopeInternal() constin GeometryCollection.o
    :info:build ld: symbol(s) not found for architecture x86_64
    :info:build collect2: ld returned 1 exit status

It's said to have been fixed in [https://trac.macports.org/ticket/30309](https://trac.macports.org/ticket/30309), but still failing at latest macport. And the temporary solution mentioned in the ticket doesn't work either.

    sudo port install geos configure.cc=gcc-4.2 configure.cxx=g++-4.2

I got the imperfect way. That is to install geos by non-macport way.

- Download GEOS framework for Snow Leopard/Lion from [http://www.kyngchaos.com/software:frameworks#geos](http://www.kyngchaos.com/software:frameworks#geos), and install it.

- Install proj

	    sudo port install proj

- Download postgis source code from [http://postgis.refractions.net/download/](http://postgis.refractions.net/download/)

- Unarchive and install with
    
	    ./configure --prefix=/opt/local --with-geosconfig=/Library/Frameworks/GEOS.framework/unix/bin/geos-config --with-projdir=/opt/local
	    make
	    sudo make install