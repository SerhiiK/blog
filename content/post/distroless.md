---
title: "Distroless images: why, how and when"
date: 2023-11-14T21:47:46+02:00
draft: false
description: "Article describes technology as distroless container.
    Why it's useful to use, how and when it make sense"
tags: ["container", "docker", "distroless", "security"]
ShowToc: true
TocOpen: true
---

Containers - technology that changed the way how develop, deploy, and manage applications.<!--more-->
Traditional container images often include a complete OS with a lot of useless tools and libraries
exposes vulnerabilities and increasing the size of an image. 
"Distroless" images are an approach to building images and solving these disadvantages.

### Distroless
"Distroless" images contain only your application and runtime dependencies, that's all.  
1.Images cut down unnecessary parts, They reduce the attack surface and make it harder 
to find weak spots in your application, 
making you container more secure. You don't have any libs or tools like cURL that 
may has some vulnerabilities.   
2.  Because of image doesn't have nonimportant libs, tool they are smaller, which means they 
taky up less space, save storage in registries, and can start up faster. 

More details about distroless images you may find in [repo](https://github.com/GoogleContainerTools/distroless).

### Practice
Let's take a look on example:

As app writes webserver on Go:

{{< details "Code" >}}
  ```go
package main

import (
	"fmt"
	"log"
	"net/http"
)

func main() {
	http.HandleFunc("/", func(w http.ResponseWriter, r *http.Request) {
		fmt.Fprintf(w, "Hello, Distrolles")
	})

	log.Println("Starting server on :8080...")
	err := http.ListenAndServe(":8080", nil)
	if err != nil {j
		log.Fatal("Error starting server: ", err)
	}
}
  ```
{{< /details >}}

Create Dockerfile for building image:
Golang image based on Debian 12.1 Bookworm

{{< details "Dockerfile" >}}
  ```
FROM golang:1.20.11-bookworm

WORKDIR /app

COPY . /app

RUN go build -o main .

EXPOSE 8080

CMD ["./main"]
  ```
{{< /details >}}

As result, we have image with size 914MB. Information from
dockerhub says it has 302 packages.
Let's try to scan image on vulnerabilities. For it I'm using trivy:  
```Total: 333 (UNKNOWN: 1, LOW: 241, MEDIUM: 56, HIGH: 33, CRITICAL: 2)```

Use only one image now it's not good idea so let's try to build container 
with [multi-stage build](https://docs.docker.com/build/building/multi-stage/).  

{{< details "Dockerfile" >}}
  ```
FROM golang:1.20.11-bookworm AS builder

WORKDIR /app

COPY . /app

RUN go build -o main .

FROM alpine:3.18.4

WORKDIR /app

COPY --from=builder /app/main /app/main

EXPOSE 8080

CMD ["./main"]j
  ```
{{< /details >}}

Now image has size only 14MB but what about vulnerabilities ?
Trivy's result:  
```Total: 4 (UNKNOWN: 0, LOW: 0, MEDIUM: 0, HIGH: 4, CRITICAL: 0)```

It's good result, but all vulnerabilities are from ssl packages and have high level. 
Finally, we can try to use a distroless image.

{{< details "Dockerfile" >}}
  ```
FROM golang:1.20.11-bookworm AS builder

WORKDIR /app

COPY . /app

RUN CGO_ENABLED=0 go build  -o main .

FROM gcr.io/distroless/static-debian12

WORKDIR /app

COPY --from=builder /app/main /app/main

EXPOSE 8080

CMD ["./main"]
  ```
{{< /details >}}

This image has only 8,6MB. Trivy has good results:  
```Total: 0 (UNKNOWN: 0, LOW: 0, MEDIUM: 0, HIGH: 0, CRITICAL: 0)```

### Summary 
Distroless images is a good choise whe you need minimalistic and contain only 
necessary tools and libs for your application. Thanks to this, they have
reduced attack surface. They don't have issue that exist in Alpine images.
For better understanding here is comparing of images size:

![image_size](/img/distroless/size.png)

and vulnerabilities count:

![vuln_img](/img/distroless/vuln.png)

Distroless images is not for you if: 

1. Compatibility - ensure that your application has all dependencies to work
  inside. 
2. Debugging - distroless images don't have even a shell. Of course, you may 
   use the debug version of an image for troubleshooting but it's not so comfortable.
3. Customization - offers a list of images for popular languages like Python,
   java, etc. If you need to support some specific programming language distroless
   is not for you. 
