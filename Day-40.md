# Day 40: Docker EXEC Operations

## Execute the docker command
```
docker exec -it kkloud bash
```

## Install apache
```
apt update -y
apt install apache2 -y
```

## Edit the port
```
vi /etc/apache2/ports.conf
vi /etc/apache2/sites-available/000-default.conf
apache2ctl configtest
```

## Start the service
```
service apache2 start
```

