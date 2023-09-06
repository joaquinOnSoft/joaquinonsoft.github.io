---
title: "Docker Cheat Sheet"
header:
  image: /images/2023-09-05-docker-cheat-sheet/docker.png
  og_image: /images/2023-09-05-docker-cheat-sheet/docker.png
tags:
  - Docker
last_modified_at: 2023-09-05T20:27:15-04:00
---


This is a text-based version of the [Cheatsheet for Docker CLI](https://dockerlabs.collabnix.com/docker/cheatsheet/) [image](https://raw.githubusercontent.com/sangam14/dockercheatsheets/master/dockercheatsheet8.png).

## Run a new Container 

Start a new Container from an Image

```
docker run IMAGE 
docker run nginx
```

... and assign it a name

```
docker run --name CONTAINER IMAGE 
docker run --name web nginx
```

... and map a port

```
docker run -p HOSTPORT:CONTAINERPORT IMAGE 
docker run -p 8080:80 nginx
```

... and map all ports

```
docker run -P IMAGE 
docker run -P nginx
```

... and start container in background

```
docker run -d IMAGE
docker run -d nginx
```

... and assign it a hostname

```
docker run --hostname HOSTNAME IMAGE 
docker run --hostname srv nginx
```

... and add a DNS entry

```
docker run --add-host HOSTNAME:IP IMAGE 
```

... and map a local directory into the container

```
docker run -v HOSTDIR:TARGETDIR IMAGE
docker run -v ~/:/usr/share/nginx/html nginx 
```

... but change the entrypoint

```
docker run -it --entrypoint EXECUTABLE IMAGE
docker run -it --entrypoint bash nginx
```

If you want to exit the container's interactive shell session, but do not want to interrupt the processes running in it, press `Ctrl+P` followed by `Ctrl+Q`.


## Manage Containers 

### Show a list of running containers

```
docker ps
```

### Show a list of all containers

```
docker ps -a
```

### Delete a running container

```
docker rm -f CONTAINER
docker rm -f web
```

### Delete stopped containers

```
docker container prune
```

### Stop a running container

```
docker stop CONTAINER
docker stop web
```

### Start a stopped container

```
docker start CONTAINER
docker start web
```

### Copy a file from the container to the host

```
docker cp CONTAINER:SOURCE TARGET
docker cp web:./index.html index.html
```

### Copy a file from the host to the container

```
docker cp TARGET CONTAINER:SOURCE 
docker cp index.html web:./index.html 
```

### Start a shell inside a running container

```
docker exec -it CONTAINER EXECUTABLE
docker exec -it web bash
```

### Rename a container

```
docker rename OLD_NAME NEW_NAME
docker rename 096 web
```

### Create an image out of a container

```
docker commit CONTAINER
docker commit web
```




## Manage Images 

### Download an Image

``` 
docker pull IMAGE[:TAG] 
docker pull nginx
docker pull ubuntu:23.10
```

### Upload an image to a repository 

``` 
docker push IMAGE[:TAG] 
docker push myimage:1.0
```

### Delete an image

``` 
docker rmi IMAGE
```

### Show a list of all images

``` 
docker images
```

### Delete dangling images

``` 
docker image prune
```

### Delete all unused images

``` 
docker image prune -a
```

### Built an image from a Dockerfile

``` 
docker build DIRECTORY
docker build .
```

### Tag an image

``` 
docker tag IMAGE NEW_IMAGE
docker tag ubuntu ubuntu:23.10
```

### Build and tag and image grom a Dockerfile

``` 
docker build -t IMAGE DIRECTORY
docker build -t myimage .
```

### Save an image to .tar file

``` 
docker save IMAGE > TARFILE
docker save nginx > nginx.tar
```

### Load an image from a .tar file

``` 
docker load -i TARFILE
docker load -i nginx.tar
```


## Info & Stats

### Show the logs of a container

``` 
docker logs CONTAINER 
docker logs web
```

### Show stars of running containers

``` 
docker stats
```

### Show processes of running containers

``` 
docker top CONTAINER
docker top web
```

### Show installed docker version

``` 
docker version
```

### Get detailed info about and object

``` 
docker inspect NAME
docker inspect nginx
```
		 
### Show all modified files in container

``` 
docker diff CONTAINER
docker diff web
```
		 		 
## Show mapped ports of a container

``` 
docker port CONTAINER
docker port web
```				 