=============
Backup Server
=============

This docs is about building a desktop backup server. Under progress atm.

macOS
=====

Host(Ubuntu zfs)
################

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
  sudo zpool create -f -o ashift=12 \
  -O compression=lz4 \
  -O casesensitivity=insensitive \
  -O atime=off \
  -O normalization=formD \
  -poolname- mirror /dev/disk /dev/disk  

* compression=lz4, which not only saves space, but is faster as well. Loading a file from even an SSD is slow, decompressing it the CPU faster. So, the reduced file size helps loading it faster, while the time needed for decompression is still smaller, resulting in overall lesser time used. Follow this link for experimental results.
* atime=off switches of the access time file attribute. Otherwise every time a file is read the access time would be set to the current date, issuing an unnecessary write (wearing down the hard drive and endangering the file).
* ashift=12 adapts the block size to suit modern hard drives (Advanced Format Disks). Read on for a better explanation.

.. _zfs_linux_mail_list: http://list.zfsonlinux.org/pipermail/zfs-discuss/2013-July/010059.html

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
