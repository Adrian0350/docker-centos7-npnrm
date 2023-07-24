# Centos7 server with NGINX, PHP, NodeJS, Redis & MySQL

This docker container has been build with CentOS7, NGINX 1.x, PHP 7.1, NodeJS 10.23.2, NPM 3.x, Redis and MySQL 5.7. Additionally I loaded Composer 2.x, Grunt 1.x, Wkhtmltopdf and Gearman daemon, which I need for most of my apps.

There is two versions of this docker that I called Server and Pipeline.

## Server Image

The goal of this one is to be an executable fully loaded with the services. These will be managed with PM2, which will be the main process keeping alive the image.

```
docker pull linnk/centos7-npnrm
```

**NOTE**: npnrm (node, php, nginx, redis, mysql)

## Pipeline Image

This one is going to be a lite version (kind of) of the server image. The objective is to be used in bitbucket pipelines for coding style checking and testing purposes, so this one will have only the necessary to run my tests and style checkers.

```
docker pull linnk/centos7-np-pipeline
```

**NOTE**: np (node, php)

## Building Image

Instructions to build the image yourself:

```
docker build -t docker-centos7-npnrm .
docker run --name centos7-npnrm -e MYSQL_ROOT_PASSWORD=root -d centos7-npnrm
```

Executing bash in the server image:

```
docker exec -i -t centos7-npnrm /bin/bash
```

Stopping and clearing:

```
docker stop centos7-npnrm
docker rm centos7-npnrm
```

## Pushing Image to Docker Hub

```
# There is an image with PHP7.1, for PHP7.4 use the tag "php7.4"
docker push adrian0350/incrementacrm-pipelines:php74 
```


Docker containers are fun. :)
