---
title: "Docker basics"
date: 2020-11-10T16:19:44+01:00
draft: false
ShowToc: true
TocOpen: true
---

# 01. Intro & Docker basics

### Why you should learn Docker and Kubernetes?

Docker and Kubernetes is most trending and loved platform in DevOps world, I collected some metrics from StackOverflow, you can see as below:

![StackOverflowstats](/img/stackoverflow_stats.png)

This is not only the reason you should learn Kubernetes, I think this will give you same fun and interest that you have with learning Linux.

So join me learning Kubernetes.

---

Before moving forward lets check history, how it started and what was the main focused.

## **Going back in time**

Let's take a look at why Kubernetes is so useful by going back in time.

![Container evolution](/img/container_evolution.png)

### **Traditional deployment era:**

Early on, organizations ran applications on physical servers. There was no way to define resource boundaries for applications in a physical server, and this caused resource allocation issues. For example, if multiple applications run on a physical server, there can be instances where one application would take up most of the resources, and as a result, the other applications would underperform. A solution for this would be to run each application on a different physical server. But this did not scale as resources were underutilized, and it was expensive for organizations to maintain many physical servers.

### **Virtualized deployment era:**

As a solution, virtualization was introduced. It allows you to run multiple Virtual Machines (VMs) on a single physical server's CPU. Virtualization allows applications to be isolated between VMs and provides a level of security as the information of one application cannot be freely accessed by another application.

Virtualization allows better utilization of resources in a physical server and allows better scalability because an application can be added or updated easily, reduces hardware costs, and much more. With virtualization you can present a set of physical resources as a cluster of disposable virtual machines.

Each VM is a full machine running all the components, including its own operating system, on top of the virtualized hardware.

### **Container deployment era:**

Containers are similar to VMs, but they have relaxed isolation properties to share the Operating System (OS) among the applications. Therefore, containers are considered lightweight. Similar to a VM, a container has its own filesystem, share of CPU, memory, process space, and more. As they are decoupled from the underlying infrastructure, they are portable across clouds and OS distributions.

Containers have become popular because they provide extra benefits, such as:

- Agile application creation and deployment: increased ease and efficiency of container image creation compared to VM image use.
- Continuous development, integration, and deployment: provides for reliable and frequent container image build and deployment with quick and easy rollbacks (due to image immutability).
- Dev and Ops separation of concerns: create application container images at build/release time rather than deployment time, thereby decoupling applications from infrastructure.
- Observability not only surfaces OS-level information and metrics, but also application health and other signals.
- Environmental consistency across development, testing, and production: Runs the same on a laptop as it does in the cloud.
- Cloud and OS distribution portability: Runs on Ubuntu, RHEL, CoreOS, on-premises, on major public clouds, and anywhere else.
- Application-centric management: Raises the level of abstraction from running an OS on virtual hardware to running an application on an OS using logical resources.
- Loosely coupled, distributed, elastic, liberated micro-services: applications are broken into smaller, independent pieces and can be deployed and managed dynamically – not a monolithic stack running on one big single-purpose machine.
- Resource isolation: predictable application performance.
- Resource utilization: high efficiency and density.

---

## Container History

![History](/img/containers_history.png)

- 1980
    - Chroot
- 1990
    - Jail - FreeBSD
    - sysadmin use to setup chroot-nginx, chroot bind
- 2007
    - cgroups
        - it's a Linux kernel feature that limits, accounts for, and isolates the resource usage (CPU, memory, disk I/O, network, etc.)
    - namespace
        - A Linux namespace is a Linux kernel feature that isolates and virtualizes system resources.
- 2008
    - LXC (Linux containers)
    - based cgroup and namespace
    - LXD - just for better user experience
- 2013
    - Docker
    - On top of LXC - until Docker v0.8
    - Docker build own in v0.9 libcontainer
- 2014
    - AWS ECS, Openshift
- 2014
    - Kubernetes - (Borg 5 years)
    - Maintained by CNF (Cloud native foundation)
- 2016
    - containerd - runc - container
    - OCI (Open container Initiative) - defines rules and format and standard

---

So you just heard Cloud native term and its important to understand that.

## What is Cloud native?

cloud native application is one that is built to deal with modern day demands, modern business demands, ability to scale, the ability to self‑heal, and react to market conditions, but also, it tends to be written in a very modular way.

just taking an existing application and running it on a cloud compute instance doesn’t make it cloud native. Neither is it just about running in a container, or using cloud services such as Azure’s Cosmos DB or Google’s Pub/Sub, although those may well be important aspects of a cloud native application.

What are the cloud native attributes?

### 1. Automatable

- able to be done by machines without human action. that means app developer don't even need to worry about them. it should have common standard, format and interface and kubernetes provides that

### 2. Ubiquitous and Ubiquitous

Because they are decoupled from physical resources such as disks, or any specific knowledge about the compute node they happen to be running on, containerized microservices can easily be moved from one node to another, or even one cluster to another

### 3. Resilient and scalable

Traditional applications tend to have single points of failure: the application stops working if its main process crashes, or if the underlying machine has a hardware failure, or if a network resource becomes congested. Cloud native applications, because they are inherently distributed, can be made highly available through redundancy and graceful degradation.

### 4. Dynamic

A container orchestrator such as Kubernetes can schedule containers to take maximum advantage of available resources. It can run many copies of them to achieve high availability, and perform rolling updates to smoothly upgrade serv‐ ices without ever dropping traffic

### 5. Observable

Cloud native apps, by their nature, are harder to inspect and debug. So a key requirement of distributed systems is observability: monitoring, logging, tracing, and metrics all help engineers understand what their systems are doing (and what they’re doing wrong).

### 6. Distributed

Cloud native is an approach to building and running applications that takes advantage of the distributed and decentralized nature of the cloud. It’s about how your application works, not where it runs. Instead of deploying your code as a single entity (known as a monolith), cloud native applications tend to be com‐ posed of multiple, cooperating, distributed microservices. A microservice is sim‐ ply a self-contained service that does one thing. If you put enough microservices together, you get an application.

---

# Virtual Machine vs Container

![vm_vs_container](/img/vm_vs_container.png)

| VM                               | Container                                 |
| ---------------------------------|-------------------------------------------|
| Heavyweight                      | Lightweight                               |
| Hardware level virtualization    | OS level virtualization                   |
| Each runs in its own OS          | All shares host kernel                    |
| Require more memory and space    | Require less memory and space             |
| Startup time in minutes          | Startup time in milliseconds              |
| Limited performance              | Native performance                        |
| Process-level isolation, possibly less secure | Fully isolated and hence more secure |

---

# Getting started with Docker

We are going to mostly focused on Kubernetes but before moving forward, we will learn basics docker.

### What is Docker?

> Docker is a tool designed to make it easier to create, deploy, and run applications by using containers. Containers allow a developer to package up an application with all of the parts it needs, such as libraries and other dependencies, and ship it all out as one package. — [OpenSource.com](http://opensource.com/)

### Why Docker?

> “It works on my machine, I don’t know why it won’t work on yours.” — any developer, ever

![ship docker](/img/ship-docker.png)

1. **Isolation**

It helps us create an environment agnostic system. Your application runs smoothly on different platforms. This is essentially achieved using containers.

***2.* Portability**

Since all of your dependencies are in the same container, it’s easy to carry from one place to another giving Docker its portability.

***3.* Lightweight**

Runs as another application on your system instead of consuming whole lot resources of your system.

***4.* Robustness**

Less demanding in terms of hardware and needs very little memory as compared to VMs, hence providing efficient isolation levels which help save not only the cost but also time.

## Docker Architecture

![Docker Arch](/img/docker-arch.png)

We are going to focus on Docker Engine, which is open source containerization technology for building and containerizing applications.

### **The Docker daemon**

The Docker daemon (`dockerd`) listens for Docker API requests and manages Docker objects such as images, containers, networks, and volumes. A daemon can also communicate with other daemons to manage Docker services.

### **The Docker client**

The Docker client (`docker`) is the primary way that many Docker users interact with Docker. When you use commands such as `docker run`, the client sends these commands to `dockerd`, which carries them out. The `docker` command uses the Docker API. The Docker client can communicate with more than one daemon.

### **Docker registries**

A Docker *registry* stores Docker images. Docker Hub is a public registry that anyone can use, and Docker is configured to look for images on Docker Hub by default. You can even run your own private registry.

When you use the `docker pull` or `docker run` commands, the required images are pulled from your configured registry. When you use the `docker push` command, your image is pushed to your configured registry.

### **Docker objects**

When you use Docker, you are creating and using images, containers, networks, volumes, plugins, and other objects. This section is a brief overview of some of those objects.

### Image

A Docker image is a read-only template that contains a set of instructions for creating a container that can run on the Docker platform. It provides a convenient way to package up applications and preconfigured server environments, which you can use for your own private use or share publicly with other Docker users.

### CONTAINERS

A container is a runnable instance of an image. You can create, start, stop, move, or delete a container using the Docker API or CLI

# Docker Practical

### Requirements:

- Option 1# Install Docker On you machine if you have access. You can install it on Windows, Linux or Mac using same process you use for any other application.
- Option 2# If you don't have access then you can use public docker, check following link: [https://labs.play-with-docker.com/](https://labs.play-with-docker.com/)

Test that your installation works by running the hello-world Docker image

```bash
docker run hello-world
```

Command breakdown:

- It will check if image `hello-world:latest` exists locally or not
- and if not then it will download from docker hub registry
- then it will run CMD which will print message on terminal

### Run docker with interactive mode

```bash
docker run -it alpine sh
```

- You will shell from container  and on exit you will be out from container and container will stop

### Container you launch, it will be still there that you can check using

```bash
docker ps -a
```

### Start docker in deattach mode

```bash
docker run -dt alpine sh
```

### Container will be running until you stop/kill it using

```bash
docker stop container-id
```

### List docker images

```bash
docker images
```

---

## Building custom image

![Build docker image](/img/build-image.png)

As you can in above diagram, you have `Dockerfile`, its set of instruction to create docker image, think like Makefile, then once you build image, you can create containers from it.

### **Dockerfile Commands**

Below, are the commands that will be used 90% of the time when you’re writing `Dockerfiles`, and what they mean.

- `FROM` — this initializes a new build stage and sets the *[Base Image](https://docs.docker.com/engine/reference/glossary/#base-image)* for subsequent instructions. As such, a valid `Dockerfile` must start with a `FROM` instruction.
- `COPY` — To copy over files or directories from a specific location.
- `ADD` — As COPY, but also able to handle remote URLs and unpack compressed files.
- `RUN` — will execute any commands in a new layer on top of the current image and commit the results. The resulting committed image will be used for the next step in the `Dockerfile`.
- `ENV` — sets the environment variable `<key>` to the value `<value>`. This value will be in the environment for all subsequent instructions in the build stage and can be [replaced inline](https://docs.docker.com/engine/reference/builder/#environment-replacement) in many as well.
- `EXPOSE` — informs Docker that the container listens on the specified network ports at runtime. You can specify whether the port listens on TCP or UDP, and the default is TCP if the protocol is not specified. This makes it possible for the host and the outside world to access the isolated Docker Container
- `VOLUME` — creates a mount point with the specified name and marks it as holding externally mounted volumes from the native host or other containers.
- `ENTRYPOINT` — Command that will always be executed when the container starts. If not specified, the default is /bin/sh -c
- `CMD` — Arguments passed to the entrypoint. If `ENTRYPOINT` is not set (defaults to /bin/sh -c), the CMD will be the commands the container executes.
- `WORKDIR` — To set the working directory for any commands that follow in the Dockerfile.

---

So our use case is to build web server based on nginx and expose port 8080 and it should be accessible from browser

First thing you should do is to use DRY principle, mean DO NOT REPEAT YOURSELF, so go on docker hub or github to find official nginx image, so you don't have to compile nginx your self however there are cases there you will need to compile it if there is other requirement.  but in following example to learn docker build, we will not use official image, instead we will use small alpine image to setup nginx

Create `Dockerfile` with following content

```docker
FROM  alpine:3.8

# packages & configure
RUN adduser -D -g 'nginx' nginx  \
    && apk add --no-cache nginx \
    && mkdir -p /www \
        /etc/nginx/conf.d \
        /etc/nginx/hosts.d \
        /etc/nginx/keys \
        /var/lib/nginx \
        /run/nginx/ \
    && chown -R nginx:nginx /var/lib/nginx /www

# external
EXPOSE 8080

CMD ["nginx", "-g", "daemon off;"]
```

Run following command to build web image

```bash
docker build -t web .
```

You can run `docker images` to list image

Let's add require files in current directory i.e. `index.html` and `default.conf`

Simple index page `index.html`

```html
<html>
	<body>
	<h1>Welcome to nginx</h1>
	</body>
</html>
```

nginx default conf `default.conf`

```
server {
    listen       8080;
    server_name  localhost;

    location / {
        root   /www;
        index  index.html index.htm;
    }

    # redirect server error pages to the static page /50x.html
    #
    error_page   500 502 503 504  /50x.html;
    location = /50x.html {
        root   /usr/share/nginx/html;
    }
}
```

Lets run our web server on port 8080

```bash
docker run \
    -v $PWD/index.html:/www/index.html  \
    -v $PWD/default.conf:/etc/nginx/conf.d/default.conf \
    -p 8080:8080 -it web
```

Now try to access [http://localhost:8080](http://localhost:8080)

---

## How to optimize docker image size

**Use case:**

- Simple web server in golang
- Build golang code
- Run web server and expose port 8080

Create dir `demo_app`  and `cd demo_app`

Code: `web_app.go`

```go
package main

import (
    "fmt"
    "net/http"
    "log"
)

func main() {
    http.HandleFunc("/", func (w http.ResponseWriter, r *http.Request) {
        fmt.Fprintf(w, "Hello World!")
        query := r.URL.Query()
        name := query.Get("name")
        if name != "" {
            log.Printf("Received request for %s\n", name)
        }
    })
    log.Println("Starting Server")
    if err :=  http.ListenAndServe(":8080", nil); err != nil {
        log.Fatal(err)
    }
    log.Println("Server stopped")
}
```

Make sure you create `web_app.go`  in with above content

## Method 1# Standard process

We will use standard approach to build golang code to build docker image

```docker
FROM golang:1.10

# Set the Current Working Directory inside the container
WORKDIR /go/src/app

# Copy everything from the current directory to the PWD(Present Working Directory) inside the container
COPY . .

# Download all the dependencies
RUN go get -d -v ./...

# Install the package
RUN go install -v ./...

# This container exposes port 8080 to the outside world
EXPOSE 8080

# Run the executable
CMD ["/app"]
```

Now in your current directory you have two files:

- `web_app.go`
- `Dockerfile`

Let's build our demo app

```docker
docker build -t demo:v1.0.0 .
```

Run app in deattach mode

```docker
docker run -p 8080:8080  -dt demo:v1.0.0
```

You can check logs:

```docker
docker logs -f <container-id>
```

Now access from browser [http://localhost:8080/](http://localhost:8080/)

You should be able to see "Hello World!" response from above link

Let's check docker image size that you just created

```docker
docker images | grep demo | grep v1.0.0
```

Output:

```
demo   v1.0.0   0cedc4305a0c  3 minutes ago       767MB
```

Its "767MB", that's not fair

How we can reduce this?

First thing in mind, we need to use small base image and the smallest base image is `alpine`

clean-up: Stop all running containers and remove stopped containers

```docker
docker stop $(docker ps -qa)
docker rm $(docker ps -qa)
```

## Method 2# Using alpine as base image

Let's create file called `Dockerfile.alpine` with following content

```docker
FROM golang:1.7-alpine

RUN apk add --update --no-cache git && \
  rm -rf /tmp/* /var/cache/apk/*

# Set the Current Working Directory inside the container
WORKDIR /go/src/app

# Copy everything from the current directory to the PWD(Present Working Directory) inside the container
COPY . .

# Download all the dependencies
RUN go get -d -v ./...

# Install the package
RUN go install -v ./...

# This container exposes port 8080 to the outside world
EXPOSE 8080

# Run the executable
CMD ["/app"]
```

Now in your current directory you have two files:

- `web_app.go`
- `Dockerfile`
- `Dockerfile.alpine`

Let's build our demo app

```docker
docker build -t demo:v1.0.0-alpine -f Dockerfile.alpine .
```

Run app in deattach mode

```docker
docker run -p 8080:8080  -dt demo:v1.0.0-alpine
```

You can check logs:

```docker
docker logs -f <container-id>
```

Now access from browser [http://localhost:8080/](http://localhost:8080/)

You should be able to see "Hello World!" response from above link

Let's check docker image size that you just created

```docker
docker images | grep demo | grep v1.0.0
```

Output:

```
demo   v1.0.0-alpine   c357bed946dd  1 minutes ago       263MB
```

From  "767MB" to "263MB" that a lot improvement

But wait there is more!

You can even reduce more using "distroless" images build by google

## Method 3# Using distroless as base image

You can learn more about distroless from [this](https://github.com/GoogleContainerTools/distroless) page

Let's create file `Dockerfile.destroless`

```docker
FROM golang:1.10 as build-env

WORKDIR /go/src/app
ADD . /go/src/app

RUN go get -d -v ./...

RUN go build -o /go/bin/app

FROM gcr.io/distroless/base
COPY --from=build-env /go/bin/app /
CMD ["/app"]
```

Yes, you see some strange thing here, two `FROM` statement, yes, that's actually two stages, you can go even multi stage using multiple images and copy only require stuff from each stage you want.

In above docker file we have two stages

- Build stage `FROM golang:1.10 as build-env`
    - You can install require tools to build your code and that build dev packages are useless after build
    - Golang advantages here: Go links its dependencies statically by default. This has the benefit of having one simple and portable deliverable — the binary itself
    - TIP: Read about static libs vs shared libs for interview
- Runtime stage `FROM [gcr.io/distroless/base](http://gcr.io/distroless/base)`
    - Copy static binary from build stage
    - Run binary

Let's build it

```docker
docker build -t demo:v1.0.0-destroless  -f Dockerfile.destroless .
```

Whats size?

```docker
demo   v1.0.0-destroless   d4681a0aa1c9        1 minutes ago         23.5MB
```

WOW 23.5MB !!

TIP: Show this demo in your company .

Now run app and see if its working or not.


### Reference:
- https://docs.docker.com/
- https://dev.to/
- https://luminousmen.com/
