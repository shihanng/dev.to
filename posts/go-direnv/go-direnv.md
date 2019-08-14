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
$ go1.8.7 version
go version go1.8.7 darwin/amd64
$ go1.8.7 build
```

Uninstalling can be done by removing the `GOROOT` directory:

```sh
$ rm -rf "$(go1.8.7 env GOROOT)"
```

## Using `goX.Y.Z` as `go`

Often in a project we have some kind of build scripts of Makefiles which will
call `go` instead of `go1.8.7` to build our Go binary.
Therefore it would be very convenient if we can use the executable `go1.8.7` as `go`
for a specific directory/project.
This is where [**direnv**](https://github.com/direnv/direnv) comes into play.

> direnv is an environment switcher for the shell. It knows how to hook into
> bash, zsh, tcsh, fish shell and elvish to load or unload environment
> variables depending on the current directory

For demostration purposes, we will create a new project in the `GOPATH` as following:

```sh
$ export GOPATH="$(go1.8.7 env GOPATH)"
$ mkdir -p "${GOPATH}/src/github.com/user/hello"
$ cd "${GOPATH}/src/github.com/user/hello"
$ cat >main.go <<EOL
package main

import "fmt"

func main() {
	fmt.Println("Hello, world.")
}
EOL
```

In order to enable **direnv** in our project directory, we need to setup
the `.envrc` file.
This can be done by doing

```sh
$ direnv edit .
```

inside the root directory of our project.
The content of `.envrc` file should be:

```
export GOROOT="$(go1.8.7 env GOROOT)"
PATH_add "$(go1.8.7 env GOROOT)/bin"
```

In the first line, we point `GOROOT` to the version that we want to work with.
Then we add the Go 1.8.7 compiler into `PATH` so that it will be found before the
default one.
Once the setup is done, running `go version` will give us the older version.

```sh
$ go version
go version go1.8.7 linux/amd64
```

We can then build the `main.go` using

```
$ go build
$ gdb ./hello
...
(gdb) p 'runtime.buildVersion'
$1 = 0x4a5208 "go1.8.7"
```

The above also shows how to check the which version of Go is used to build
the `hello` binary as described in [this post](https://dave.cheney.net/2017/06/20/how-to-find-out-which-go-version-built-your-binary).

---

That's all for the demo. Hope you find this post useful.
Personally I think that this approach has a good balance between simplicity and usability.
One other thing that I often find useful in tools like `pyenv` is the `pyenv install --list` subcommand
which shows the available versions that can be installed with `pyenv`.
Of course the approach above does not have this feature. One hack we can use
to list all available versions on terminal is by using the [`pup`](https://github.com/ericchiang/pup) command
with `curl`:

```
$ curl -s https://godoc.org/golang.org/dl | pup 'body > div.container > table > tbody > tr > td:nth-child(1) > a text{}'

go1.10
go1.10.1
go1.10.2
go1.10.3
go1.10.4
...
```

It's not pretty but it does the trick 💪.

#### Found a typo?

Thank you for reading! Found a typo? See something that could be improved or anything else that should be updated on this blog post? Thanks to [this project](https://github.com/maxime1992/dev.to), you can easily create a pull request on https://github.com/shihanng/dev.to to propose a fix!
