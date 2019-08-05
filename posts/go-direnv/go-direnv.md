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

A similar tool also available in Go known as [gvm](https://github.com/moovweb/gvm)
with additional features such as managing different locations of `GOPATH`.

https://medium.com/@vCabbage/go-installing-multiple-go-versions-from-source-db5573067c

balance simlicity and usability


To uninstall a downloaded version, just remove its GOROOT directory and the goX.Y.Z binary.


Then I found out that we can go get the previous version of Go!
https://golang.org/doc/install#extra_versions
From the official website
Go 1.8

Explode!

https://godoc.org/golang.org/dl#pkg-subdirectories
parse?


$ go get golang.org/dl/go1.10.8
$ go1.10.8 download


check your go version
$ go1.10.7 version
go version go1.10.7 linux/amd64

direnv settings
what is

Create a new Go project in GOPATH

create main.go

export GOROOT=/Users/nzk190629a/sdk/go1.10.8
PATH_add /Users/nzk190629a/sdk/go1.10.8/bin

compile


https://dave.cheney.net/2017/06/20/how-to-find-out-which-go-version-built-your-binary

