# Docker

This repo is used for testing basic docker functions.

## Baic management commands

```bash
# show list of all images downloaded on local machine
docker images 

# show list of all 'used' images
docker ps -a

# show list of images currently running
docker ps

# remove docker image instance from active list
docker rm <instance name>

# remove all instances from list
docker rm `docker ps -a -q`

# stop all instances currently running
docker stop `docker ps -q`

# remove image from local repo (machine)
docker rmi <<image name or id >>

```

## Port forwarding nginx container

This command will start 2 nginx docker containerS and connect it to local server port 8080 and 8087.

```bash
  docker run -d -p 8080:80 --name=web-dev nginx
  docker run -d -p 8087:80 --name=web-api nginx
```

## Dockerfile

For building custom docker images we use docker file. 
