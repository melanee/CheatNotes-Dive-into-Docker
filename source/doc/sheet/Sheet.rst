##################
 Dive into docker
##################

.. _Image:

**************************
 Docker image management!
**************************
Images
  Are build, pull or run.

Image
  Is a file specification of it's content. 

Building
  Takes the specification of an image and package it as per the docker standard.

Dockerfile
  Written line by lines sequentially  is a specificaton, each actionable step become a seperate layer in the Docker image.

  When building an image only the self contain layer that have changed are updated.

**Writting the Dokerfile specification**::

     FROM <base-image>[:tag]
          ex. FROM python:3.8-alpine

     RUN <run as inside container base-image (script.shell,linux)  command>
          ex. RUN mkdir /app 
     WORKDIR <set a contextual path from this point on to refered  to>
          ex. WORKDIR /app

     COPY <source within the Dockerfile path or sub-path> <destination inside the base-image>
          ex. COPY requirements.txt requirements.txt
     RUN <install dependencies>
          ex. RUN pip install -r requirements.txt

     COPY <. for recursively from Dockerfile source path> <. for relative to WORKDIR path inside image>
          ex.  COPY . .

     LABEL <key="value"> <...> ...
          ex.  LABEL maintainer="Melanee Hannah <melanee@tutanota.de>" \
                     version="1.0"

     CMD <run command when docker image is started as opposed to when it is build>
          ex. CMD flask run --host=0.0.0.0 --port=5000


Image Management Command
========================

Image command listing images
----------------------------

``docker image ls``
::

   docker image ls 


Image command build
-------------------

``docker image build [<option>] [<image-id>[:<version>]] <path>``
::

   docker image build -t web1:1.0 . 
     
     :build:Parsing and building the Dockerfile
     :-t:tag iname of image
     :image-id:Randowm hash or name of the tag for image if option -t 
     :path:ibuild the current directory 

Image command inspect
---------------------

``docker image inspect <image-id>``
::

   docker inspect web1

    :inspect:show a json dump about the image 
      
Image command delete
--------------------

``docker image rm [-f] <image-id>|<repository>[<:tagname>]``
::

   docker image rm web1:1.0

   docker image rm -f 1d08

    : -f:force will insure all image deletes
    :image-id:Found by `docker image ls` use 4 first digit of IMAGE ID field

Image command tagging username
------------------------------

``docker image tag <source> <username>/<repository>:<tagname>|[latest]``
::

   docker image tag web1 melanee/web1:latest

Image command login in Docker Hub
---------------------------------
::

   docker login

Image command pushing to Docker Hub
-----------------------------------

``docker image push <username>/<repository>:<tagname>|[latest]``
::

   docker image push melanee/web1:latest 

Image command pulling from Docker Hub
-------------------------------------

``docker image pull <username>/<repository>:<tagname>``
::

   docker pull melanee/web1:latest

Pulling
  The deamon (client) tries localy to find the image else download it from the docker hub repository.

Pulling an image from the repository at the docker hub registry
---------------------------------------------------------------

``docker run [[<url>/]<name space>/]image``
::

   docker run docker.io/library/python:3.8-alpine

    :url: Url of the registry or docker.io or default Docker Hub Registry 
    :name space: User name or library for default Docker Hub Registry 
    :image: The image name of the Repsitory hence the Image


Docker Hub
  Where you find official images and can publish your non-official images privatly or publicly.

* Is a  Docker Registry.
* Docker Registry contains Docker Repository. 
* Docker Repository is a version controle of an image.

  * Has a collection of Docker images all with the same name.

* Docker image the image

  * Has a Tag that identify the image

* Tag is a the version id  of the image. Default to a tag named latest.


Running
  Run create a running instance of an image as a component of a container.

 
.. _Container:

******************************
 Docker container management!
******************************
A running instance of a image is called a container. Container are immutable and independant of each other.

Container Management Command
============================

Container command listing containers
------------------------------------

``docker container ls [-a]``
::

   docker container ls -a

    :-a:without list active containers with -a list all active and non-active containers

Conainer command running a instance of an image
-----------------------------------------------

``docker containr run -it -p [<bind port on docker host>:]<bind port in the container> \``
     ``[--name <container-name>] [-e <variable=value> ...] [-d] [--rm|--restart on-failure] <image>``

::

   docker container run -it -p 5000:5000  --name web1 -e FLASK_APP=app.py --rm web1
   docker container run -it -p 5000 --name web1_2 -e FLASK_APP=app.py -d --restart on-failure web1

     :-it:itractive terminal
     :-p:Binds random host port if non exitant or specified host port to container port  
     :--name:custom container-name 
     :-e:environment variable 
     :-d:run as a deamon return a container-id
     :--rm:remove container when stopped 
     :--restart on-failure: mutualy exclusive to --rm 

Container command deleting container
------------------------------------

``docker container rm <container-id>|<container-name>``
::

   docker container rm hardcore_dijkstra

     :container-id:Found by `docker container ls -a` use 4 first digit of CONTAINER ID field

Container command logs
----------------------

``docker container logs [-f] <container-id>|<container-name>``
::

   docker logs -f web1

      :-f:Foreground when present and single step if non existant


Container command stop
----------------------

``docker container stop <container-id>|<container-name>``
::

   docker stop  web1
      
Container debuging using live code reloading with volumes
=========================================================

   In this course we use FLASK Framework for debuging we need to activate the debug while running  the container so we use  another flag environment FLASK_DEBUG=1
   We also use volume to link  a path of local storage to a path inside docker image already defined in Dockerfile (/app)

Container flag volume use in debug
----------------------------------

``docker containr run -it -p [<bind port on docker host>:]<bind port in the container> \``
     ``[--name <container-name>] [-e <variable=value> ...] -v <local-path>:<image-path> --rm <image>``

::

   docker container run -it -p 5000:5000 -name web1 -e FLASK_APP=app.py \
     -e FLASK_DEBUG=1 -v $PWD:/app --rm web1

      :-it: itractive terminal
      :-p:Binds random host port if non exitant or specified host port to container port  
      :--name:custom container-name 
      :-e:environment variable 
      :-v:local-path:image-path

Container running process inside the container
----------------------------------------------

``docker container exec -it --user <user:group> <image-name> <cmd>``
::

   docker container exec -it --user $(id -u):$(id -g) web1 sh

     :exec:run the cmd inside the container
     :-it: itractive terminal
     :--user:$(id -u)=current user on linux. $(id -g)=curent group of user.
     :cmd:linux command

Practical usage Running a image to learn languages
--------------------------------------------------
::

   docker container run -it --name testing-python --rm python:3.8-alpine python


*********
 Network
*********
   Docker use a default bridge which has a gateway IP address, where containers connect their internal interface which have default attributed IPs of the same default subnet.
   The default network use hash host-id as host-name making difficult for internetworking  because of the need to hard code the IP address of their remote host during communication.
   However the ability to create bridge has DNS implemented wich permits to use host-name during internetwroking communication.

  
**************************
 Docker network management
**************************

Network Management Command
==========================
Default bridge
   The default bridge have as device docker0, type ifconfig at the terminal to list the network devices.

Network command listing networks
--------------------------------

``docker network ls``
::

   docker network ls 


Network command inspect network
-------------------------------

``docker network inspect <networki-name>``
::

   docker network inspect bridge


Network command create bridge
-----------------------------
``docker network create --driver bridge <network-name>``
::

    docker network create --driver bridge firstnetwork

Network command add container to bridge       
---------------------------------------

``docker container run -itd -p [<bind port on docker host>:]<bind port in the container> \``
     ``[--name <container-name>] [-e <variable=value> ...] -v <local-path>:<image-path> --rm --net <network-name> <image>``

::

   docker container run -itd -p 6379:6379 -name redis \ 
     -v $PWD:/app --rm --net firstnetwork redis:3.2-alpine

   docker container run -itd -p 5000:5000 -name web2 -e FLASK_APP=app.py \
     -e FLASK_DEBUG=1 -v $PWD:/app --rm --net firstnetwork web2


Practical usage exec a process in a container to use network utilities
----------------------------------------------------------------------

``docker exec <container-name> <utility>``
::

   docker exec redis ifconfig

   docker exec redis ping 172.17.0.3 
     :172:17:0:3:Internal address of web2

   docker exec redis ping web2 


*************************
 Docker volume management
*************************

Volume Management Command
=========================

Name Volume
     A named volume makes docker manage a persistent volume on the host, making the image no more stateless therefore not scalable.

Volume command and data management
----------------------------------

``docker volume create <volume-name>``
::

   docker volume create web2_redis

   docker volume ls

   docker volume inspect web2_redis


``docker container run -itd -p [<bind port on docker host>:]<bind port in the container> \``
     ``[--name <container-name>] [-e <variable=value> ...] -v <named-volume>:<image-path> --rm --net <network-name> <image>``

::

   docker container run -itd -p 6379:6379 --name redis --rm --net firstnetwork \
     -v web2_redis:/data redis:3.2-alpine

      :named-volume:Volume created for persistence on the host
      :image-path:volume inside the container provide by resis README on Docker Hub


Sharring data between containers
--------------------------------

Modifying Dockerfile to share /app/public volume between containers::

   VOLUME ["/app/public"] 

Building the new image::

   docker image build -t web2 .

Or without modifying the Dockerfile:

``docker container run -itd -p [<bind port on docker host>:]<bind port in the container> \``
     ``[--name <container-name>] [-e <variable=value> ...] -v <local-path>:<image-path> -v <source-name-volume> --rm --net <network-name> <image>``
 
::

   docker container run -itd -p 5000:5000 --name web2 \
      -e FLASK_APP=app.py -e FLASK_DEBUG=1 -v $PWD:/app -rm -net firstnetwork web2

   docker container run -itd -p 5000:5000 --name web2 \
      -e FLASK_APP=app.py -e FLASK_DEBUG=1 -v $PWD:/app -v /app/public -rm -net firstnetwork web2

Then in both case: 

``docker container run -itd -p [<bind port on docker host>:]<bind port in the container> \``
     ``[--name <container-name>] [-e <variable=value> ...] -v <named-volume>:<image-path> --volumes-from <remote-name-container> --rm --net <network-name> <image>``

::

   docker container run -itd -p 6379:6379 --name redis --rm --net firstnetwork \
   -v web2_redis:/data --volumes-from web2 redis:3.2-alpine

*************************
Running ENTRYPOINT SCRIPT
*************************

Entrypoint sctipt
  The docker entrypoint script customized each containers running the image.

Pre requesit
============

* Docker-entrypoint script in the local-path.
* Modified updated Dockerfile

Docker docker-entrypoint.sh script
----------------------------------

::

    #!/bin/sh
    set -e

    echo "The Dockerfile ENTRYPOINT has been executed!"

    export WEB2_COUNTER_MSG="${WEB2_COUNTER_MSG:-carbon based life forms have sensed this website}"

    exec "$@"
 

      :WEB2_COUNTER_MSG: Use by app.py part of business logic code 
      :exec "$@":Execute the script code including any custom arguments \
          pass in the CMD line of the Dockerfile and  environment variables. It is executing  /bin/sh -c "$ &&  ...

Docker updated Dockerfile
-------------------------

::

    COPY docker-entrypoint.sh /
    RUN chmod +x /docker-entrypoint.sh
    ENTRYPOINT ["docker-entrypoint.sh"]

      :COPY:local-file to inside container path. Using in this case root-path
      :ENTRYPOINT:Dockerfile command; local-entrypoint-script

Docker entrypoint command
-------------------------

``docker container run -itd -p [<bind port on docker host>:]<bind port in the container> \``
     ``[--name <container-name>] [-e <variable=value> ...] -e custom environement ariable ... --rm --net <network-name> <image>``

::

    docker container run -it -p 5000:5000 -e FLASK_APP=app.py \
       -e WEB2_COUNTER_MSG="Docker fans have visited this web site" \
        -e FLASK_DEBUG=1 --name webentrypoint  --rm --net firstnetwork webentrypoint


********************
Using Docker Compose
********************

Adding docker-compose.yml and env
=================================

Docker Compose
  Use directives in a yaml file to run containers 

docker-compose.yml
------------------
| version: <"value">
|    Service-directives:
|      Service-name:
|      image: <"image-name"> | build: <"host-path">
|      [depends_on-list:
|        - <"Remote-Service-name">]
|      [env-file-list:
|        - "env-file"]
|      ports-list:
|        - <"local-port:image-port">
|      volumes-list:
|        - <"name-volume:image-path|local-path:image-path">
|
|  volume-directive:
|    ref-to-name-volume-of-service: {[options]}    

::

    version: "3"

    services:
      redis:
        image: "redis:3.2-alpine"
        ports:
          - "6379:6379"
        volumes:
          - "redis:/data"  


      web:
        build: .
        depends_on:
          - "redis"
        env_file:
          - ".env"
        #images: "melanee/web:1.0"
        ports:
          - "5000:5000"
        volumes:
          -   ".:/app"

    volumes:
      redis: {}

env-file
--------
``COMPOSE_PROJECT_NAME=<project-name>``

``key=<value>``

``...``

::

    COMPOSE_PROJECT_NAME=web2

    PYTHONBUFFERED=true
    FLASK_APP=app.py
    FLASK_DEBUG=1


