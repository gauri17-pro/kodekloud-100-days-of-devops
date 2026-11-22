# Day 41: Write a Docker File

## Write a Dockerfile for Apache

```
FROM ubuntu:24.04

RUN apt-get update && apt-get install -y apache2

RUN sed -i 's/Listen 80/Listen 8082/' /etc/apache2/ports.conf

EXPOSE 8082

CMD ["apachectl", "-D", "FOREGROUND"]

```
