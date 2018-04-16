==========
Containers
==========
Containerization means, that your application runs in an isolated container, that is an explicitly defined, reproducable and portable environment. The analogy is taken from freight transport where you ship your goods in containers. [1]_

Docker
======
Docker solves the problem of having identical environments across various stages of development and having isolated environments for your individual applications. [1]_ They persist one main executable/app per container.

Basic management
################

.. code-block:: bash
  
  ## List Docker CLI commands
  docker
  docker container --help
  
  ## Display Docker version and info
  docker --version
  docker version
  docker info

  ## List Docker images
  docker image ls

  ## List Docker containers (running, all, all in quiet mode)
  docker container ls
  docker container ls --all
  docker container ls -aq

  ## Delete an image
  docker image rm <image id>  
  # Remove all images from this machine
  docker image rm $(docker image ls -a -q)   

.. code-block:: bash

  docker pull

  # port forwarding
  # -p host-port:container-port
  docker run --name my-nginx -p 80:80 nginx:1.10.1-alpine


.. code-block:: bash

  docker info

  docker ps
  docker ps -a    # List all, even the stopped ones
  docker stop
  docker start
  docker restart
  docker logs --tail 50 --follow --timestamps instance-name
  docker inspect
  docker rm


.. code-block:: bash
  
  # Launch shell
  docker exec -ti my-nginx /bin/sh


Dockerfile
##########

.. code-block:: bash

  # An example from https://docs.docker.com/get-started/part2/#dockerfile
  # Use an official Python runtime as a parent image
  FROM python:2.7-slim

  # Set the working directory to /app
  WORKDIR /app

  # Copy the current directory contents into the container at /app
  ADD . /app

  # Install any needed packages specified in requirements.txt
  RUN pip install --trusted-host pypi.python.org -r requirements.txt

  # Make port 80 available to the world outside this container
  EXPOSE 80

  # Define environment variable
  ENV NAME World

  # Run app.py when the container launches
  CMD ["python", "app.py"]


Build
#####

.. code-block:: bash

  # Create image using current directory's Dockerfile
  docker build -t image-name .  



Run
###

.. code-block:: bash

  # Map port host:container
  docker run -p 4000:80 image

  # Run in background
  docker run -p 4000:80 image

  # Run Ubuntu Bash
  docker run -it ubuntu bash  


Stop
####

.. code-block:: bash

  # Gracefully stop the specified container
  docker container stop <hash>           

  # Force shutdown of the specified container
  docker container kill <hash>         


Share
#####

.. code-block:: bash

  # 1.
  docker tag image username/repository:tag

  docker push username/repository:tag 


Docker swarm
############

.. code-block:: bash
  
  # docker-compose.yml
  version: "3"
  services:
    web:
      # replace username/repo:tag with your name and image details
      image: username/repo:tag
      deploy:
        replicas: 5
        resources:
          limits:
            cpus: "0.1"
            memory: 50M
        restart_policy:
          condition: on-failure
      ports:
        - "80:80"
      networks:
        - webnet
  networks:
    webnet:



.. code-block:: bash

  docker swarm init

  docker stack deploy -c docker-compose.yml <appname>

  docker service ls

  # A single container running in a service is called a task. 
  # Tasks are given unique IDs that numerically increment, 
  # up to the number of replicas you defined.
  # List the tasks for your service:
  docker service ps <service>

  docker inspect <task or container> 

  # Take down an app
  docker stack rm <appname>

  # Take down the swarm
  docker swarm leave --force


Cleanup containers & images for disk space
##########################################

.. code-block:: bash

  # https://www.digitalocean.com/community/tutorials/how-to-remove-docker-images-containers-and-volumes
  # https://github.com/moby/moby/issues/21925
  docker rm $(docker ps -a -q)


.. code-block:: bash


.. code-block:: bash


.. code-block:: bash


.. code-block:: bash


.. code-block:: bash


.. code-block:: bash




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
