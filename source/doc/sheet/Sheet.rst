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

* Tag is a the verion id  of the image. Default to a tag named latest.

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

