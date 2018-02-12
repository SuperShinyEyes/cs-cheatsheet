=============
Backup Server
=============

This docs is about building a desktop backup server. Under progress atm.

macOS
=====

Client
######
defaults write com.apple.systempreferences TMShowUnsupportedNetworkVolumes 1

Host
####
/etc/netatalk/AppleVolumes.default

=======================================================================
http://dae.me/blog/1660/concisest-guide-to-setting-up-time-machine-server-on-ubuntu-server-12-04/
https://fzhu.work/blog/mac/making-ubuntu-server-a-mac-time-capsule.html
https://samuelhewitt.com/blog/2015-09-12-debian-linux-server-mac-os-time-machine-backups-how-to
https://kremalicious.com/ubuntu-as-mac-file-server-and-time-machine-volume/
https://wiki.archlinux.org/index.php/avahi#File_sharing

sudo apt-get install netatalk avahi-daemon
sudo adduser danny

mkdir -R /home/danny/tm/
sudo chown -R danny /home/danny/tm/

sudo vim /etc/netatalk/AppleVolumes.default
  :DEFAULT: options:upriv,usedots
  /home/danny/tm "Danny's Time Machine" options:tm volsizelimit:300000 allow:danny

sudo service netatalk restart

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



Linux
=====