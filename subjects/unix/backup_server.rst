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

  chown danny timemachine_backup_zpool/fs

  # Install netatalk
  sudo apt-get install netatalk

  sudo vim /etc/netatalk/AppleVolumes.default
  # Edit the ending.
  # :DEFAULT: options:upriv,usedots
  # /home/danny/tm "Danny's Time Machine" options:tm allow:danny

  sudo service netatalk restart


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