# TOR: Application development in Torizon

## Torizon Bootcamp with Sergio Prado

### Application development in Torizon

===

## Linux containers

* Container technology in general and Linux containers, in particular, have become increasingly popular in recent years.
* The usage of containers in data centers and cloud computing environments is consolidated, and tools like Docker and Kubernetes have become quite popular.
* It's no different in the world of embedded products since many container-based Linux distributions for embedded systems like Torizon OS have started to appear in recent years.
* But what are containers? What problems do they solve? What technologies are involved?

===

## What are containers?

* Containers are a way to package software and its dependencies in a standard, deployable and runnable form.
* They have a number of resources allocated to them (CPU, memory, I/O) and run isolated from the rest of the system.
* A container is not a virtual machine, it is just a mechanism to package and run software.
  * A containerized solution is lighter and less resource intensive.
  * The kernel is shared with other processes and containers running on the operating system.

===

## Diagram: Linux containers

<img src="/training/images/tor/containers-arch.png"
     alt="Linux containers"
     width="60%" />

===

## Motivations to use containers on embedded

* *Productivity*: focus on application development.
* *Flexibility*: easier to leverage modern tools, programming languages and technologies.
* *Isolation*: possible to have different execution environments in the same host.
* *Modularity*: encourage the development of a more modular system, improving maintenance, portability and reuse.
* *Control*: more control over the usage of hardware and software resources.
* *Updatability*: easier to implement a more robust, fast and fail-safe update system.
* *Security*: provides an infrastructure to improve security!

===

## Challenges in using containers

* Increased consumption of hardware resources like RAM and storage: mitigated by the Docker layering mechanism. Hardware is cheap, time is expensive!
* Wearing out the flash memory with frequent writes to the file system: can be managed to avoid extra writes do the storage device.
* Creating a container image with cross-compiled binaries for the target platform is not trivial: Docker makes that simple.
* Software license management: this has improved in recent years with SBOMs.  
[https://docs.docker.com/engine/sbom/](https://docs.docker.com/engine/sbom/)
* New technology for the engineering team: it is always nice to learn something new!

===

## How containers are implemented?

* A container runtime implementation uses several Linux kernel features, including namespaces and cgroups.
  * The namespaces feature makes it possible to isolate the execution of a process on Linux (PID, users, network connections, mount points, etc).
  * Control groups or cgroups allow to partition system resources (CPU, memory, I/O, etc) by process or group of processes.
* Using these and some other kernel features, it is possible to create a completely isolated execution environment for applications on Linux.

===

## Torizon OS implementation

* Torizon OS uses Docker as the container engine.
  * There is also an experimental Podman image (container engine from Red Hat).
* Torizon OS uses Compose as a tool for defining and running multi-container Docker applications.
  * Containers are defined in a YAML file.
  * Compose reads the YAML file at boot time to configure and start containers.
* Several [container images for Torizon](https://developer.toradex.com/torizon/provided-containers/list-of-container-images-for-torizon/) are available on Docker Hub for usage in development and production.

===

## Docker

* Docker can mean different things to different people!
  * Docker is a set of platform as a service (PaaS) products that use OS-level virtualization to deliver software in packages called containers.
  * Docker, Inc. is the name of the company behind it.
  * The container engine (software that runs container images) is also called Docker!
  * The command line tool is called *docker*.
* Docker, Inc. didn't revolutionize the technology, but it was certainly responsible for popularizing the usage of containers.
  * There are a few alternatives to Docker, including Podman and LXC.

===

## Docker components

* **Docker engine**: low-level component (daemon) that creates and runs containers.
* **Docker client**: command-line tool that is used to interact with the Docker engine.
* **Docker Hub**: public repository of container images.
* **Docker Desktop**: graphical application to manage containers (Linux, Windows and MacOS support).
* **Docker Compose**: tool for defining and running multi-container Docker applications.

===

## Diagram: Docker components

<img src="/training/images/tor/docker-architecture.png"
     alt="Docker architecture"
     width="80%" />

===

## Docker architecture

* The Docker client (*docker*) uses the Docker API to interact with the Docker daemon.
* The Docker daemon (*dockerd*) listens for Docker API requests and manages Docker objects (images, containers, networks, and volumes).
  * It communicates with a "high-level" container runtime called *containerd*.
* *containerd* manages the complete container lifecycle, from image transfer and storage, to container execution, supervision and networking.
  * It communicates with a "low-level" container runtime (*runc*) via the OCI standard.
* *runc* is responsible to create and run containers.

===

## Diagram: Docker architecture

<img src="/training/images/tor/docker-internal.png"
     alt="Docker internal architecture"
     width="70%" />

===

## Container registry

* A container registry is a repository of container images.
  * It's a marketplace of containerized applications!
* There are private and public registries:
  * Private registries allow for access control to their images without having them publicly available (Google Container Registry, Amazon Elastic Container Registry, Microsoft's Azure Container Registry, etc).
  * Public registries are publicly available repositories of container images (may have some limitations for free accounts).
* The most popular public container registry is Docker Hub.

===

## Docker Hub

* Docker Hub is a public container registry that anyone can use to publish and download container images.  
[https://hub.docker.com/](https://hub.docker.com/)
* Be aware that free accounts have some limitations:  
[https://www.docker.com/pricing/](https://www.docker.com/pricing/)
* Docker Hub is configured as the default registry in the Docker engine.
* There are several images published on Docker Hub for Torizon:  
[https://hub.docker.com/u/torizon](https://hub.docker.com/u/torizon)

===

## Torizon containers

* *debian*: Debian based image to start a simple container.
* *weston*: Wayland libraries and Weston compositor to run graphical applications.
  * *weston-vivante*: variant for iMX8 based modules (with Vivante support).
* *qt5-wayland*: Wayland and Qt5 libraries.
  * *qt5-wayland-vivante*: variant for iMX8 based modules (with Vivante support).
* *chromium*: Chromium browser to run web-based GUIs.
* *cog*: Cog browser to run web-based GUIs.

===

## Docker CLI

* The Docker client (*docker* command) is a tool to interface with the Docker Engine.
  * Makes it possible to manage containers from the command-line.
* It uses the Docker API (RESTful API accessed via HTTP) to interact with the Docker daemon (*dockerd*).  
[https://docs.docker.com/engine/api/](https://docs.docker.com/engine/api/)
* Documentation available on the Docker website:  
[https://docs.docker.com/engine/reference/commandline/cli/](https://docs.docker.com/engine/reference/commandline/cli/)

===

## docker help

```Shell Session
$ docker --help

Usage:  docker [OPTIONS] COMMAND

A self-sufficient runtime for containers

Options:
      --config string      Location of client config files (default "/etc/docker")
  -c, --context string     Name of the context to use to connect to the daemon
  -D, --debug              Enable debug mode
  ...

Management Commands:
  builder     Manage builds
  config      Manage Docker configs
  container   Manage containers
  ...

Commands:
  attach      Attach local standard input, output, and error streams to a running container
  build       Build an image from a Dockerfile
  ...
```

===

## docker version

```Shell Session
$ docker version
Client:
 Version:           19.03.14-ce
 API version:       1.40
 Go version:        go1.14.15
 Git commit:        5eb3275d40
 Built:             Tue Jul 26 14:39:09 2022
 OS/Arch:           linux/arm64
 Experimental:      false

Server:
 Engine:
  Version:          19.03.14-ce
  API version:      1.40 (minimum version 1.12)
  Go version:       go1.14.15
  Git commit:       5eb3275d4006e4093807c35b4f7776ecd73b13a7
  Built:            Thu Jan  1 00:00:00 1970
  OS/Arch:          linux/arm64
  Experimental:     false
 containerd:
  Version:          v1.2.14-3-g3b3e9d5f6.m
  GitCommit:        3b3e9d5f62a114153829f9fbe2781d27b0a2ddac.m
 runc:
  Version:          1.0.0-rc8
  GitCommit:        425e105d5a03fabd737a126ad93d62a9eeede87f-dirty
 docker-init:
  Version:          0.18.0
  GitCommit:        fec3683-dirty
```

===

## docker info

```Shell Session
$ docker info   
Client:
 Debug Mode: false

Server:
 Containers: 3
  Running: 3
  Paused: 0
  Stopped: 0
 Images: 3
 Server Version: 19.03.14-ce
 Storage Driver: overlay2
  Backing Filesystem: extfs
  Supports d_type: true
  Native Overlay Diff: true
 Logging Driver: journald
 Cgroup Driver: cgroupfs
 Plugins:
  Volume: local
  Network: bridge host ipvlan macvlan null overlay
  Log: awslogs fluentd gcplogs gelf journald json-file local logentries splunk syslog
 Swarm: inactive
 Runtimes: runc
 Default Runtime: runc
 Init Binary: docker-init
 containerd version: 3b3e9d5f62a114153829f9fbe2781d27b0a2ddac.m
 runc version: 425e105d5a03fabd737a126ad93d62a9eeede87f-dirty
 init version: fec3683-dirty (expected: fec3683b971d9)
 Security Options:
  seccomp
   Profile: default
 Kernel Version: 5.4.193-5.7.0+git.f78299297185
 Operating System: TorizonCore 5.7.0+build.17 (dunfell)
 OSType: linux
 Architecture: aarch64
 CPUs: 4
 Total Memory: 3.546GiB
 Name: verdin-imx8mp-06817295
 ID: KPNI:ERR5:BMB4:XKSE:LDN6:ZQ36:24OQ:RAC5:OYSZ:COGG:A7DK:MNXR
 Docker Root Dir: /var/lib/docker
 Debug Mode: false
 Registry: https://index.docker.io/v1/
 Labels:
 Experimental: false
 Insecure Registries:
  127.0.0.0/8
 Live Restore Enabled: false
```

===

## Images vs Containers

* The *docker* tool provides commands to manage images and containers, and it is important to understand the difference between them.
* Images are a read-only template used to create and run containers.
  * Contain the executable and all dependencies (tools, libraries, configuration files, etc) required to run the application(s).
* Containers are the live, running instances of images.
  * It's possible to have multiple containers (instances) of the same image.
  * They are transient by design (changes in a container are not persistent by default).

===

## Managing images

* There are a few commands available in Docker to manage container images.
* A container image can be built with the *build* command.
* The commands *push* and *pull* are able to respectively upload and download images from a container registry.
* Images can be saved and loaded from a tar archive with the *save* and *load* commands.
* The list of images can be displayed with the *images* command.
* The images can be deleted with the *rmi* command.

===

## Pulling an image

```text [|1-11|13-15|]
$ docker pull nginx:1.23.3
1.23.3: Pulling from library/nginx
934ce60d1040: Pull complete 
238b470e100d: Pull complete 
fd4ff90344fc: Pull complete 
7be7509b8147: Pull complete 
fc07d3e6158f: Pull complete 
d44fa61c1ffa: Pull complete 
Digest: sha256:b8f2383a95879e1ae064940d9a200f67a6c79e710ed82ac42263397367e7cc4e
Status: Downloaded newer image for nginx:1.23.3
docker.io/library/nginx:1.23.3

$ docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
nginx               1.23.3              d8906c7d4c44        6 days ago          135MB
```

===

## Running a container

* The *run* command will start a new container from a container image.
  * The *-d* flag can be used to run the container in the background (detached).
  * If the image is not available locally, it will be automatically pulled from a registry.
* To list the containers, the *ps* command can be used.
* The *exec* command is able to execute a command inside a container.
* Logs from the container can be collected via the *logs* command.
* Statistics about the execution of a container can be displayed via the *top* and *stats* commands.

===

## docker run

```text [|1-2|4-6|8-22|]
$ docker run -d --name webserver nginx:1.23.3
852836adf39d1daf40576d5dc63f7b781af819720a125a993280ec227cdef4bf

$ docker ps -a
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS               NAMES
852836adf39d        nginx:1.23.3        "/docker-entrypoint.�…"   25 seconds ago      Up 23 seconds       80/tcp              webserver

$ docker logs webserver
/docker-entrypoint.sh: /docker-entrypoint.d/ is not empty, will attempt to perform configuration
/docker-entrypoint.sh: Looking for shell scripts in /docker-entrypoint.d/
/docker-entrypoint.sh: Launching /docker-entrypoint.d/10-listen-on-ipv6-by-default.sh
10-listen-on-ipv6-by-default.sh: info: Getting the checksum of /etc/nginx/conf.d/default.conf
10-listen-on-ipv6-by-default.sh: info: Enabled listen on IPv6 in /etc/nginx/conf.d/default.conf
/docker-entrypoint.sh: Launching /docker-entrypoint.d/20-envsubst-on-templates.sh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/30-tune-worker-processes.sh
/docker-entrypoint.sh: Configuration complete; ready for start up
2023/01/17 10:47:21 [notice] 1#1: using the "epoll" event method
2023/01/17 10:47:21 [notice] 1#1: nginx/1.23.3
2023/01/17 10:47:21 [notice] 1#1: built by gcc 10.2.1 20210110 (Debian 10.2.1-6) 
2023/01/17 10:47:21 [notice] 1#1: OS: Linux 5.4.193-5.7.0+git.f78299297185
2023/01/17 10:47:21 [notice] 1#1: getrlimit(RLIMIT_NOFILE): 1024:524288
...
```

===

## top and stats

```text [|1-7|9-11|]
$ docker top webserver
UID                 PID                 PPID                C                   STIME               TTY                 TIME                CMD
root                2376                2355                0                   10:47               ?                   00:00:00            nginx: master process nginx -g daemon off;
101                 2422                2376                0                   10:47               ?                   00:00:00            nginx: worker process
101                 2423                2376                0                   10:47               ?                   00:00:00            nginx: worker process
101                 2424                2376                0                   10:47               ?                   00:00:00            nginx: worker process
101                 2425                2376                0                   10:47               ?                   00:00:00            nginx: worker process

$ docker status webserver
CONTAINER ID        NAME                CPU %               MEM USAGE / LIMIT     MEM %               NET I/O             BLOCK I/O           PIDS
852836adf39d        webserver           0.00%               5.113MiB / 3.546GiB   0.14%               3.72kB / 0B         0B / 8.19kB         5
```

===

## docker exec

```text [|1|3-4|6-7|9|11-14|16|]
$ docker exec -it webserver /bin/sh

# cat /etc/issue
Debian GNU/Linux 11 \n \l

# ps
/bin/sh: 1: ps: not found

# apt update && apt install procps

# ps
    PID TTY          TIME CMD
     39 pts/0    00:00:00 sh
    384 pts/0    00:00:00 ps

# exit
```

===

## Container lifecycle

* When the container main process terminates, the container stops its execution.
  * However, changes to the filesystem are not lost.
* The container can be restarted by re-executing the main process or using the *start* command.
* A container can be stopped with the *stop* or *kill* commands, and restarted with the *restart* command.
* When stopped, a container can be removed with the *rm* command.
  * The container is automatically removed when executed with the *--rm* flag.

===

## Starting/stopping containers

```text [|1-3|5-6|8-10|12-13|15-17|]
$ docker ps -a
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS               NAMES
852836adf39d        nginx:1.23.3        "/docker-entrypoint.�…"   13 minutes ago      Up 13 minutes       80/tcp              webserver

$ docker stop webserver
webserver

$ docker ps -a
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS                      PORTS               NAMES
852836adf39d        nginx:1.23.3        "/docker-entrypoint.�…"   14 minutes ago      Exited (0) 18 seconds ago                       webserver

$ docker start webserver
webserver

$ docker ps -a
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS               NAMES
852836adf39d        nginx:1.23.3        "/docker-entrypoint.�…"   16 minutes ago      Up 2 minutes        80/tcp              webserver
```

===

## Removing containers

```text [|1-3|5-6|8-9|11-12|14-15|17-19|]
$ docker ps -a
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS               NAMES
852836adf39d        nginx:1.23.3        "/docker-entrypoint.�…"   16 minutes ago      Up 2 minutes        80/tcp              webserver

$ docker rm webserver
Error response from daemon: You cannot remove a running container 852836adf39d1daf40576d5dc63f7b781af819720a125a993280ec227cdef4bf. Stop the container before attempting removal or force remove

$ docker stop webserver
webserver

$ docker rm webserver
webserver

$ docker ps -a
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES

$ docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
nginx               1.23.3              d8906c7d4c44        6 days ago          135MB
```

===

## Networking

* By default, the network inside the container is isolated from the host OS.
* Docker is very flexible in how to configure the network between containers or between a container and the host OS.  
[https://docs.docker.com/network/](https://docs.docker.com/network/)
* Some of the options to configure networking inside a container:
  * Exposing a port to the host OS via the *--publish* or *-p* flag.
  * Exposing the host network inside the container via the *--net host* option (not recommended in most cases due to security implications).
  * Using the *network* command to create private networks and connect containers to them.

===

## Exposing a port

```text [|1-2|4-5|7-22|]
$ docker run --rm -d --name webserver -p 8080:80 nginx:1.23.3
481f51a35eac2725098735fae7b9cb41229c87883431efdc784dfcb2546a9402

$ sudo netstat -natp | grep 8080
tcp6       0      0 :::8080                 :::*           LISTEN      3449/docker-proxy

$ curl http://localhost:8080
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>
...
```

===

## Storage

* Inside a container, writes to the filesystem are not persistent.
  * As soon as the container is removed, changes to the filesystem are lost!
* There are two approaches to persistent storage in a container:
  * **Bind mount**: share a file or directory from the host OS inside a container.
  * **Volumes**: data storage abstraction independent from the host OS filesystem.
* Both options can be configured via the *-v* flag.

===

## Creating a bind mount

```text [|1-2|4|6-7|9-10|]
$ mkdir -p /var/www/
$ sudo chown -R torizon:torizon /var/www/

$ echo "<html><p>Hello world!</p></html>" > /var/www/index.html

$ docker run --rm -d --name webserver -p 8080:80 -v /var/www:/usr/share/nginx/html nginx:1.23.3
b58298f7a853a825199cc246606f730da19fc3dc3161b3d54c7f81bf2ba62fb8

$ curl http://localhost:8080
<html><p>Hello world!</p></html>
```

===

## Accessing hardware devices

* The Linux kernel has two main mechanisms to provide access to hardware devices for user space applications: files and network.
* For devices that are exported as a network interface (Ethernet, WiFi, CAN, NFC, etc), configuring the network inside the container is enough.
* For devices that are accessible as files (*/dev*, */sys*, etc), there are a few options:
  * Bind mount the file(s) with the *-v* flag.
  * Add a host device to the container via the *--device* or *--device-cgroup-rule* flags.
  * Use the *--privileged* flag (good for prototyping, not recommended for production).
  * For details on security, watch the talk [Designing Secure Containerized Applications for Embedded Linux Devices](https://www.youtube.com/watch?v=lP5F-9QK3X0) (OSS Latin America - Sergio Prado - 2022).

===

## Accessing the RTC

```text [|1-2|4|6-7|9|11-13|15-16|]
$ docker run --rm -d --name webserver -p 8080:80 -v /var/www:/usr/share/nginx/html nginx:1.23.3
6d0e99cf8c78103f24c67c08a5642f855755ad9746a2383c28a275efe8358dc5

$ docker exec -it webserver ls -l /dev/rtc0

$ docker exec -it webserver ls -l /dev/rtc0
ls: cannot access '/dev/rtc0': No such file or directory

$ docker stop webserver

$ docker run --rm -d --name webserver -p 8080:80 -v /var/www:/usr/share/nginx/html \
             --device /dev/rtc0 nginx:1.23.3
72e083cfc9bba08ce0f1dcc45141c6b38719418b425de8540c866a46dd53cbf2

$ docker exec -it webserver ls -l /dev/rtc0
crw------- 1 root root 253, 0 Jan 17 15:44 /dev/rtc0
```

===

## Building a container

* A container image is basically a filesystem bundled in a format defined by the OCI Image Format Specification:  
[https://github.com/opencontainers/image-spec](https://github.com/opencontainers/image-spec)
* There are several tools available to build container images, including Buildah, BuildKit, Podman and Docker.
* Build systems like Buildroot and OpenEmbedded/Yocto Project are also able to create container images.
* In the training, we will learn the basics of building container images with Docker.

===

## Dockerfile

* Docker can build container images via the *build* command.
  * It does this by reading instructions from a configuration file called *Dockerfile*.
* The *Dockerfile* is essentially a list of instructions that Docker will run in order to assemble the image.
* Instructions on how to write a *Dockerfile* are available on the Docker website:  
[https://docs.docker.com/engine/reference/builder/](https://docs.docker.com/engine/reference/builder/)
* An interesting feature for embedded Linux developers is that Docker is able to build container images with multi-platform support:  
[https://docs.docker.com/build/building/multi-platform/](https://docs.docker.com/build/building/multi-platform/)

===

## Writing a Dockerfile

* There are several commands that can be used in a *Dockerfile*, including:
  * *FROM*: select a base image.
  * *ARG*, *ENV*: define environment variables.
  * *RUN*: run commands in the image.
  * *ADD*, *COPY*: add files to the image.
  * *USER*: run the container with a specific user.
  * *ENTRYPOINT*, *CMD*: command to run when the container is started.

===

## Building a web server image (1)

```Shell Session
$ ls
Dockerfile  index.html
```

```html []
<!-- index.html -->
<html>
    <p>Hello world!</p>
</html>
```

```Dockerfile []
# Dockerfile
FROM --platform=linux/arm64 nginx:1.23.3

RUN apt update && \
    apt upgrade -y && \
    apt install -y procps

COPY index.html /usr/share/nginx/html

EXPOSE 80/tcp

CMD ["nginx", "-g", "daemon off;"]
```

===

## Building a web server image (2)

```text [|1|2-5|7-9|11-12|14-16|18-20|22-23|]
$ docker build -t sergioprado/webserver:1.0 .
Sending build context to Docker daemon  3.072kB
Step 1/5 : FROM --platform=linux/arm64 nginx:1.23.3
1.23.3: Pulling from library/nginx
...

Step 2/5 : RUN apt update &&     apt upgrade -y &&     apt install -y procps
  ---> Running in 2dd16a20abc0
...

Step 3/5 : COPY index.html /usr/share/nginx/html
 ---> 305758315442

Step 4/5 : EXPOSE 80/tcp
 ---> 6fd01c279fa8
...

Step 5/5 : CMD ["nginx", "-g", "daemon off;"]
 ---> ad0e58f00199
...

Successfully built ad0e58f00199
Successfully tagged sergioprado/webserver:1.0
```

===

## What happens when an image is built?

* Each container image is made up of a series of layers that are combined together in a stack architecture.
* Every time a user specifies a command, such as *RUN* or *COPY*, a new layer is created.
  * The new layer contains only the changes compared to the previous layer.
  * Changing one layer will impact all subsequent layers.
* Docker reuses these layers to build new container images, which accelerates the building process.

===

## Image layers

<img src="/training/images/tor/container-image-layers.png"
     alt="Container images layers"
     width="100%" />

===

## Pushing to a registry

* When the container image is built, it is stored locally in the build machine.
* To transfer the image to a target device, we might use the *save* and *load* commands, or push to a container registry like Docker Hub.
* To push to Docker Hub, we just have to create an account and use the *login* and *push* commands.
* When pushing a container to a registry, a tag can be defined.
  * A tag is just a label, used to reference different versions of the same image.
  * The default value is *latest*, and it is commonly used to version the image.
  * Each image is also associated with a hash (SHA256), that can be used to uniquely identify the image.

===

## Pushing to Docker Hub

```text [|1-7|9-19|]
$ docker login
Authenticating with existing credentials...
WARNING! Your password will be stored unencrypted in /home/sprado/.docker/config.json.
Configure a credential helper to remove this warning. See
https://docs.docker.com/engine/reference/commandline/login/#credentials-store

Login Succeeded

$ docker push sergioprado/webserver:1.0
The push refers to repository [docker.io/sergioprado/webserver]
8b9c84136713: Pushed 
75593d2fb2c7: Pushed 
ebb3e792ec5a: Mounted from library/nginx 
65812db0d576: Mounted from library/nginx 
683e7a9d8422: Mounted from library/nginx 
e0bf0268ba6a: Mounted from library/nginx 
c7999db6d07b: Mounted from library/nginx 
afd7e44a4e08: Mounted from library/nginx 
1.0: digest: sha256:1cd5fbe919784d0c5a37b465ef2ca9cbd8c1b8a1c4bc3688e3e3f358492d2a46 size: 1989
```

===

## Running on the device

```text [|1|2-13|15-17|19-22|]
$ docker run --rm -d --name webserver -p 8080:80 sergioprado/webserver:1.0
Unable to find image 'sergioprado/webserver:1.0' locally
1.0: Pulling from sergioprado/webserver
934ce60d1040: Already exists 
238b470e100d: Already exists 
fd4ff90344fc: Already exists 
7be7509b8147: Already exists 
fc07d3e6158f: Already exists 
54b40516ae22: Pull complete 
5702d9818fab: Pull complete 
Digest: sha256:1cd5fbe919784d0c5a37b465ef2ca9cbd8c1b8a1c4bc3688e3e3f358492d2a46
Status: Downloaded newer image for sergioprado/webserver:1.0
604726eaf3c9cc87e1e58bac9fd8cefd440ea214a2a2cee7b8280a9d94bb1202

$ docker ps
CONTAINER ID        IMAGE                       COMMAND                  CREATED             STATUS              PORTS                  NAMES
604726eaf3c9        sergioprado/webserver:1.0   "/docker-entrypoint.�…"   11 seconds ago      Up 9 seconds        0.0.0.0:8080->80/tcp   webserver

$ curl http://localhost:8080
<html>
    <p> Hello world! </p>
</html>
```

===

## Docker compose

* Usually, a containerized solution is composed of multiple containers for the different parts of the system (user interface, servers, database, etc).
* Compose is a tool for defining and running multi-container Docker applications.
  * It uses a YAML file (*docker-compose.yml*) to configure and run the containers.
* Using the docker compose plugin (new, implemented in Go) or the *docker-compose* command-line tool (old, implemented in Python), a multi-container solution can be managed as a single application (start, stop, restart, etc).
* The specification of the Compose file is available on the Docker website:  
[https://docs.docker.com/compose/compose-file/](https://docs.docker.com/compose/compose-file/)

===

## Example: Docker compose

```Shell Session
$ docker run --rm -d --name webserver -p 8080:80 -v /tmp:/tmp \
             --device /dev/rtc0 sergioprado/webserver:1.0
```

```yaml []
# docker-compose.yml
# Generated with https://www.composerize.com/

version: '3.3'
services:
    webserver:
        container_name: webserver
        ports:
            - '8080:80'
        volumes:
            - '/tmp:/tmp'
        devices:
            - /dev/rtc0
        image: 'sergioprado/webserver:1.0'
```

===

## Docker compose in Torizon OS

* In Torizon OS, the Compose file is stored at */var/sota/storage/docker-compose*:
```Shell Session
$ ls /var/sota/storage/docker-compose/docker-compose.yml
```
* At boot time, a systemd service called *docker-compose* will run and start the containers defined in the Compose file.
  * This is how [applications are automatically started](https://developer.toradex.com/torizon/application-development/how-to-autorun-an-application-with-torizoncore/) at boot time in Torizon OS.
* Application updates are managed via the Compose file.
  * To update an application, we just have to push the new Compose file to Torizon Cloud.
  * This can be done in Torizon Cloud's dashboard or via TorizonCore Builder.

===

## Pushing the update to Torizon Cloud (1)

```Shell Session
$ torizoncore-builder platform push --credentials credentials.zip \
                                    --package-name webserver \
                                    --package-version 1.0 \
                                    --canonicalize \
                                    docker-compose.yml
Canonicalized file 'docker-compose.lock.yml' has been generated.
Warning: For usage of this package with offline updates, the package name must end with ".lock.yml" or ".lock.yaml".
Pushing 'docker-compose.lock.yml' with package version 1.0 to OTA server. You should keep this file under your version control system.
Successfully pushed docker-compose.lock.yml to OTA server.
```

===

## Pushing the update to Torizon Cloud (2)

<img src="/training/images/tor/torizon-platform-packages-app-update.png"
     alt="Torizon Cloud Application Update"
     width="80%" />

===

## Additional resources

* There are several articles on Toradex's developer website that can help during application development, including:
  * Peripheral Access Overview ([link](https://developer.toradex.com/torizon/application-development/peripheral-access/)).
  * Networking Connectivity Overview ([link](https://developer.toradex.com/torizon/application-development/networking-connectivity/)).
  * GUI Overview ([link](https://developer.toradex.com/torizon/application-development/gui/)).
  * Multimedia Overview ([link](https://developer.toradex.com/torizon/application-development/multimedia/)).  
  * Machine Learning Overview ([link](https://developer.toradex.com/torizon/application-development/machine-learning/)).
  * Torizon Best Practices Guide ([link](https://developer.toradex.com/torizon/torizoncore/best-practices/torizon-best-practices-guide/)).

===

## Exercise 3

### Developing containerized applications
