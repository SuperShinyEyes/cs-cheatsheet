===========
Singularity
===========

Intro
=====

.. code-block:: bash

  # Test
  singularity selftest

  ####### Output #######
  # + sh -c test -f /etc/singularity/singularity.conf                       (retval=0) OK
  # + test -u /usr/lib/x86_64-linux-gnu/singularity/bin/action-suid         (retval=0) OK
  # + test -u /usr/lib/x86_64-linux-gnu/singularity/bin/mount-suid          (retval=0) OK
  # + test -u /usr/lib/x86_64-linux-gnu/singularity/bin/start-suid          (retval=0) OK


Build
=====

.. code-block:: bash

  # Build from Docker hub
  singularity build englishspeechupsampler-latest.simg docker://shinyeyes/englishspeechupsampler


Shell
=====

.. code-block:: bash

  # Enter the shell of an image with specified shell and GPU
  singularity shell -s /bin/bash --nv englishspeechupsampler-latest.simg

  # Bind a dir
  singularity shell -s /bin/bash --nv -B /l:/l englishspeechupsampler-latest.simg

  singularity shell -s /bin/bash --nv -B /l/EnglishSpeechUpsampler/settings:/settings -B /storage_zpool/data:/storage_zpool/data english-speech-upsampler-v0.1.simg


  source /environment
Exec
====

.. code-block:: bash

  # Run Python from the image
  singularity exec python englishspeechupsampler-latest.simg


Run
===
Execute the "Run" scripts from recipe.

.. code-block:: bash

  singularity run --nv -B /l:/l englishspeechupsampler-latest.img

  singularity run  -B _build/html:/usr/share/nginx/html ${SINGULARITY_PULLFOLDER}/nginx.simg

  singularity run  -B /usr/share/nginx/html:_build/html ${SINGULARITY_PULLFOLDER}/nginx.simg
  

.. rubric:: References

.. [1] https://takacsmark.com/getting-started-with-docker-in-your-project-step-by-step-tutorial/










singularity shell -s /bin/bash --nv -B /l/gadichs1/gitrepos/temp/fsecure-exercise/:/workspace $SINGULARITY_PULLFOLDER/audio-u-net-v0.1.simg