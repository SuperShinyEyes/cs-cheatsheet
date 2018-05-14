=====
Samba
=====

Setup on Ubuntu 16.04
=====================
https://help.ubuntu.com/community/How%20to%20Create%20a%20Network%20Share%20Via%20Samba%20Via%20CLI%20(Command-line%20interface/Linux%20Terminal)%20-%20Uncomplicated,%20Simple%20and%20Brief%20Way!

.. code-block:: bash

    sudo apt-get update
    sudo apt-get install samba

    sudo smbpasswd -a <user_name>


.. code-block:: bash

    sudo nano /etc/samba/smb.conf


::

    Once "smb.conf" has loaded, add this to the very end of the file:

    [<folder_name>]
    path = /home/<user_name>/<folder_name>
    valid users = <user_name>
    read only = no


.. code-block:: bash

    sudo service smbd restart
    
    # Once Samba has restarted, use this command to check your smb.conf for any syntax errors
    testparm



.. [Fixing slow macOS finder connection to Linux Samba server] https://medium.com/@augusteo/fixing-slow-macos-finder-connection-to-linux-samba-server-ed7e5ea784c1
.. [Samba4, ZFS on Linux and Mac OS X clients] http://juosukai.github.io/2014/12/29/samba-4-mac/
.. [MacEnterprise] https://groups.google.com/forum/#!forum/macenterprise
.. [Samba (fruit) configuration] https://gist.github.com/eddyxu/d9bfff97e33183c34bbb906b9d48c55a
.. [Samba fruit man] https://www.samba.org/samba/docs/current/man-html/samba.7.html


zfs create timemachine_pool/mnelimar \
    -o nbmand=off \
    -o utf8only=on \
    -o aclinherit=passthrough \
    -o compression=lz4 \
    -o casesensitivity=insensitive \
    -o atime=off \
    -o normalization=formD \
    -o quota=500G

chown -R mnelimar /timemachine_pool/mnelimar