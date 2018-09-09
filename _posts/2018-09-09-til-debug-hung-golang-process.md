---
date: "2018-09-09 19:29:22 +0800"
layout: post
slug: "til-debug-hung-golang-process"
title: "TIL: Debug hung Golang process"
categories:
- Tips
tags:
- Golang
---

I've been using Golang for server-side development for a while, and started to
get familiar with Golang pitfalls. I like its builtin concurrency feature
goroutine, and single binary which makes it very easy to deploy, but also find
it irritating to check `if err != nil` for every function, and some parculiar
"features".
[A recent post](https://about.sourcegraph.com/go/gophercon-2018-go-says-wat/)
is an representative.

Today, I learned one other good thing about it. Golang has an official
profiling tool. This helped me locate a race condition in my code.

In my application, I started lots of goroutines to do repetitive tasks
infinitely. But after a while, the CPU usage would drop to almost zero. I had
no clues, so I decided to check what each goroutine was doing at that time.

- Add a webserver for queries.

```go
import (
	"net/http"
	_ "net/http/pprof"
)

func main() {
	go http.ListenAndServe(":8080", http.DefaultServeMux)
}
```

- Start the process, and wait until it hangs.

- I can run `go tool pprof http://localhost:8080/debug/pprof/goroutine` to get
  samples, but in this case, I want to get all the goroutines.

```bash
wget -O all_goroutines http://localhost:8080/debug/pprof/heap?debug=2
```

- Thanks to
  [goroutine-inspect](https://github.com/linuxerwang/goroutine-inspect), the
  dumped goroutine tracebacks can be analyzed easily.

```
$ go get github.com/linuxerwang/goroutine-inspect
$ goroutine-insepct
>> all = load("all_goroutines")
>> all.dedup()
>> all.save("dedupped")
```

- After de-duplication, a few different kinds of goroutines are presented,
  instead of tons of thousands of them.

- Going through them, I found the race condition.

#### Conclusion

It's very convenient that a language shipped with its official profiling tool,
and this tool is quite powerful. There are also other things can be profiled,
like heap, cpu usage, etc.

#### References

- [Profiling Go Programs](https://blog.golang.org/profiling-go-programs)
- [Profiling Go programs with pprof](https://jvns.ca/blog/2017/09/24/profiling-go-with-pprof/)
