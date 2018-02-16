==========
Containers
==========
Containerization means, that your application runs in an isolated container, that is an explicitly defined, reproducable and portable environment. The analogy is taken from freight transport where you ship your goods in containers. [1]_

Docker
======
Docker solves the problem of having identical environments across various stages of development and having isolated environments for your individual applications. [1]_ They persist one main executable/app per container.


.. code-block:: bash
  
  docker pull

  # port forwarding
  # -p host-port:container-port
  docker run --name my-nginx -p 80:80 nginx:1.10.1-alpine

  docker run -d \
  -v $PWD/services:/etc/avahi/services \
  --net=host \
  --name=avahi \
  --restart=always \
  stanback/alpine-avahi

  docker ps
  docker ps -a    # List all, even the stopped ones
  docker stop
  docker start
  docker restart
  docker logs --tail 50 --follow --timestamps instance-name
  docker inspect

Singularity
===========

Start a shell with Nvidia ``LD_PATH``
#####################################

.. code-block:: bash

  singularity shell --nv foo.simg

  # Mount a disk /l
  singularity shell --nv -B /l:/l foo.simg

  # Use your shell
  singularity shell --nv -B /l:/l -s /usr/bash foo.simg

  # My daily use
  singularity shell --nv -B /l:/l -s /usr/bash opensciencegrid-osgvo-tensorflow-gpu-master-latest.simg


.. rubric:: References

.. [1] https://takacsmark.com/getting-started-with-docker-in-your-project-step-by-step-tutorial/
