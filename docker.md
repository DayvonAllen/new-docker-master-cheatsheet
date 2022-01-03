## Docker
- Released in 2013.
- Docker is all about speed, increases developer productivity and the speed it takes to get development done.
- Docker also reduces costs, easy to migrate to the cloud where you only pay for what you use.
- Containers reduce complexity, come with all dependencies packaged together.
- It's a containerization software.
- Docker in a nutshell:
``` 
Docker is a software framework for building, running, and managing containers on servers and the cloud. 
The term "docker" may refer to either the tools (the commands and a daemon) or to the Dockerfile file format.

It used to be that when you wanted to run a web application, you bought a server, installed Linux, set up a LAMP stack, 
and ran the app. 
If your app got popular, you practiced good load balancing by setting up a second server 
to ensure the application wouldn't crash from too much traffic.

Times have changed, though, and instead of focusing on single servers, 
the Internet is built upon arrays of inter-dependent and redundant servers in a system commonly called "the cloud". 
Thanks to innovations like Linux kernel namespaces and cgroups, the concept of a server could be removed from the 
constraints of hardware and instead became, essentially, a piece of software. 
These software-based servers are called containers, and they're a hybrid mix of the Linux OS they're running on plus a 
hyper-localized runtime environment (the contents of the container).
```
---

## Container
- is an instance of an image running as a process.
- you can have many containers based off the same image.
- containers are:
  - just processes running on a host operating system.
  - limited to what resources they can access.
  - exit when the process stops.
  - not virtual machines.
---

## Image
- It's the App binaries and dependencies of your app.
  - It's also the metadata about the image data and how to run the image.
  - Does not include a kernel, or kernel modules(e.g. drivers).
  - The host provides the kernel.
  - It's not a complete OS.
- We get all of our images from registries, the default docker registry is `Docker Hub`.
  - `Docker Hub` - a registry for docker images, does what `github` does for source code but for docker images.
---
  
## Docker Network
- Each container is connected to a private virtual network `bridge`.
- Each virtual network routes through an NAT firewall on the host's IP.
- All containers on a virtual network can talk to each other without `-p` or `--publish`.
- We can make new virtual networks.
- We can attach containers to more than one virtual network(or none).
- We can skip default virtual network configuration and use the host IP with this option(`--net=host`).
- We can use different docker network drivers to gain new abilities.
- Containers in the same virtual network can find each other by specifying each other's container name.
- The DNS will automatically resolve their names to an IP, and they will be able to communicate.
---

## How To Push To Docker Hub(DH)
- `Tag` - a pointer to specific image commit(kinda like a version).
- `docker login`
- `docker logout`
- `docker image tag <image> <DH_username>/<image_name>` - creates a repository with the latest tag from an image.
  - `docker image push <image>` - push to DH when no tag name is specified.
- How to create a repository with a `tag` for a custom image:
  - If you don't specify a tag it will default to `latest`
  - `docker image tag <image> <DH_username>/<image_name>:<tag_name>`
    - `docker image push <image>:<tag_name>` - push to DH when tag is specified.
---