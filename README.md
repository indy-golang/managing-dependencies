###Context

Go is not always easy to obtain and, furthermore, not easy to maintain.

Go code must be written in a workspace.

* src contains Go source files organized into packages (one package per directory),
* pkg contains package objects, and
* bin contains executable commands.

Meaning that your code is foced to live alongside your dependencies. go build pulls dependencies from your workspace to build your code without explicit linking.

###Installation

1. apt-get install golang

``` bash
$ docker run -it debian:jessie /bin/bash
$ apt-get update
$ apt-get install golang
$ go version
go1.3.3
```

Upsides: easy. Downsides: older version of Go. If we install a package, everything gets installed to our `GOPATH`, like so:

``` bash
$ go get github.com/go-martini/martini
$ ls ~/go
```

 If you have a few Go projects and they all depend on different versions of these packages, you're out of luck. More on that later.

2. From source

``` bash
$ docker run -it debian:jessie /bin/bash
$ apt-get update
$ apt-get install git build-essential
$ git clone https://github.com/golang/go.git
$ git checkout release-branch.go1.4
$ ./src/all.bash
$ echo "PATH=./bin/go:$PATH" >> ~/.bashrc
$ go version
go1.4.1
```

Building Go 1.5 will require installing Go 1.4.

3. gvm install go1.4

``` bash
$ docker run -it debian:jessie /bin/bash
$ apt-get update
$ apt-get install curl git mercurial make binutils bison gcc build-essential
$ bash < <(curl -s -S -L https://raw.githubusercontent.com/moovweb/gvm/master/binscripts/gvm-installer)
$ source /root/.gvm/scripts/gvm
$ gvm install go1.4
$ gvm use go1.4
$ go version
go1.4
```

More interesting here is when you run `which go`. As you can see, we're running Go out of a directory in our home folder suited for housing many different versions of Go, similar to `rvm`. `gvm` provides me with the concept of pkgsets. By default, all packages are installed to the global pkgset, like this:

``` bash
$ go get github.com/go-martini/martini
$ ls ~/.gvm/pkgsets/go1.4/global/
```

This is the same functinoality as the default `go` install, except the versions are separated. BUT. Now we can set our pkgset per project.

``` bash
$ gvm pkgset create indy-golang
$ gvm pkgset use indy-golang
$ go get github.com/go-martini/martini
$ ls ~/.gvm/pkgsets/go1.4/indy-golang/
```

There are still dumb issues with this as far as `go` being able to find your package in the path. You can put your project src inside the global pkgset directory, or you can make a symbolic link from the pkgset directory to your project folder. There are other solutions that will allow you to put your project wherever you want.

4. docker's golang image

This is certainly the easiest way to get a go environment up and running.

``` bash
$ docker run -it golang:1.4 /bin/bash
$ go version
go1.4
```

5. gvp and gpm

This is the realm of only package dependencies.

``` bash
$ docker run -it golang:1.4 /bin/bash
$ apt-get update
$ apt-get install git build-essential golang
$ git clone https://github.com/pote/gvp.git && cd gvp
$ ./configure
$ make install
$ git clone https://github.com/pote/gpm.git && cd gpm
$ ./configure
$ make install
```

Now usage:

``` bash
$ mkdir test
$ cd test
$ echo $GOPATH
$ source gvp
$ echo $GOPATH
$ go get github.com/go-martini/martini
$ rm -rf .godeps
$ echo "github.com/go-martini/martini v1.0" > Godeps
$ gpm install
```

Now everything is installed to a local project directory. `gpm` also lets me specify package versions, so now I can keep all my dependencies in line.