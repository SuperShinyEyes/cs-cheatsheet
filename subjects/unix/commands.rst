========
Commands
========

grep
####

.. code-block:: bash

  # https://stackoverflow.com/a/16957078
  grep -rnw '/path/to/somewhere/' -e 'pattern'
  grep -rnw --exclude=\*.{html,js}  "Welcome"

  grep -rnw . -e so | xargs egrep -v '^#'
  grep -rnw . -e so | xargs -n 1 egrep -v '^#'
  dpkg -L netatalk | grep etc | xargs -n 1 egrep -v '^#'
  dpkg -L netatalk | grep etc | xargs -n 1 egrep -v '^#' | less
  dpkg -L netatalk | grep etc | xargs -n 1 grep -v '^#' | less
  dpkg -L netatalk | grep etc | xargs -n 1 grep -v '^#' | grep '\.so'
  dpkg -L netatalk | grep etc | xargs grep -v '^#' | grep '\.so'

  # Get only directories
  ls -l | grep "^d"

  # Get all non-empty lines
  # Pattern matches "begin of string (^), directly followed by "end of string" ($)
  grep -v '^$' file-name

  # Get all, except empty & commented lines
  grep -v '^$' .zshrc | grep -v '^#'


.. code-block:: bash

  find . -name "*netatalk*"


File system
###########

.. code-block:: bash
  
  # Understand Linux Files ystem
  man 7 hier

  # Check file systems
  df -T

  # Delete all partition table on a disk
  # Unmount all partitions!
  dd if=/dev/zero of=/dev/sda bs=512 count=1 conv=notrunc

Disk usage
##########

.. code-block:: bash

  // For any file or dir
  du -sh *

  // The filesystem.
  df -h

GPU info
########

.. code-block:: bash

  # 1. NVIDIA gpus
  nvidia-smi

  # 2. Display info
  (sudo) lshw -C display

  # 3. Device info
  lspci  -v -s  $(lspci | grep ' VGA ' | cut -d" " -f 1)

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

  # Install zfs on Ubuntu
  apt-get install zfsutils-linux

  # Check zfs version
  modprobe zfs

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

  # zfs version
  dmesg | grep ZFS
  modinfo zfs
  cat /sys/module/zfs/version

  # Add a disk
  # https://www.howtogeek.com/175159/an-introduction-to-the-z-file-system-zfs-for-linux/
  sudo zpool add -f pool /dev/sde /dev/sdf /dev/sdg

  # Setting mount point
  zfs get mountpoint timemachine_backup_zpool/fs
  zfs unmount -f timemachine_backup_zpool/fs  
  zfs set mountpoint=/home/leon/timemachine_backup_zpool/fs timemachine_backup_zpool/fs
  zfs get mountpoint timemachine_backup_zpool/fs
  zfs mount 
  zfs get mounted pool/filesystem
  

Network
#######

.. code-block:: bash
  
  # Show open ports
  netstat -tulpn | grep LISTEN    # sudo for process id
  

Package management
##################

.. code-block:: bash

  # https://unix.stackexchange.com/a/6286
  # Dry run. -s for simulation
  apt-get -s install <package>

  # See all possible upgrades
  apt-get -V -s upgrade

  # Search installed/remote packages
  apt-cache policy <package>

  # Search remote packages
  apt-cache search <package>

  # Show version if installed
  apt-show-versions <package>

  # Show every file related to the package
  dpkg-query -L <package>

  # Get package version
  dpkg -l | grep netatalk

  # Get package files
  dpkg -L netatalk


.. code-block:: bash

  ./configure
  make
  checkinstall

  dpkg -r <package>