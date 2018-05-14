# bytecoin-docker
This image use ubuntu 16.04 to run `bytecoin` daemon

### How to run the container
```
docker run -d \
    -e TZ=Europe/Berlin \
    -v ~/.bytecoin:/root/.bytecoin \
    --restart unless-stopped \
    -p 8080:8080 \
    -p 8081:8081 \
    --name bytecoin-fullnode \
    rafalsladek/bytecoin-docker
```

### How to see logs of running the container
```
docker logs bytecoin-fullnode --follow
```

### How to attach to running the container
```
docker attach bytecoin-fullnode
```

### Docker - How to cleanup (unused) resources

Once in a while, you may need to cleanup resources (containers, volumes, images, networks) ...
    
### delete volumes
    
// see: https://github.com/chadoe/docker-cleanup-volumes
```
docker volume rm $(docker volume ls -qf dangling=true)
docker volume ls -qf dangling=true | xargs -r docker volume rm
```
### delete networks
```
docker network ls  
docker network ls | grep "bridge"   
docker network rm $(docker network ls | grep "bridge" | awk '/ / { print $1 }')
``` 
### remove docker images
    
// see: http://stackoverflow.com/questions/32723111/how-to-remove-old-and-unused-docker-images
```
docker images
docker rmi $(docker images --filter "dangling=true" -q --no-trunc)

docker images | grep "none"
docker rmi $(docker images | grep "none" | awk '/ / { print $3 }')
```
### remove docker containers

// see: http://stackoverflow.com/questions/32723111/how-to-remove-old-and-unused-docker-images
```
docker ps
docker ps -a
docker rm $(docker ps -qa --no-trunc --filter "status=exited")
```
### Resize disk space for docker vm
```
docker-machine create --driver virtualbox --virtualbox-disk-size "40000" default
```