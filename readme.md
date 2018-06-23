# Docker

This repo is used for testing basic docker functions. It requires docker to be installed on your local machine.

## Basic docker commands

```bash
# show list of all images downloaded on local machine
docker images

# search for images locally and on docker hub
docker search

# docker run [PARAMS] <image name>
# run nginx docker image (in this example)
docker run nginx

# show list of all 'active' images
docker ps -a
docker container ps -a

# show list of images currently running
docker ps

# remove docker image instance from active list
docker rm <instance name>
# OR
docker container rm

# remove all instances from list
docker rm `docker ps -a -q`

# start instance
docker start <container name or id>

# stop all instances currently running
docker stop `docker ps -q`

# remove image from local repo (machine)
docker rmi <image name or id >
# OR
docker image rm <image name or id>
# OR 
docker image prune
# run container in interactive mode and access bin/bash
# NOTE! after closing bash the image will deactivate/close itself

docker run -it nginx /bin/bash

# visit docker image and run bin/bash command without closing container
docker exec -it <container id> /bin/bash

# show me docker system info (images, volumes, networks etc)
docker system df

```

## Port forwarding nginx container

This command will start 2 nginx docker containerS and connect it to local server port 8080 and 8087.

```bash
  docker run -d -p 8080:80 --name=web-dev nginx
  docker run -d -p 8087:80 --name=web-api nginx
```

## Mounting host path into docker container images

Very powerful feature is mounting host directories into docker image. In the below example we start docker nginx image and mount website directory from this repo (ensure you have proper path on your machine) into nginx docker image. Using this feature we have content on parent machine and can make changes to a content without affecting content of docker image.

```bash
# run nginx image, in background, map image port 80 to 8080 on out host,
# name instance as nginx1, mount host location website folder into
# default nginx html folder

docker run -d -p 8080:80 --name=nginx1 -v /home/<user>/docker/website:usr/share/nginx/html nginx

```

## Dockerfile

For building custom docker images we use docker file. See docker files in this repo for various examples

Basic command to build docker image from docker file
```bash
# execute in folder where docker file is present
docker build -t <image name> <docker file directory>

# check the layers docker image is build of
docker image history <image name>

# read image metadata
docker image inspect <image name>

```
### Tips
When constructing docker file CHAIN RUN commands. This will create only one layer in docker file and will fail if any of command fails. When using install commands append -y or -qq to avoid interactive mode when you need to answer installation question. Example

```docker

RUN apt-get update && apt-get upgrade -y && apt-get install -y openssh-server

```

### Dockerfile syntax


```docker
# starting with image
# pull basic debian stable image from docker hub
FROM debian:stable

# information about MAINTAINER name and email
MAINTAINER dv4all <info@dv4all.com>

# RUN command, chaining command is advised because it represents one single container step. Also use -y or -qq to bypass question during install (interactive mode)
RUN apt-get update && apt-get upgrade -y && apt-get install -y apache2

# create environment variables to be embedded into created image
# if you need variables to pass in - THIS IS THE WAY to do it
# example: create TEST_VAL environment variable loaded with "this is the value"
ENV TEST_VAL "this is the value"

# Exposing ports to external world
# in this example we expose port 80 to external world so we can map it to host 8080 port when running the container
EXPOSE 80

# Execute specific command when image is initialized
# Commands are stored in an array. Params are separate item of an array
# start apache server
CMD ["/usr/sbin/apache2ctl","-D","FOREGROUND"]

```

## Docker virtual networks

By default all containers running in virtual Docker network. The default docker network is called bridge. Generally it is beter to create your own docker network to cluster apps. Containers running within same virtual network can communicate between eachother. And generally you will open only ports for the parts of the app that really need to be exposed to external worls (example: webserver op port 80)

```bash

  #help
  docker network --help

  #list docker virtual networks
  docker network ls

  #inspect docket network
  docker network inspect <network-name>

  #create new docker (virtual) network
  docker network create <network-name>

  # attach network to docker container
  docker network connect <container-name>

  # detach
  docker network disconnect <container-name>

```

## Docker hub

You can have free account at docker hub. When using docker cli you first need to login. After you logged in json config is created for cli. When using 'temp' machine use logout to remove this config file. Public private repos can be defined from webapp.

```
  # login
    docker login
  # logout
    docker logout
```

## Persistent data

Containers are immutable. Ideally data should be separated from docker container and saved outside of the container. When container is removed all data within container will be lost. In addition we want to multiple containers to use same persistent data source.
NOTE! It is not possible to have 2 containers using same db volume at the same time, it is more about persising data and restarting/upgrading containers.

There are two solutions: Volumes and BindMounts
- Volumes: make special location outside of container.
- Bind Mounts: link container path to host path 

In both cases container does not distinct between local and mounted volumes.

### Volumes

```bash
  # create mysql container with named data volume mysql-db persisted on host machine
  # this way the containers will use same persisten data volume
  # note! by default each container will create its own random volume and data will be scattered. 

  docker container run -d --name mysql -e MYSQL_ALLOW_EMPTY_PASSWORD=True -v mysql-db:/var/lib/mysql mysql

  # list data volumes
  docker volume ls

  # inspect data volumes - Mountpoint shows where is data stored on local machine
  docker inspect <volume id>

```

Specifying volumes in Dockerfile

```Dockerfile
  # volume name is volume defined in docker
  VOLUME <volume-name>:/var/lib/mysql


```

### Bind Mounting

It can be used only at run time (using cli). It can be placed in Dockerfile. It supports complete file path, which means it can be anywhere on the local disk. This approach is very usefull for development environment so developer can use local file mounted into docker image and change the content without changin docker image (for example website content).

```bash
  #mount current local directory into mysql location in the container
  docker run ... -v $(pwd):/var/lib/mysql

```


# Docker compose

Compose YAML file has its versions. There is also docker-compose cli. Since 2017 uses version 3. Version 2 is also widely used. For compsing check compose-assigment-2

```bash
  #run docker-compose yml 
  docker-compose up

  # stop 
  docker-compose down

```

# Docker swarm

Enables creating complicated container orchestration. With docker command we can create only one container. Swarm manager uses `docker service create`. We have Manager and Worker nodes.
When using swarm use `docker service create` instead of `docker run`  

Using docker service update we are able to replace/scale nodes with ZERO downtime. If service is removed, even with docker rm, it will spawn new container to replace one that is removed.


```bash
  # check if swarm is activated - it is not by default
  docker info

  # initialize swarm mode
  docker swarm init

  # list node managers
  docker node ls

  # help
  docker node --help

  # create alpine container and ping google DNS server
  docker service create alpine ping 8.8.8.8

  # list swarm services running
  docker service ls

  # list task running in the service
  docker service ps <service-name>

  # scale up service to 3 replicas/nodes
  docker service update <service-name OR id> --replicas 3

  # remove swarm service
  docker service rm <service-name>

```

## Docker stack
Since version 3 YAML doker swarm supports stack commands. Stack is higher level of abstraction of some services grouped together. See swarm-stack-1 sample.

## Docker secrets (passwords & api keys)
If you want to use secrets in docker-compose file you need to indicate version: 3.1. See secrets-sample-2. Secrets can be stored in files or in docker secrets database.

```bash
  # create secret in docker secret db (swarm required)
  echo "mySecretPasswordOrKey" | docker secret create <secret-name> - <value>

```

To use secrets in local development, without swarm, use file based secrets. This are only support with docker-compose.


# Docker HEALTHCHECKS

Sincs version 2.1 YAML file, Docker supports healtchecks. The interval is added in version 3.4

Using docker cli, it can also be docker compose or docker (swarm) service

```bash
  # run postgres container incl. healtcheck with build-in postgres healthcheck command
  # exit 1 or false with || construction indicates unhealthy flag
  docker container run --name p1 -d --health-cmd="pg_isready -U postgres || exit 1" postgres

  # run docker swarm
  docker service create --name p1 -d --health-cmd="pg_isready -U postgres || exit 1" --replicas 3 postgres

```

```yml
  # check localhost webserver is running every 90s,
  # alowed timeout is 10s, retry 3 times before conclusion
  # alow for start period of 40 seconds
  healthcheck:
    test: ["CMD", "curl","-f","http://localhost"],
    interval: 1m30s
    timeout: 10s
    retries: 3
    start_period: 40s

```

# Docker registry

You can run docker registry on your local machine/server. It enables you to store docker images locally, so you are not depending on docker hub.

You can use registry in the swarm, but all nodes need to be able to access registry images. When registry is running in the swarm as docker container all nodes will be able to access the image due to docker network mash.

```bash
  # run docker registry locally as container on port 5000
  # save images in current dir/registry-data folder
  docker container run -d -p 5000:5000 --name registry -v $(pwd)/registry-data:var/lib/registry registry

  # re-tag existing image
  docker tag hello-world 127.0.0.1:5000/hello-world
  # push image to local registry
  docker push 127.0.0.1:5000/hello-world
  # pull image from local registry
  docker pull 127.0.0.1:5000/hello-worl

```