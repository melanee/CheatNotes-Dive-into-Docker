##################
 Dive into docker
##################

.. _Image:

**************************
 Docker image management!
**************************
Images
  Images are build, pull or run.

*Image*
  Is a file specification of it's content. 

Building
========
  Takes the specification of an image and package it as per the docker standard.

Dockerfile
  Read line by lines the specificaton sequentially each actionable step become a seperate layer in the Docker image.

When building an image only the self contain layer that have changed are updated.

Writting the Dokerfile specification::

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

     COPY <. for recursively from Dockerfile path> <. for relative to WORKDIR path>
          ex.  COPY . .

     LABEL <key="value"> <...> ...
          ex.  LABEL maintainer="Melanee Hannah <melanee@tutanota.de>" \
     version="1.0"

     CMD <run command when docker image is started as opposed to when it is build>
          ex. CMD flask run --host=0.0.0.0 --port=5000


Image building command
----------------------

Image command build::

     docker image build [<option>] [<image-id>[:<version>]] <path>
          ex. docker image build -t web1:1.0 . 
     
     build:Parsing and building the Dockerfile
     -t:tag iname of image
     image-id:Randowm hash or name of the tag for image if option -t 
     path:ibuild the current directory 

Image command inspect::

     docker image inspect <image-id>
          ex. docker inspect web1

     inspect:shoes a json dump about the image 
      
Image command delete::

     docker image rm <image-id>|<repository>[<:tagname>]
          ex. docker image rm web1:1.0

Image command tagging username::

     docker image tag <source> <username>/<repository>:<tagname>|[latest]
          ex. docker image tag web1 melanee/web1:latest

Image command login in Docker Hub::

     docker login

Image command pushing to Docker Hub::

     docker image push <username>/<repository>:<tagname>|[latest]
          ex. docker image push melanee/web1:latest 

Image command delete local image by image-id::

     docker image rm  -f <image-id>
          ex. docker image rm -f 1d08
     -f:force will insure all image deletes
     image-id:Found by `docker image ls` use 4 first digit of IMAGE ID field

Image command pulling from Docker Hub::

     docker image pull <username>/<repository>:<tagname>
          ex. docker pull melanee/web1:latest

Pulling
=======
  The deamon (client) tries localy to find the image else download it from the docker hub repository.

Pulling an image from the repository at the docker hub registry::

    docker run [[<url>/]<name space>/]image

    :url: Url of the registry or docker.io or default Docker Hub Registry 
    :name space: User name or library for default Docker Hub Registry 
    :image: The image name of the Repsitory and Image


Docker Hub
----------
  Where you find official images and can publish your non-official images privatly or publicly.

* Is a  Docker Registry.
* Docker Registry contains Docker Repository. 
* Docker Repository is a version controle of an image.

  * Has a collection of Docker images all with the same name.

* Docker image the image

  * Has a Tag that identify the image

* Tag is a the version id  of the image. Default to a tag named latest.

Running
=======
  Create a instance of the image

 
.. _Container:

******************************
 Docker container management!
******************************
A running instance of a image is called a container. Container are immutable and independant of each other.

List of active containers::

    container ls -a

This paragraphe end the literal block

*********
 Network
*********

.. _Prius Quam:

Prius Quam
==========
Içi l'instant précédent de l'action.

.. _Punctum Temporis:

Punctum Temporis
================
Içi le déroulement de l'action.

.. _A Posteriori:

A Posteriori
============
Içi l'instant après l'action.

**********
 Rapports
**********

.. _Logs:

Logs
====

.. _Stat:

Stat
====

.. _Notes:

*******
 Notes
*******

