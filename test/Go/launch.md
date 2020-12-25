---
exclude: true
layout: default
title: LaunchPad
parent: The Go Workshop
nav_order: 1
---

exclude: true
1. TOC
{:toc}

---
name: title
class: middle, center
# Go Launch Pad

---
## Agenda
**You will learn about**
* How to install Go tools
* Write and run Simple Go program
* Core Concepts of Go

---
## Install Go Tools
* Download the platform specific golang install from the following location
> https://golang.org/dl/

* Follow the steps specific to platform
> https://golang.org/doc/install

**Linux Specific Steps**
1. Download the tarball and untar
    ```bash
    wget https://golang.org/dl/go1.15.6.linux-amd64.tar.gz
    tar -C /usr/local -xzf go1.15.6.linux-amd64.tar.gz
    ```
2. Append the path for go folder in `$HOME/.profile`
    ```bash
    export PATH=$PATH:/usr/local/go/bin
    ```
3. Verify if go runs
    ```bash
    go version
    ```
4. Set your worksapce folder in Gopath by adding the line to `$HOME/.profile`
    ```bash
     export GOPATH=$HOME/workspace
    ```

---
## Sample Program
* Create a folder in $HOME/workspace
    ```bash
    cd $HOME/workspace
    mkdir hello
    cd hello
    ```

* Write the following sample code in `hello/hello.go`
    ```go
    package main
    
    import "fmt"
    
    func main() {
        fmt.Println("Hello, World!")
    }
    ```

* Build & Run
    ```bash
    $ go build
    $ ./hello
    Hello, World!
    ```
