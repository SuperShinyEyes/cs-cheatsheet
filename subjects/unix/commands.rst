========
Commands
========

grep
####

.. code-block:: bash

  // https://stackoverflow.com/a/16957078
  grep -rnw '/path/to/somewhere/' -e 'pattern'
  grep -rnw --exclude=\*.{html,js}  "Welcome"



Processes
#########

Show process tree

.. code-block:: bash

  pstree -p -s  # p for pid, s for "show parents"

------------------

About Disks
###########

.. code-block:: bash

  # Show hard drive info
  sudo lshw -C disk

  # Show installed disks
  lsblk

  # Investigate partitions
  parted /mnt/sparsebundle/sparsebundle.dmg unit B print

  # Manipulate disk partition table
  sudo fdisk -l | grep Error


----------------------------

SSH
###

Run commands from login shell via SSH

.. code-block:: bash

  ssh false bash -c -l "module load tensorflow"
  

Samba
#####

.. code-block:: bash
  
  net ads search samaccountname="username"
  net ads search co="korea" cn
  net ads search cn="famil-name first-name"
  net ads search mail=email samaccountname
  net ads search cn="famil-name first-name" samaccountname
  net ads search cn="famil-name first-name" samaccountname mail


System
######

.. code-block:: bash

  journalctl

zfs
###

.. code-block:: bash

  # Create a pool
  zpool create -f timemachine_backup_zpool /dev/sda ...
  zpool status
  zfs list

  # Create a fs
  zfs create timemachine_backup_zpool/fs
  zfs list

  # Properties of the fs
  zfs get all timemachine_backup_zpool/fs

  # Set properties of the fs
  zfs set quota=500G timemachine_backup_zpool/fs
  zfs set compression=on timemachine_backup_zpool/fs


Network
#######

.. code-block:: bash
  
  # Show open ports
  netstat -tulpn | grep LISTEN    # sudo for process id
  

