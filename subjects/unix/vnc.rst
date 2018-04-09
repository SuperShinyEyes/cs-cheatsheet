===
VNC
===

TigerVNC
========

Server Installation on Ubuntu 16.04
###################################

.. code-block:: bash

	wget -O tigervnc.deb https://bintray.com/tigervnc/stable/download_file?file_path=ubuntu-16.04LTS%2Famd64%2Ftigervncserver_1.8.0-1ubuntu1_amd64.deb

	sudo dpkg -i tigervnc.deb

	sudo apt-get install -f

	vncserver



Client
######

.. code-block:: bash


	# 1. Establish an SSH session. Port number is 5900 + (#display)
	ssh -L 5901:127.0.0.1:5901 -N -f -l host-username host-ip

	# 2. Launch TigerVNC Viewer

	# 3. Type in at *VNC server*: localhost:5901

	# 4. Enter pw for TigerVNC server
