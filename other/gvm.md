---
description: GVM allow you to install and manage multiple Go version on a machine easily.
---

# GVM

`gvm` stands for "Go Version Manager." It is a command-line tool that allows you to manage multiple versions of the Go programming language on your system. Go is known for its simplicity, performance, and efficiency in building scalable and concurrent applications. However, sometimes you might need to work with different versions of Go due to compatibility requirements or to test your code with different language versions.

`gvm` simplifies the process of installing, switching between, and managing multiple Go versions on your system. It provides an easy way to:

1. **Install Different Versions**: You can install various versions of Go side by side.
2. **Switch Between Versions**: `gvm` lets you switch between installed Go versions on demand, making it easy to work with different projects that might require different language versions.
3. **Set Environment Variables**: It automatically adjusts environment variables like `GOROOT` and `PATH` to ensure you're using the correct Go version.
4. **Install Packages**: You can install and manage packages for each version of Go independently.
5. **Update Go**: `gvm` can also help you update your installed Go versions.

Please note that while `gvm` is a convenient tool for managing multiple Go versions, there are other tools and methods available as well, such as manually installing Go versions or using other version managers. The specific choice depends on your preferences and needs.

***

**GVM Installation**

```
sudo apt-get update
sudo apt-get install curl git mercurial make binutils bison gcc build-essential bsdmainutils 
bash < <(curl -s -S -L https://raw.githubusercontent.com/moovweb/gvm/master/binscripts/gvm-installer)
```

**Installing Go & Using Go with GVM**

You can check all Go version available on [https://go.dev/dl/](https://go.dev/dl/)

```
// You can check all Go version available on https://go.dev/dl/
// For example we will install Go version 1.19.12
gvm install go1.19.12

// Using the go1.19.12
gvm use go1.19.12
```

Please note that you can use and switch between all go installed easily, as you install more Go version in one server, it may need more spaces on your disk.
