---
title: "Squeezing disk space on github runner: Guide"
date: 2025-12-21T12:47:46+02:00
draft: false
description: "Article describes technology as distroless container.
    Why it's useful to use, how and when it make sense"
tags: ["github", "ci", "docker"]
ShowToc: true
TocOpen: true
---

Once time developer send you message in Slack that Github Action pipeline failed.
You opened pipeline, scrolled down logs and see message:
```
ERROR: Could not install packages due to an OSError: [Errno 28] No space left on device
```
But what this means ? Why it happenned and what to do now ? Let's figure it out.<!--more-->

### Github Action Runner

The runner is the application that runs a job from a GitHub Actions workflow.
It is used by GitHub Actions in the hosted virtual environments.
If briefly this is a VM with some specification and image. 
For example here is detail for most popular runer:   
![gitlab_runner](/images/github_runner/most_popular_runner.png)

So it has only 14GB storage size. Does it enough for building docker images, run tests or something else ?
By default yes. But sometimes we need more space. For example if need to build very big image.
In this situation big change you will see "No space left on device"

So what to do when you faced this error ? 

#### Large runner

First solution is create larger runner. 
Gihub allows create large managed runner for your organization.   
![large_runner](/images/github_runner/large_runner.png)

But here exist two cons and limitation:
1. it's more expensive.
2. It's available only in organization.

#### Clean space

Github runners has a lot of preloaded languages, SDK and tools like JDK, NodeJs, Android SDK, 
Google Cloud SDK, Rust toolchain, etc.
This takes up 70% of disk space - almost 50GB of 72GB disk. What if we'll delete unused parts and clean space ? 
Let's clean space a disk. 

First of all check free space on runner.

![free_space](/images/github_runner/free_space.png)

I wondering 23G available when documentation say 14G. 
Idea is check size of folder and delete files using the following piece of ci and start with java
```
  - name: Clean Disk
    run: | 
      du -sh /usr/lib/jvm
      sudo rm -rf /usr/lib/jvm
```
'du' shows jvm folder had size 1.5G so now we have 24.5G free space. 

Let's delete all languages&runtimes and see result: 
 Deleted: 
- Java
- Swift
- Julia
- .Net
- Haskell 
- Rust

After it we have 38G available space on runner.

![free_space2](/images/github_runner/free_space2.png)

But it's not the end. Runner also has SDK and tool not useful for everyone build. 
Add next in deleted list
- Android SDK
- Chromium
- Powershell
- AzureCLI
- Google SDK 
- pipx
- Microsoft libs

In final result is 58G free space on runner. 

![free_space3](/images/github_runner/free_space3.png)

In result here is code help you to delete all these languages, tools and SDK:

{{< details "Code" >}}
  ```yaml
      name: Clean github runner

      on:
        push:
          branches:
            - main

      jobs:
        clean_space:
          name: Clean runner
          runs-on: ubuntu-24.04
          steps:
            - name: Delete languages
              run: | 
                sudo rm -rf /usr/lib/jvm
                sudo rm -rf /usr/share/dotnet
                sudo rm -rf /usr/share/swift
                sudo rm -rf /usr/local/.ghcup
                sudo rm -rf /home/runner/.rustup
                sudo rm -rf /usr/local/julia*
            - name: Delete SDK & tools
              run: | 
                sudo rm -rf /usr/local/lib/android/sdk
                sudo rm -rf /usr/local/share/chromium
                sudo rm -rf /opt/microsoft/msedge
                sudo rm -rf /usr/lib/firefox
                sudo rm -rf /opt/az
                sudo rm -rf /usr/local/share/powershell /opt/microsoft/powershell
                sudo rm -rf /opt/hostedtoolcache
                sudo rm -rf /opt/google
                sudo rm -rf /opt/pipx
                sudo rm -rf /opt/microsoft
                sudo rm -rf /opt/google-cloud-sdk
        
            - name: Show disk usage
              run: |
                df -h
  ```
{{< /details >}}


#### Bonus

For convenience, I created a GitHub Action that cleans the runner. You can check it [here](https://github.com/marketplace/actions/github-actions-cleaner).
