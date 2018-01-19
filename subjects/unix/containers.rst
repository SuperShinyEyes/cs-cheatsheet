==========
Containers
==========

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
