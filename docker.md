# Common Docker Command Cheatsheet

A list of common commands for working with Docker. 

**_NOTE_** to process JSON it might be helpful to have **_JQ_** installed on your system (`brew install jq`). **_JQ_** is a lightweight and flexible command-line JSON processor. More details can be found here [https://stedolan.github.io/jq/](https://stedolan.github.io/jq/)

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

Note, be careful using this as deleting the `amazon/amazon-ecs-agent:latest` container
 means the ECS service will no longer run. If this does happen, simply recreate the
 service in the ECS cluster setup.

#### Explore container by container ID     (very handy for viewing filesystem of container)ls
`docker exec -t -i container-id bash`

## Images

#### Delete all images
`docker rmi $(docker images -q)`

#### Delete all dangling images
`docker rmi $(docker images -q -f dangling=true)`

#### Delete all untagged, unnamed, dangling images
`docker images -q --no-trunc -f dangling=true | xargs docker rmi`

#### Explore image by image name
`docker inspect IMAGE [IMAGE NAME HERE]`

## Volumes

#### Delete all dangling volumes
`docker volume rm $(docker volume ls -q -f dangling=true)`

#### View mounted volumes on a machine
`docker inspect -f {{.Mounts}} container-id`

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
