# Common Docker Command Cheatsheet

A list of common commands for working with Docker. 

**NOTE:** to process JSON it might be helpful to have **JQ** installed on your system (`brew install jq`). **JQ** is a lightweight and flexible command-line JSON processor. More details can be found here [https://stedolan.github.io/jq/](https://stedolan.github.io/jq/)

## Containers

#### Start a container with port mapping

`docker run -p  [HOST PORT HERE]:[INTERNAL CONTAINER PORT HERE] [IMAGE NAME HERE]`

#### List running docker containers:

`docker ps`

#### List all docker containers:

`docker ps -a`

#### Stop all containers
`docker stop $(docker ps -a -q)`

#### Stop a runing container by the image name

`docker stop $(docker ps --filter status=running --filter ancestor=[IMAGE NAME HERE] --format "{{.ID}}")`

#### Delete all stopped containers
`docker rm $(docker ps -a -q)`

#### Delete all exited containers
`docker ps -aq -f status=exited | xargs docker rm`

#### Delete all containers
`docker rm  -f $(docker ps -a -q)`

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

## Volumes

#### Delete all dangling volumes
`docker volume rm $(docker volume ls -q -f dangling=true)`

#### View mounted volumes on a machine
`docker inspect -f {{.Mounts}} container-id`

## Exploring Images and Running Containers

#### Explore container by container ID     (very handy for viewing filesystem of container)ls
`docker exec -t -i container-id bash`

#### Explore image by image name
`docker inspect IMAGE [IMAGE NAME HERE]`

#### Start Container with Interactive Shell (Non-Dameon)
`docker run -i -t --rm ubuntu /bin/bash`  
NOTE: This will exit the container after the session ends.

#### Start Container with Interactive Shell (Dameon) 
`docker run --name daemon -d ubuntu /bin/sh -c "while true; do echo hello world; sleep 1; done"`  
Run as a daemon to keep the container running after the end of the session.

## Misc

#### Clean up the local machine
`docker system prune`

#### Clean up the server machine
```
docker images -q --no-trunc -f dangling=true | xargs docker rmi
docker volume rm $(docker volume ls -q -f dangling=true)
docker rm $(docker ps -a -q)
docker ps -aq -f status=exited | xargs docker rm
```

#### View logs for a container
`docker logs container-id`
