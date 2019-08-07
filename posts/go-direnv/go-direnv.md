---
title: Managing Go Versions with direnv
published: false
description: 'How to simply manage Go versions for multiple projects with direnv'
tags: Go, shell, direnv
---

Recently I've joined a project that requires previous version of Go.
Since I have the latest version already installed on my machine
I was looking for a way to manage different versions of Go in a single machine.
In Python world, there is a very well-known version management called
[pyenv](https://github.com/pyenv/pyenv) which is highly inspired
by the Ruby's one: [rbenv](https://github.com/rbenv/rbenv).

As for Go, it seems that there is no authoritative way to manage the versions of Go.
Probably it is not a big issue as many would just upgrade to the latest version of the language
since it essentially [promises that new minor version of 1.X would not break the older ones](https://golang.org/doc/go1compat).
Yet, through a very brief Googling, I found that there are three main approaches
for this problem:

1. Installing each version of Go from source. This [article by Kale Blankenship](https://medium.com/@vCabbage/go-installing-multiple-go-versions-from-source-db5573067c)
   describe the steps in details.
2. There is also a tool that is similar to **pyenv** and **rbenv** available in
   the Go ecosystem known as [gvm](https://github.com/moovweb/gvm)
   It also comes with additional features such as managing different locations of `GOPATH`.
3. Lastly, we can also [use `go get`](https://golang.org/doc/install#extra_versions). Yes! GO GET!

## Installing a Different Version of Go

In this post, we will explore how to use `go get` to manage different versions Go.
One small caveat for this approach is that not all Go versions are support.
Supported versions are listed on [the download page](https://godoc.org/golang.org/dl).
Let's say that we need `go1.8.7`, all we have to do is:

```sh
$ go get golang.org/dl/go1.8.7
$ go1.8.7 download
```

Once the download is complete, we can compile our source code with `v1.8.7` using
```sh
$ go1.8.7 build
```


To uninstall a downloaded version, just remove its GOROOT directory and the goX.Y.Z binary.



direnv settings
what is

Create a new Go project in GOPATH

create main.go

export GOROOT=/Users/nzk190629a/sdk/go1.10.8
PATH_add /Users/nzk190629a/sdk/go1.10.8/bin

compile

https://github.com/ericchiang/pup

```
curl -s https://godoc.org/golang.org/dl | pup 'body > div.container > table > tbody > tr > td:nth-child(1) > a text{}'
```


https://dave.cheney.net/2017/06/20/how-to-find-out-which-go-version-built-your-binary


balance simlicity and usability
