# Common Docker Command Cheatsheet

A list of common commands for working with Docker. Ensure you have **_JQ_**
installed on your system (`brew install jq`).

## Containers

#### List running docker containers:

`docker ps`

#### List all docker containers:

`docker ps -a`

#### Stop all containers
`docker stop $(docker ps -a -q)`

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
`docker inspect IMAGE [IMAGE...]`

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
