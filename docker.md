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
- is your running application, built from the image.
- it's an isolated unit of software which is based on an image. 
  - A running instance of that image.
- containers are:
  - just processes running on a host operating system.
  - limited to what resources they can access.
  - exit when the process stops.
  - not virtual machines.
- Multiple containers can be based on the same image without interrupting each other.
- Containers are separated from each other and have no shared data or state by default(This is `isolation`). 
---

## Image
- It's the App binaries and dependencies of your app.
  - It's also the metadata about the image data and how to run the image.
  - Does not include a kernel, or kernel modules(e.g. drivers).
  - The host provides the kernel.
  - It's not a complete OS.
- Images are `blueprints` for containers which then are running instances with read and write access. 
- We get all of our images from registries, the default docker registry is `Docker Hub`.
  - `Docker Hub` - a registry for docker images, does what `github` does for source code but for docker images.
- They are read-only and containers execute them. 
- Every time we make a change we have to rebuild the image to get the changes to the image.
- Images are layered based, docker will cache every instruction result(using `SHA-256 hashes`) and when you rebuild it will use these cached results if nothing has changed, this is called a `layer based architecture`, every instruction represents a layer.
- An image is built up based on these multiple layers(instructions, like `COPY`, `WORKDIR`, `RUN` etc.)
  - When you build or rebuild an image, only the instructions where something has changed and all of the instructions after are re-evaluated
    - For node apps, it's best to do `npm install` at the before copying your code in the `dockerfile` because if your code changes often and you have `npm install` before your code, that command will have to run everytime.
    - Things that are likely to change often should come after things that are less likely to change, so docker can use its cache mechanism(this will increase image build time performance)
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
- `docker image tag <prev_image_name> <DH_username>/<image_name>`(tag will default to latest) - rename an image.
  - `docker image push <image>` - push to DH when no tag name is specified.
- How to name an image with a `tag` for a custom image:
  - If you don't specify a tag it will default to `latest`
  - `docker image tag <prev_image_name> <DH_username>/<image_name>:<tag_name>`
    - `docker image push <image>:<tag_name>` - push to DH when tag is specified.
---
    
## Persistent Data
- Two ways to persist data:
  - `Volumes`:
    - Makes a special location outside of the container's unique file system to save unique data.
    - Keeps data past container removal and allows us to attach this data to any container we want.
    - `VOLUME <PATH_TO_VOLUME_LOCATION>` - How to create and assign a new volume to this container(for `Dockerfile`).
    - Volumes need manual deletions.
  - `Bind Mounts`(can't use in `Dockerfile`):
    - link container path to host path.
    - sharing or mounting a hosted directory or file to a container.
---

## Volumes
- they are folders on your host machine hard drive which are mounted(`made available`, mapped) into containers.
  - folders which you make docker aware of then mapped to a directory in a docker container, saves all data on your computer not in the container(for persistent data, not editable data).
- Two Type if Exterbak Data Storages:
  - `Volumes` - managed by docker and we don't know where docker puts them on our host machine
    - `Anonymous volumes` - name is generated automatically, only exists as long as our container exists.
      - docker deletes theses volumes.
    - `Named volumes` - We give the volume a name, volumes will survive container shutdown(good for persistence of data)
      - docker doesn't deletes theses volumes.
  - `Bind Mounts` - managed by us, we set the path for where the container volume should be(great for persistent, editable data(ex. changing source code for dev purposes)).
---