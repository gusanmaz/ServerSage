# Go Configuration Guide

This guide covers the process of installing, configuring, and using Go (Golang) on your VPS.

## Table of Contents

1. [Installing Go](#installing-go)
2. [Configuring Go Environment](#configuring-go-environment)
3. [Managing Go Versions with GVM](#managing-go-versions-with-gvm)
4. [Building and Running Go Programs](#building-and-running-go-programs)

## Installing Go

1. First, download the latest version of Go. Check the official Go website for the most recent version.

   ```bash
   wget https://golang.org/dl/go1.17.linux-amd64.tar.gz
   ```

2. Extract the archive to `/usr/local`:

   ```bash
   sudo tar -C /usr/local -xzf go1.17.linux-amd64.tar.gz
   ```

3. Add Go to your PATH by adding these lines to your `~/.profile` or `~/.bashrc`:

   ```bash
   export PATH=$PATH:/usr/local/go/bin
   export GOPATH=$HOME/go
   export PATH=$PATH:$GOPATH/bin
   ```

4. Apply the changes:

   ```bash
   source ~/.profile
   ```

5. Verify the installation:

   ```bash
   go version
   ```

## Configuring Go Environment

1. Create your Go workspace:

   ```bash
   mkdir -p $HOME/go/{bin,src,pkg}
   ```

2. Set GOPATH and add Go binaries to PATH (if not done in the installation step):

   ```bash
   export GOPATH=$HOME/go
   export PATH=$PATH:$GOPATH/bin
   ```

3. You can add these lines to your `~/.bashrc` to make them permanent.

## Managing Go Versions with GVM

GVM (Go Version Manager) allows you to manage multiple Go versions.

1. Install GVM:

   ```bash
   bash < <(curl -s -S -L https://raw.githubusercontent.com/moovweb/gvm/master/binscripts/gvm-installer)
   ```

2. Restart your terminal or source your profile:

   ```bash
   source ~/.bashrc
   ```

3. Install a Go version:

   ```bash
   gvm install go1.17
   ```

4. Use a specific Go version:

   ```bash
   gvm use go1.17
   ```

5. Set a default Go version:

   ```bash
   gvm use go1.17 --default
   ```

## Building and Running Go Programs

1. Create a new Go file:

   ```bash
   nano hello.go
   ```

2. Add the following content:

   ```go
   package main

   import "fmt"

   func main() {
       fmt.Println("Hello, World!")
   }
   ```

3. Build the program:

   ```bash
   go build hello.go
   ```

4. Run the compiled program:

   ```bash
   ./hello
   ```

5. Alternatively, you can build and run in one step:

   ```bash
   go run hello.go
   ```

Remember to set up your `GOPATH` correctly and organize your Go projects within the `src` directory of your `GOPATH`.

By following this guide, you should have a fully functional Go development environment on your VPS. You can now start building and deploying Go applications with ease.