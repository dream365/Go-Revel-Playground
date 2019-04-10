Getting Started
===================

Install Go
-------------

Before you can use Revel, first You need to install Go
- [Ubuntu](https://github.com/golang/go/wiki/Ubuntu)
- [Windows](https://golang.org/doc/install#windows)

##### Go 1.8+ is required.

Set Up Your GOPATH
-------------

If you have not created a GOPATH as part of the installation, do so now.

The **GOPATH** is a directory where all of your Go code will live.

 Here is one way of setting it up:
 
 1. Make a directory: ```mkdir ~/gocode```
 2. Tell Go to use that as your GOPATH: ```export GOPATH=~/gocode```
 3. Save your GOPATH so that it will apply to all future shell sessions: ```echo export GOPATH=$GOPATH >> ~/.bash_profile```

Now your Go installation is complete.

Install Git and HG
-------------

Git and Mercurial are required to allow ```go get``` to clone various dependencies.
- [Installing Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)
- [Installing Mercurial](https://www.mercurial-scm.org/downloads)

Get The Revel Framework
-------------

To get the Revel framework, run

```go get github.com/revel/revel```

This command does a couple of things:
- Go uses git to clone the repository into **$GOPATH/src/github.com/revel/revel/**
- Go transitively finds all of the dependencies and runs ```go get``` on them as well.

Get And Build The Revel Command Line Tool
-------------

The revel command line tool is used to build, run, and package Revel applications.

Use go get to install:

```go get github.com/revel/cmd/revel```

Ensure the **$GOPATH/bin** directory is in your PATH so that you can reference the command from anywhere.

```export PATH="$PATH:$GOPATH/bin"```

Verify that it works:
```
  $ Usage:
       revel [OPTIONS] <command>
     
     Application Options:
       -v, --debug              If set the logger is set to verbose
           --historic-run-mode  If set the runmode is passed a string not json
       -X, --build-flags=       These flags will be used when building the application. May be specified multiple times, only applicable for Build, Run, Package, Test commands
     
     Available commands:
       build
       clean
       new
       package
       run
       test
       version
```

***
#### [Home](https://dream365.github.io/Go-Revel-Playground)