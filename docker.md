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
---

## Image
- The binary and the source code that makes up your application
- We get all of our images from registries, the default docker registry is `Docker Hub`.
  - `Docker Hub` - a registry for docker images, does what `github` does for source code but for docker images.
---