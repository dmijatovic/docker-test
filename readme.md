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

# show list of images currently running
docker ps

# remove docker image instance from active list
docker rm <instance name>

# remove all instances from list
docker rm `docker ps -a -q`

# start instance
docker start <container name or id>

# stop all instances currently running
docker stop `docker ps -q`

# remove image from local repo (machine)
docker rmi <image name or id >

# run container in interactive mode and access bin/bash
# NOTE! after closing bash the image will deactivate/close itself

docker run -it nginx /bin/bash

# visit docker image and run bin/bash command without closing container
docker exec -it <container id> /bin/bash

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

```
### Tips
When constructing docker file CHAIN RUN commands. This will create only one layer in docker file and will fail if any of command fails. When using install commands append -y or -qq to avoid interactive mode when you need to answer installation question. Example

```docker

RUN apt-get update && apt-get upgrade -y && apt-get install -y openssh-server

```

### Docker syntax
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
