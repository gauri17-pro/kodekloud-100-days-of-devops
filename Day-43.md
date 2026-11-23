# Day 43: Docker Ports Mapping

## Map host port 8083 to container port 80
```
docker pull nginx:stable
docker run -itd -p 8083:80 --name cluster nginx:stable
```
