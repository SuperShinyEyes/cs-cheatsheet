=====
Linux
=====

Commands
========

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

  net ads search cn="jackie chan"
  net ads search mail=email samaccountname
  net ads search cn="jackie chan" samaccountname
  net ads search cn="jackie chan" samaccountname mail


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

