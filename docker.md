# Common Docker Command Cheatsheet

A list of common commands for working with Docker. 

**NOTE:** to process JSON it might be helpful to have **JQ** installed on your system (`brew install jq`). **JQ** is a lightweight and flexible command-line JSON processor. More details can be found here [https://stedolan.github.io/jq/](https://stedolan.github.io/jq/)

## Containers

#### Start a container and mount local drive

`docker run -v [LOCAL PATH]:[CONTAINER PATH]`

#### Start a container with port mapping

`docker run -p  [HOST PORT HERE]:[INTERNAL CONTAINER PORT HERE] [IMAGE NAME HERE]`

#### Start a container with port mapping and custom container name

`docker run -p  [HOST PORT HERE]:[INTERNAL CONTAINER PORT HERE] --name=[CUSTOM CONTAINER NAME HERE] [IMAGE NAME HERE]`

#### Start a container with an interactive shell. 

`docker run -i -t [IMAGE NAME HERE]`

This is especially handy for running OS images. i.e. Centos, etc.

#### List running docker containers:

`docker ps`

#### List all docker containers:

`docker ps -a`

#### Stop all containers
`docker stop $(docker ps -a -q)`

#### Stop a running container by the image name

`docker stop $(docker ps --filter status=running --filter ancestor=[IMAGE NAME HERE] --format "{{.ID}}")`

#### Stop a running container by the container name

`docker stop $(docker ps --filter status=running --filter name=[IMAGE NAME HERE] --format "{{.ID}}")`

#### Delete all stopped containers
`docker rm $(docker ps -a -q)`

#### Delete exited container by name
`docker rm $(docker ps --filter status=exited --filter name=[IMAGE NAME HERE] --format "{{.ID}}")`

#### Delete all exited containers
`docker ps -aq -f status=exited | xargs docker rm`

#### Delete all containers
`docker rm  -f $(docker ps -a -q)`

#### View the Docker file commands
`docker image history --no-trunc <image_name>`
Useful to try and view the dockerfile for an image when you can't find the dockerfile.

**NOTE:** when using AWS ECS be careful using this as deleting the `amazon/amazon-ecs-agent:latest` 
container means the ECS service will no longer run. If this does happen, simply recreate the service 
in the ECS cluster setup.

## Images

#### Show all images
`docker images`

#### Delete all images
`docker rmi $(docker images -q)`

`docker rmi -f $(docker images -q)` (with force option)

#### Delete all dangling images
`docker rmi $(docker images -q -f dangling=true)`

#### Delete all untagged, unnamed, dangling images
`docker images -q --no-trunc -f dangling=true | xargs docker rmi`

#### Clean up the images
`docker image prune`

## Volumes

#### Delete all dangling volumes
`docker volume rm $(docker volume ls -q -f dangling=true)`

#### View mounted volumes on a machine
`docker inspect -f {{.Mounts}} container-id`

## Exploring Images and Running Containers

#### Explore running container by container ID     
`docker ps -a` (list container ids)  
`docker exec -t -i container-id ash`

#### Explore image by image name
`docker inspect IMAGE [IMAGE NAME HERE]`

#### Start Container with Interactive Shell (Non-Dameon)
`docker run -i -t --rm [IMAGE ID HERE] ash`  
NOTE: This will exit the container after the session ends.

#### Start Container with Interactive Shell (Dameon) 
`docker run --name daemon -d [IMAGE ID/NAME HERE] /bin/sh -c "while true; do echo hello world; sleep 1; done"`  
Run as a daemon to keep the container running after the end of the session.

## Misc

#### Clean up the local machine
`docker system prune`

#### Clean up the build cache
`docker builder prune`

#### Clean up the server machine
```
docker images -q --no-trunc -f dangling=true | xargs docker rmi
docker volume rm $(docker volume ls -q -f dangling=true)
docker rm $(docker ps -a -q)
docker ps -aq -f status=exited | xargs docker rm
```

#### View logs for a container
`docker logs container-id`

#### View stats for a container
`docker stats container-id`

#### Command to ensure container does not exit upon startup
`CMD exec /bin/sh -c "trap : TERM INT; (while true; do sleep 1000; done) & wait"`  
Add this to the end of your Dockerfile to when developing your to help with debugging.
