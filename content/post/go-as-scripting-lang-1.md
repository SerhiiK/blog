---
title: "Does Go good for scripting? Part 1"
date: 2022-02-13T17:01:00+02:00
description: "Golang is more and more popular language and some company uses it for
              scripting. But does it ready for it? Let's check"
tags: ["scripting", "golang", "provisioning"]
ShowToc: true
TocOpen: true
---

Nowadays Golang is so popular language. In Feb 2022 it has 11 place on the TIOBE index.
Also, a lot of tools and apps are written or rewritten in this lang. Some company like 
Cloudflare even use it for scripting.
<!--more-->
Inspired by this article I would like to check does Go is
comfortable for scripting and does it good alternative to Python? 

## Start

For testing, I'll use a famous example😁

```golang
package main

import "fmt"

func main(){
    fmt.Println("Hello, World") 
}
```
Saved it with the name *goscript.go*

That's all. Simple command `go run goscript.go` will run this script and you will see
a greeting. But it seems very simple, doesn't it? And use `go run` command instead run
script by `/.goscript.go` like BASH or Python is not comfortable. So let's find another
approach

## Shebang
Shebang - is the first line of script that has information how to run it. 
Some examples of it:  
`#!/bin/bash`        - Execute the file using the Bash shell  
`#!/bin/env python3` - Execute the file using Python interpreter  

### Go shebang
Let's try to use the same for Go. 
`#!/bin/env go run`   

Don't forget to do file executable `chmod +x goscript.go`   
Trying to run it:  

```
$ ./goscript.go
Failed to execute process './goscript.go'. Reason:
The file './goscript.go' specified the interpreter '/bin/env go run',
which is not an executable command.
```
It happens because shebang can have only one argument but we need two for a running script.  

### Workaround 1

Of course, I'm not the first man who faced this issue. Internet searching led me to this topic
on [StackOverflow](https://stackoverflow.com/questions/7707178/whats-the-appropriate-go-shebang-line)
In this topic user recommends to use `//usr/bin go run $0 $@ ; exit` like shebang. 

I know on my computer Go installed in another place. Find this place you may by using the command 
`whereis go`

My result:   
```
$ whereis go
go: /usr/local/go
```
I changed the path for go and try to run it
```
$./goscript.go
./goscript.go: 1: ./goscript.go: //usr/local/go: Permission denied
```

Hmm, it's strange that there is a problem with permission. Let's try to find a solution for it. 

### Workaround Improvement

In the same topic, little bit below user suggests to use this command:   
`///usr/bin/true; exec /usr/bin/env go run "$0" "$@"`   

Try one more time: 
```
$ ./goscript.go
./goscript.go: 1: ./goscript.go: ///usr/bin/true: not found
Hello, World
```
Finally, a solution to run the script was found. But I don't like `not found` issue. 

## Gorun

[Gorun](https://github.com/erning/gorun) is a tool enabling one to put a "bang line" in the source code of 
a Go program to run it, or to run such a source code file explicitly.
In my case, I downloaded binary in `/usr/local/bin`. 
Changed shebang on `#!/usr/local/bin/gorun` and tried one more time:  
```
$ ./goscript.go
Hello, World
```

It's working. 

## Summary

Go has a solution to run a script without any problem: command `go run`. If you want to use
shebang better to use *gorun* for it. In other cases it's a workaround with duct tape and 
I will not recommend using it. 

In the next part, we will create a simple and check how comfortable using Go is for scripting.
