# Docker implementation of weewx in simulator mode

This small set of files lets you spin up a working Docker instance of weewx on top of minimal debian within just a minute or two, depending on your bandwidth.

Yes, I called the resulting image 'weebian' :-)

## how to build and use

* strongly suggest just using docker-compose to build and start it
* the yaml file above here:
    * starts this container
    * exposes its public_html and archive directories to directories on the host
    * starts a vanilla nginx container that uses this public_html directory as its docroot

## low-level Docker details, if you don't use docker-compose

### build instructions

    make a working directory
    cd into there
    copy the Dockerfile and supervisord.conf files into that directory

#### build the image with the desired image tag name
    docker build -t weebian .

#### verify your image is available in docker

    $ docker images
    REPOSITORY          TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
    weebian             latest              62ae86bf4f67        14 hours ago        323.6 MB
    debian              latest              4d6ce913b130        4 days ago          84.98 MB
 
#### run the image in a new container
    $ docker run -d -p 80 -p 22 weebian
    00a256c24b2b97342fc8a5768e1aeab6cfa6d17df159550e738319dded6c8931
    $ docker ps -a
    CONTAINER ID        IMAGE               COMMAND                CREATED             STATUS              PORTS                                          NAMES
    00a256c24b2b        weebian:latest      "/usr/bin/supervisor   9 seconds ago       Up 8 seconds       
    0.0.0.0:49228->22/tcp, 0.0.0.0:49229->80/tcp   drunk_jang

#### to expose ports to the local host
    $ docker run -d -p 8001:80 -p 2201:22 weebian
    (this exposes container port 80 to localhost:8001, and container port 22 to localhost 2201)

