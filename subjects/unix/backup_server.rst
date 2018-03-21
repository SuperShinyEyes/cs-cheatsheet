=============
Backup Server
=============

This docs is about building a desktop backup server. Under progress atm.

macOS
=====

Host(Ubuntu zfs) with Netatalk 2
################################

Dependencies
^^^^^^^^^^^^
* netatalk
* zfs-utils

Steps
^^^^^
We should remember that some zfs file system options cannot be changed once created.

.. code-block:: bash

  # Create a pool
  zpool create -f timemachine_backup_zpool /dev/sda ...
  zpool status
  zfs list

  # Create a fs
  zfs create timemachine_backup_zpool/fs
  zfs list

  # https://linux.die.net/man/8/zfs
  zfs create -o normalization=formD -o nbmand=off -o utf8only=on -o aclinherit=passthrough

  # Properties of the fs
  zfs get all timemachine_backup_zpool/fs

  # Set properties of the fs
  zfs set quota=500G timemachine_backup_zpool/fs
  zfs set compression=on timemachine_backup_zpool/fs

  chown danny timemachine_backup_zpool/fs

  # Install netatalk
  sudo apt-get install netatalk

  sudo vim /etc/netatalk/AppleVolumes.default
  # Edit the ending.
  # :DEFAULT: options:upriv,usedots
  # /home/danny/tm "Danny's Time Machine" options:tm allow:danny

  sudo service netatalk restart


ZFS setup
^^^^^^^^^

From `ZFS Linux mail list <zfs_linux_mail_list_>`_:

  On ext4 (or ZFS with normalization=none), you get two files that appear to have the same name, because one is NFC (uses LATIN SMALL LETTER E WITH ACUTE) and the other is NFD (LATIN SMALL LETTER E, COMBINING ACUTE ACCENT). If you use any of the other normalization options on ZFS, they will be treated as the same file. This seems like a desirable behavior to me, and should help avoid interoperability problems across systems which generally use different normal forms (e.g. Linux vs. OS X).

.. code-block:: bash

  zfs create -o normalization=formD

In macOS, options `-O casesensitivity=insensitive -O normalization=formD` are `essential <zfs_option_on_macOS_>`_.

.. _zfs_option_on_macOS: https://apple.stackexchange.com/a/111186/266739

.. code-block:: bash

  sudo zpool create timemachine_pool -f \
  -o ashift=12 \
   /dev/sda /dev/sdb /dev/sdc

  sudo zfs create timemachine_pool/user \
  -o nbmand=off \
  -o utf8only=on \
  -o aclinherit=passthrough \
  -o compression=lz4 \
  -o casesensitivity=insensitive \
  -o atime=off \
  -o normalization=formD \
  -o quota=300G


  zfs create -o normalization=formD -o nbmand=off -o utf8only=on -o aclinherit=passthrough

  -o ashift=12 # For advanced. http://louwrentius.com/zfs-performance-and-capacity-impact-of-ashift9-on-4k-sector-drives.html


.. code-block:: bash

  #!/bin/bash

  USER=$1

  # Test user exists
  id $USER

  zfs create timemachine_pool/$USER \
    -o nbmand=off \
    -o utf8only=on \
    -o aclinherit=passthrough \
    -o compression=lz4 \
    -o casesensitivity=insensitive \
    -o atime=off \
    -o normalization=formD \
    -o quota=300G


  chown -R $USER /timemachine_pool/$USER

  chmod 700 /timemachine_pool/$USER


* compression=lz4, which not only saves space, but is faster as well. Loading a file from even an SSD is slow, decompressing it the CPU faster. So, the reduced file size helps loading it faster, while the time needed for decompression is still smaller, resulting in overall lesser time used. Follow this link for experimental results.
* atime=off switches of the access time file attribute. Otherwise every time a file is read the access time would be set to the current date, issuing an unnecessary write (wearing down the hard drive and endangering the file).
* `ashift=12` This specifies that your disk is Advanced Format, which is the same as saying it has 4096 byte sectors instead of the old 512 byte sectors. Most disks made after 2011 are advanced format so you'll need this option most of the time. If you forget, ZFS assumes the sector size is 512. If that's the wrong answer, you'll take a big performance hit.

Here's a script that automates the filesystem creation and acl update. Use with `./setup.sh username`.

.. code-block:: bash
  
  #!/bin/bash
  # setup.sh

  username=$1

  zfs create timemachine_pool/$username \
    -o nbmand=off \
    -o utf8only=on \
    -o aclinherit=passthrough \
    -o compression=lz4 \
    -o casesensitivity=insensitive \
    -o atime=off \
    -o normalization=formD \
    -o quota=500G

  chown -R $username /timemachine_pool/$username

.. _zfs_linux_mail_list: http://list.zfsonlinux.org/pipermail/zfs-discuss/2013-July/010059.html


Netatalk setup
^^^^^^^^^^^^^^
Set logging

.. code-block:: bash
  
  # Put this at the end of /etc/netatalk/afpd.conf
  -setuplog "default log_info /var/log/afpd.log"

  service netatalk restart

  tail -f /var/log/afpd.log


To set number of clients, edit `/etc/default/netatalk` and restart Netatalk.

.. code-block:: bash
  
  # /etc/default/netatalk  
  # Netatalk 2.x configuration

  #########################################################################
  # Global configuration
  #########################################################################

  #### machine's AFPserver/AppleTalk name.
  #ATALK_NAME=machinename

  #### server (unix) and legacy client (<= Mac OS 9) charsets
  ATALK_UNIX_CHARSET='LOCALE'
  ATALK_MAC_CHARSET='MAC_ROMAN'

  #### Don't Edit. export the charsets, read form ENV by apps
  export ATALK_UNIX_CHARSET
  export ATALK_MAC_CHARSET

  #########################################################################
  # AFP specific configuration
  #########################################################################

  #### Set which daemons to run.
  #### If you use AFP file server, run both cnid_metad and afpd.
  CNID_METAD_RUN=yes
  AFPD_RUN=yes

  #### maximum number of clients that can connect:
  AFPD_MAX_CLIENTS=50

  #### UAMs (User Authentication Modules)
  #### available options: uams_dhx.so, uams_dhx2.so, uams_guest.so,
  ####                    uams_clrtxt.so(legacy), uams_randnum.so(legacy)
  #AFPD_UAMLIST="-U uams_dhx2.so,uams_clrtxt.so"

  #### Set the id of the guest user when using uams_guest.so
  #AFPD_GUEST=nobody

  #### config for cnid_metad. Default log config:
  #CNID_CONFIG="-l log_note"

  #########################################################################
  # AppleTalk specific configuration (legacy)
  #########################################################################

  #### Set which legacy daemons to run.
  #### If you need AppleTalk, run atalkd.
  #### papd, timelord and a2boot are dependent upon atalkd.
  #ATALKD_RUN=no
  #PAPD_RUN=no
  #TIMELORD_RUN=no
  #A2BOOT_RUN=no

  #### Control whether the daemons are started in the background.
  #### If it is dissatisfied that legacy atalkd starts slowly, set "yes".
  #### In case using systemd/systemctl, this is not so significant.
  #ATALK_BGROUND=no

  #### Set the AppleTalk Zone name.
  #### NOTE: if your zone has spaces in it, you're better off specifying
  ####       it in atalkd.conf
  #ATALK_ZONE=@zone

After the edit, you run `service netatalk status` and you see `-c 50` which sets the max num. of clients as 50.

.. code-block:: bash

  ● netatalk.service
     Loaded: loaded (/etc/init.d/netatalk; bad; vendor preset: enabled)
     Active: active (running) since Thu 2018-03-08 12:45:10 EET; 46s ago
       Docs: man:systemd-sysv-generator(8)
    Process: 953 ExecStart=/etc/init.d/netatalk start (code=exited, status=0/SUCCESS)
     CGroup: /system.slice/netatalk.service
             ├─974 /usr/sbin/cnid_metad -l log_note
             └─986 /usr/sbin/afpd -U uams_dhx2.so,uams_clrtxt.so -g nobody -c 50 -n TimeMachine

Client(Macs)
############

Backup
^^^^^^

* Set the destination via GUI in order to set encryption on

  * Mount via Finder. (e.g. afp://gorilla.org.gakkou.fi )
  * Then set it as the destination

* You can set via command line but you won't be able to encrypt the backup.

  * `sudo tmutil setdestination -p "afp://danny@gorilla.org.gakkou.fi/Danny's Time Machine"`
  * The name specified in `/etc/netatalk/AppleVolumes.default` should be given.


Restore
^^^^^^^
Enter the Backup from Time Machine in Recovery Mode.

Set source as `afp://username@gorilla.org.gakkou.fi/tm`. The final directory which you defined to share in `/etc/netatalk/AppleVolumes.default` should be given after the ip address. If you cannot check the server you could try when mounted, 

.. code-block:: bash

  # Show all mounted disks
  mount


-----------------------------------------------


Miscellaneous
=============

sudo apt-get install netatalk avahi-daemon
sudo adduser danny

mkdir -R /home/danny/tm/
sudo chown -R danny /home/danny/tm/




sudo vim /etc/nsswitch.conf 
hosts:          files mdns4_minimal [NOTFOUND=return] dns mdns4 mdns

sudo vim /etc/avahi/services/afpd.service

<?xml version="1.0" standalone="no"?>
<!DOCTYPE service-group SYSTEM "avahi-service.dtd">

<service-group>
    <name replace-wildcards="yes">%h</name>

    <service>
        <type>_device-info._tcp</type>
        <port>0</port>
        <txt-record>model=Aalto Time Machine Beta</txt-record>
    </service>
</service-group>


sudo vim /etc/avahi/services/smb.service

<?xml version="1.0" standalone='no'?><!--*-nxml-*-->
<!DOCTYPE service-group SYSTEM "avahi-service.dtd">

<service-group>
    <name replace-wildcards="yes">%h</name>
    <service>
        <type>_smb._tcp</type>
        <port>445</port>
    </service>
    <service>
         <type>_device-info._tcp</type>
         <port>0</port>
         <txt-record>model=AaltoTMTest</txt-record>
    </service>
</service-group>

sudo service avahi-daemon restart




-------------------

docker run -dt \
  -v /l/backup_server_tools/smb.conf:/etc/samba/smb.conf \
  -v /timemachine_backup_zpool/parks1/dozer:/dozer \
  -v /l/backup_server_tools/share:/share \
  -p 445:445 \
  --name samba \
  --restart=always \
  stanback/alpine-samba

docker run -dt \
  -v /home/leon/smb.conf:/etc/samba/smb.conf \
  -v /timemachine_backup_zpool/parks1/dozer:/dozer \
  -v /timemachine_backup_zpool/parks1/share:/share \
  -p 445:445 \
  --name samba \
  --restart=always \
  stanback/alpine-samba






docker run -d \
  -v /l/backup_server_tools/services:/etc/avahi/services \
  --net=host \
  --name=avahi \
  --restart=always \
  stanback/alpine-avahi


[global]
  workgroup = WORKGROUP
  server string = %h server (Samba, Alpine)
  security = user
  map to guest = Bad User
  encrypt passwords = yes
  load printers = no
  printing = bsd
  printcap name = /dev/null
  disable spoolss = yes
  disable netbios = yes
  server role = standalone
  server services = -dns, -nbt
  smb ports = 445
  name resolve order = hosts
  ;log level = 3
  create mask = 0664
  directory mask = 0775
  veto files = /.DS_Store/
  nt acl support = no
  inherit acls = yes
  ea support = yes
  vfs objects = catia fruit streams_xattr recycle
  acl_xattr:ignore system acls = yes
  recycle:repository = .recycle
  recycle:keeptree = yes
  recycle:versions = yes

[Dozer]
  path = /timemachine_backup_zpool/parks1/dozer
  comment = ZFS
  browseable = yes
  writable = yes
  valid users = leon

[Shared]
  path = /timemachine_backup_zpool/parks1/share
  comment = Shared Folder
  browseable = yes
  read only = yes
  write list = leon
  guest ok = yes



Linux
=====





.. rubric:: References

.. [1] http://dae.me/blog/1660/concisest-guide-to-setting-up-time-machine-server-on-ubuntu-server-12-04/
.. [2] https://fzhu.work/blog/mac/making-ubuntu-server-a-mac-time-capsule.html
.. [3] https://samuelhewitt.com/blog/2015-09-12-debian-linux-server-mac-os-time-machine-backups-how-to
.. [4] https://kremalicious.com/ubuntu-as-mac-file-server-and-time-machine-volume/
.. [5] https://wiki.archlinux.org/index.php/avahi#File_sharing
.. [Time Machine Server Requirements] https://developer.apple.com/library/content/documentation/NetworkingInternetWeb/Conceptual/TimeMachineNetworkInterfaceSpecification/TimeMachineRequirements/TimeMachineRequirements.html
.. [AFP and SMB File Sharing on CentOS 7] https://zitseng.com/archives/6182
.. [Time Machine Setup On CentOS 7] https://zitseng.com/archives/10208
.. [Don't use FreeNAS] https://community.spiceworks.com/topic/1688975-why-would-you-pick-freenas?page=2
.. [Restoring from a snapshot with APFS] https://datarecovery.wondershare.com/apfs/how-to-convert-hfs-to-apfs-without-losing-data.html
.. [the safest file storage setup (using zfs)] http://patrick.mukherjee.de/?p=304
.. [Install ZFS on Debian GNU/Linux] https://pthree.org/2012/04/17/install-zfs-on-debian-gnulinux/
.. [Rsync OS X] https://rsyncosx.github.io/Documentation/docs/DIYNAS.html
.. [APFS in Detail: Encryption, Snapshots, and Backup] http://dtrace.org/blogs/ahl/2016/06/19/apfs-part2/
.. [Apple APFS Guide] https://developer.apple.com/library/content/documentation/FileManagement/Conceptual/APFS_Guide/Introduction/Introduction.html
.. [ZFS cheatsheet] https://www.thegeekdiary.com/solaris-zfs-command-line-reference-cheat-sheet/
.. [Time Machine Server Requirements] https://developer.apple.com/library/content/documentation/NetworkingInternetWeb/Conceptual/TimeMachineNetworkInterfaceSpecification/TimeMachineRequirements/TimeMachineRequirements.html
