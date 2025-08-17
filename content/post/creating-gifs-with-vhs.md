---
title: "Creating gifs with VHS"
date: 2023-04-02T21:03:20+03:00
draft: false
description: "How to do gifs with VHS"
tags: ["gif", "terminal", "bash", "zsh"]
ShowToc: true
TocOpen: true
---
Every engineer sometimes need to write documentation. Very often this documentation is needed for teammate,
who doesn't have a lot of knowledge of this technology or even doesn't need it if this documentation for support team.<!--more-->
That's why documentation should be as understandable as possible.
One of the ways to do it - using of records of terminal with details how to do one of another action.
Exist several tools ho to do it. One of the most popular is asciinema. 
But his tool has two cons:

 1. You should use asciinema for playing the record. It's not comfortable to use in documentation.
 2. You should insert all record manually. And if you will do typo you should record it again.

 ### VHS
[VHS](https://github.com/charmbracelet/vhs) - tool that solves these issues. This tool uses file with commands and configuration of the tapes. 
Also vhs can create record in output as `gif`, `mp4`, `webm`.  

So lets call command in terminal and open tape file.

Execute  `vhs new demo ` and open generated file in your favorite editor  
![demo](/images/vhs/demo.gif)

`demo.tape` is a configuration file that consist of a series of commands - instructions for VHS to perform them in virtual terminal.
Here we need to change output path because examples folder doesn't exist and change echo command.  
![gif](/images/vhs/gif.gif)

Now we have tape ready to record. Execute command `vhs < demo.tape` and waiting until tool generate gif.   

![generate](/images/vhs/generate.gif)
VHS will record steps an use ffmpeg for creating `demo.gif` file.

### Result
Here's result:  
![result](/images/vhs/demo_1.gif)


