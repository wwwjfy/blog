---
comments: true
date: "2013-08-04 16:24:09 +0800"
layout: post
slug: "basic-server-tuning"
title: "Basic server tuning"
categories:
- Tips
tags:
- server
- tuning
- performance
- gevent
- gunicorn
- postgresql
- redis
---
As a novice for server tuning, I learned something from a recent project.

I know premature optimization is not good, so I only do necessary optimizations
when problems occur.

I went through several steps making the server handle more traffic.

The infrastructure:

- The server is a 8 core, 4GB memory Linode VPS, with Ubuntu 12.04 64-bit
- The service consists of:
    - One PostgreSQL instance
    - One Redis instance
    - One gunicorn server
    - HAProxy as load balancer

As it's a 8-core server, so the gunicorn server gets 5 cores, redis gets 1 core,
and 2 cores left for PostgreSQL and maintenance because PostgreSQL is almost
only for storage.

I was running 4 stress testers, each adding a client every second up to 1000.
Thus, at maximum, there are 4000 clients and about 300 concurrent connections at
the same time.

## Memory Assignment ##

After running several rounds of stress tests, 4G memory is assigned to:

- Redis: 1 GB
- PostgreSQL: 1.8 GB, with 1.3G effective cache and the maximum connection 300
  with work memory 2.8 MB. The number comes from
  [pgtune].
- The rest is for gunicorn server with 5 workers and system usage.

## File descriptor limits ##

The first problem is that by default, the server has the setting of maximum
number of file descriptor of several thousand, which is far fewer than enough,
as each socket is a file descriptor, and because of HAProxy, each connection
actually uses two file descriptors.

In /etc/security/limits.conf, add

    * soft nofile 200000
    * hard nofile 200000

\* means to apply to all users, hard is hard limit, soft limit is for each
session, nofile means max number of open files, 200000 is just a big enough
number so we don't need to consider it later.

## Memory over-limit ##

I turned on both AOF and RDB in Redis, AOF for disaster recovery and RDB for
backup. When there is no enough memory (actually there is, due to copy-on-write,
check out "Background saving is failing with a fork() error under Linux even if
I've a lot of free RAM!" in [redis faq]), saving will cause "Can't save in
background: fork: Cannot allocate memory".

As advised, setting in
/etc/sysctl.conf

    vm.overcommit_memory = 1

This lets system not check memory overcommit.

Anyway, if the memory is indeed insufficient, system will most likely kill
redis, because it uses most resource, at least in my case. (_Keyword_: OOM killer
- out-of-memory killer)

## Network connection timeout ##

When the system load was high, and the clients got refused, this kind of a DoS.

In syslog on server, I saw lots of "nf\_conntrack: table full, dropping packet".
And `netstat` showed lots of `TIME_WAIT`. There are sysctl.conf settings

    net.netfilter.nf_conntrack_max = 65536
    net.ipv4.netfilter.ip_conntrack_generic_timeout = 600
    net.ipv4.netfilter.ip_conntrack_tcp_timeout_established = 432000
    net.ipv4.netfilter.ip_conntrack_tcp_timeout_time_wait = 120

Adding conntrack_max, reducing the timeout:

    net.netfilter.nf_conntrack_max = 131072
    net.ipv4.netfilter.ip_conntrack_generic_timeout = 120
    net.ipv4.netfilter.ip_conntrack_tcp_timeout_established = 54000
    net.netfilter.nf_conntrack_tcp_timeout_time_wait = 30

Run `sysctl -p` to load all the settings and the connection conditions got
better.

Some other settings I'm using from [ApacheBench & HTTPerf]

    fs.file-max = 5000000
    net.core.netdev_max_backlog = 400000
    net.core.optmem_max = 10000000
    net.core.rmem_default = 10000000
    net.core.rmem_max = 10000000
    net.core.somaxconn = 100000
    net.core.wmem_default = 10000000
    net.core.wmem_max = 10000000
    net.ipv4.conf.all.rp_filter = 1
    net.ipv4.conf.default.rp_filter = 1
    net.ipv4.ip_local_port_range = 1024 65535
    net.ipv4.tcp_ecn = 0
    net.ipv4.tcp_max_syn_backlog = 12000
    net.ipv4.tcp_max_tw_buckets = 2000000
    net.ipv4.tcp_mem = 30000000 30000000 30000000
    net.ipv4.tcp_rmem = 30000000 30000000 30000000
    net.ipv4.tcp_sack = 1
    net.ipv4.tcp_syncookies = 0
    net.ipv4.tcp_timestamps = 1
    net.ipv4.tcp_wmem = 30000000 30000000 30000000
    net.ipv4.tcp_tw_reuse = 1

There are also some other points in the article worth reading.

# End

The above are very basic tunings for a production server. There are many stuffs
to do to make a server handle more connections. C10K is no problem due to the
hardware nowadays, while C10M can be interesting.

[pgtune]: http://pgfoundry.org/projects/pgtune
[redis faq]: http://redis.io/topics/faq
[ApacheBench & HTTPerf]: http://gwan.com/en_apachebench_httperf.html
