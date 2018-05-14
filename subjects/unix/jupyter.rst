=======
Jupyter
=======

Git config for ipynb
====================

.. code-block:: bash

	cd git-repo...

	/share/apps/jupyterhub/live/miniconda/bin/nbdime config-git --enable
	sed --in-place -r 's@(= )[ a-z/-]*(git-nb)@\1/share/apps/jupyterhub/live/miniconda/bin/\2@' .git/config