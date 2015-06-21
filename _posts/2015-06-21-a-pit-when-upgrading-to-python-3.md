---
comments: true
date: "2015-06-21 01:16:26 +0800"
layout: post
slug: "a-pit-when-upgrading-to-python-3"
title: "A pit when upgrading to Python 3"
categories:
- tips
tags:
- Python 3
---

To get a better output of unicode, instead of "\uxxx", and also to follow the
fastion, I was upgrading my server to Python 3.

I got weird result from a loop applying multiple filters. Example code:

```python
n = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
ranges = ((3, 8), (6, 10))

for i in ranges:
    n = filter(lambda x: x > i[0] and x < i[1], n)
print(list(n))
```

`ranges` clamps `n`, so I was expecting the result to be `[7]`, but it gave me
`[7, 8, 9]`, which was the result of the second range.

I investigated along, and found the `filter` returns an iterator in Python 3,
instead of a list in Python 2. This means `filter` would be lazy evaluated,
which causes a series of results.

Lazy evaluation means the parameters used in the filter function would be
fetched from the scope when the filter function is executed. In the above
example, `i` would be `(6, 10)` in both loops, as well as `n` be the last
iterator. To demonstrate:

```python
n = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
ranges = ((3, 8), (6, 10))


def f(x, i, n):
    print("I'm apply the filter to {} by {}".format(n, i))
    return x > i[0] and x < i[1]


for i in ranges:
    n = filter(lambda x: f(x, i, n), n)
print(list(n))
```

And I got:

    I'm applying the filter to <filter object at 0x105849048> by (6, 10)
    I'm applying the filter to <filter object at 0x105849048> by (6, 10)
    I'm applying the filter to <filter object at 0x105849048> by (6, 10)
    I'm applying the filter to <filter object at 0x105849048> by (6, 10)
    I'm applying the filter to <filter object at 0x105849048> by (6, 10)
    I'm applying the filter to <filter object at 0x105849048> by (6, 10)
    I'm applying the filter to <filter object at 0x105849048> by (6, 10)
    I'm applying the filter to <filter object at 0x105849048> by (6, 10)
    I'm applying the filter to <filter object at 0x105849048> by (6, 10)
    I'm applying the filter to <filter object at 0x105849048> by (6, 10)
    I'm applying the filter to <filter object at 0x105849048> by (6, 10)
    I'm applying the filter to <filter object at 0x105849048> by (6, 10)
    I'm applying the filter to <filter object at 0x105849048> by (6, 10)

So in fact, `n` was clamped by `(6, 10)` twice.

This "problem" is described well in
[Problems with nested scopes and lazy evaluation](https://www.python.org/~jeremy/weblog/040204.html).
Well, that's another topic.

I might need to follow `2to3` to replace `filter` with list comprehension.

It is those "surprises" that took me hours to debug to find out the reason,
kind of frustrated me. I wish to upgrade, to follow the latest fasion, but to
change is not fit to human's nature, at least not to mine. I'm adapting to
those details, and looking for an alternative. [Rust](http://www.rust-lang.org)
is the new superstar, [Nim](http://nim-lang.org) also looks interesting.
