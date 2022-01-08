
# Creating and Using Containers Like a Boss

# Should Always Give Containers Names!!!

## Basic Commands
- `docker container run <image_name>` - creates a new container in attached mode(means we are listening for output, like logs from the container)
- `docker container attach <container_name>` - attaches to a detached running container.
  - `docker attach <container_name>` - attaches to a detached running container.
- `docker container run --publish 80:80 nginx` - run `nginx` server and port bind to port 80
- `docker container run --publish 80:80 --detach nginx` - same as above but in detached mode(means we are not listening to the output).
- `docker container run --rm <image>` - will delete this container once it stops running.
- `docker container run -v <volume_name>:<path_to_where_volume_should_be _created> <image>` - creates named volume.
- `docker container stop <container ID>` - stops a container
- `docker container ls` - list only running containers
- `docker container ls -a ` - lists all docker containers.
- `docker container start <container_name>` - restarts a stopped container.
  - `docker start -a <container_name>` - starts a container in attached mode
- `docker container stop <container_name>` - stops a container.
- `docker container run --publish 80:80 --detach --name webhost nginx` - `--name` will give the container a name.
- `docker container logs <container name>` - get logs for container
  - `docker container logs -f <container name>` - enables follow mode, which will gets previously printed logs and attach to the container to continue getting logs.
- `docker container top <container name>` - list the processes running in a certain container.
- `docker container --help` - gives list of commands.
- `docker container rm 63f 690 ode` - removes containers.
  - In the above case, 3 containers are removed by the first 3 characters of their `container IDs'`
- `docker image prune` - to clean up just "dangling" images.
- `docker images` - shows docker images
- `docker stats <container_name>` - shows how many resources a container is using
- `docker rmi <image_name>` - removes images
- `docker port <container_name>` - lists container's port mappings.
- `docker pause <container_name>` - pause all processes within a container.
- `docker inspect <container_name>` - returns low level information on docker objects(config info, ports and stuff like that).
- `docker kill <container_name>` - kill a container, doesn't wait for graceful shutdown.
- `docker system prune` -  will clean up everything.
- `docker image prune -a` - will remove all images you're not using.
- `docker system df` - shows disk usage.
- `docker history <image>` - shows the layers of changes made in an image.
- `docker build -f <other_dockerfile>` - allows you to build a dockerfile that's not the default `Dockerfile`
- `docker image build -t <DH_username>/<image_name>:<optional_tag_name> <path_to_Dockerfile>` - build `Dockerfile` to an image.
  - `docker image build -t <DH_username>/<image_name> <path_to_Dockerfile>` - build `Dockerfile` to an image with `latest` tag name.
- Volumes:
  - `docker volume rm <volume_name>` - remove local volume
  - `docker volume create` - create volume
  - `docker volume inspect` - inspect
  - `docker volume ls` - list volumes
  - `docker volume prune` - removes all unused volumes.
  - `-f` - optionally flag which means force.
---

## Pass Environment Variables Into A Container:
- `-e` or `--env` - will allow you to pass environment variables into a container.
  - `docker container run -d -p 3306:3306 --name db -e MYSQL_RANDOM_ROOT_PASSWORD=yes MYSQL_RANDOM_ROOT_PASSWORD mysql`
    - Will generate a random root password for mySQL, must use `docker container logs <mySQL_container_name>` to get the 
    - randomly generated password.
---
  
## Container VS. VM: It's Just a Process
- `docker run --name mongo -d mongo` - names a container `mongo` while using the `mongo` image.
  - If you don't give a container a name, docker will automatically generate a random name for the container.
- `docker top mongo` - list the processes running in a container named `mongo`.
- `docker stop mongo` - stops a container named `mongo`.
- `docker ps` - list only running containers
- `docker ps -a` - list all containers
- `docker start mongo` - start a container named `mongo`.
---

## Assignment Answers: Manage Multiple Containers
- `docker container run -d -p 3306:3306 --name db -e MYSQL_RANDOM_ROOT_PASSWORD=yes MYSQL_RANDOM_ROOT_PASSWORD mysql`
- `docker container logs db`
- `docker container run -d --name webserver -p 8080:80 httpd`
- `docker ps`
- `docker container run -d --name proxy -p 80:80 nginx`
- `docker container stop TAB COMPLETION`
- `docker container ls -a`
- `docker container rm`
- `docker image ls`
---

## What's Going On In Containers: CLI Process Monitoring
- `docker container run -d --name nginx nginx`
- `docker container run -d --name mysql -e MYSQL_RANDOM_ROOT_PASSWORD=true mysql`
- `docker container ls`
- `docker container top mysql`
- `docker container top nginx`
- `docker container inspect <container_name>` - gets details of specified container config.
- `docker container stats --help`
- `docker container stats` - get performance stats for all containers
---

## Getting a Shell Inside Containers: No Need for SSH
- `docker container run -help`
- `docker container run -it --name proxy <container_name> bash` - start a container interactively with a bash shell inside the container
- `docker container ls`
- `docker container ls -a`
- `docker container run -it --name ubuntu ubuntu`
- `docker container start --help`
- `docker container start -ai ubuntu`
- `docker container exec --help`
- `docker container exec -it mysql bash` - run additional command in existing container, in this case `bash`, so we can
  - get a shell inside the container.
- `docker pull alpine`
- `docker image ls` - list docker images on host machine.
- `docker container run -it alpine bash`
- `docker container run -it alpine sh`
---

## Docker Networks: Concepts for Private and Public Comms in Containers
- `docker container run -p 80:80 --name webhost -d nginx`
- `docker container port <container_name>` - check which ports are exposed for a certain running container.
- `docker container inspect --format '{{ .NetworkSettings.IPAddress }}' <container_name>` - get IP for a certain container.
---

## Docker Networks: CLI Management of Virtual Networks
- `docker network ls` - list all created networks.
- `docker network inspect <network_name>` - inspect a network
  - `docker network inspect bridge`
- `docker network create <network_name>`(default driver is bridge) - create a network.
  - `docker network create --driver <driver> <network_name>` - can optionally specify the driver.
- `docker network create --help`
- `docker container run -d --name <container_name> --network <network_name> <image>` - create a container in a certain network
- `docker network connect` - attach a network to a container.
- `docker network disconnect` - detach a network from a container.
- `docker container disconnect`
---

## Docker Networks: DNS and How Containers Find Each Other
- We just specify the container names to communicate in the same Virtual Network.
- When using the default network the containers cannot communicate by default, but they can when you create your own network(should create your own network, good practice)
- `docker container ls`
- `docker network inspect TAB COMPLETION`
- `docker container run -d --name my_nginx --network my_app_net nginx`
- `docker container inspect TAB COMPLETION`
- `docker container exec -it my_nginx ping new_nginx`
- `docker container exec -it new_nginx ping my_nginx`
- `docker network ls`
- `docker container create --help`
---

## Assignment Answers: Using Containers for CLI Testing
- `docker container run --rm -it centos:7 bash`
- `docker ps -a`
- `docker container run --rm -it ubuntu:14.04 bash`
- `docker ps -a`
---

## Assignment Answers: DNS Round Robin Testing
- `docker network create dude`
- `docker container run -d --net dude --net-alias search elasticsearch:2`
  - `--network-alias` or `--net-alias` - lets you create a network alias, so more than one container handle requests the same requests
    - used for horizontal scaling, only one container can have a certain name, so we use this to make sure multiple
    - containers can handle the same traffic.
- `docker container ls`
- `docker container run --rm --net dude alpine nslookup search`
- `docker container run --rm --net dude centos curl -s search:9200`
- `docker container ls`
- `docker container rm -f TAB COMPLETION`
---