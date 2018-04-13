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


  # Launch shell
  docker exec -ti my-nginx /bin/sh


  docker rm

  

Singularity
===========

Intro
#####

.. code-block:: bash

  # Test
  singularity selftest

  ####### Output #######
  # + sh -c test -f /etc/singularity/singularity.conf                       (retval=0) OK
  # + test -u /usr/lib/x86_64-linux-gnu/singularity/bin/action-suid         (retval=0) OK
  # + test -u /usr/lib/x86_64-linux-gnu/singularity/bin/mount-suid          (retval=0) OK
  # + test -u /usr/lib/x86_64-linux-gnu/singularity/bin/start-suid          (retval=0) OK


Build
#####

.. code-block:: bash

  # Build from Docker hub
  singularity build englishspeechupsampler-latest.simg docker://shinyeyes/englishspeechupsampler


Shell
#####

.. code-block:: bash

  # Enter the shell of an image with specified shell and GPU
  singularity shell -s /bin/bash --nv englishspeechupsampler-latest.simg

  # Bind a dir
  singularity shell -s /bin/bash --nv -B /l:/l englishspeechupsampler-latest.simg

  singularity shell --nv -B /l:/l -s /usr/bash englishspeechupsampler-latest.simg


Exec
####

.. code-block:: bash

  # Run Python from the image
  singularity exec python englishspeechupsampler-latest.simg


Run
###
Execute the "Run" scripts from recipe.

.. code-block:: bash

  singularity run --nv -B /l:/l englishspeechupsampler-latest.img
  

.. rubric:: References

.. [1] https://takacsmark.com/getting-started-with-docker-in-your-project-step-by-step-tutorial/
