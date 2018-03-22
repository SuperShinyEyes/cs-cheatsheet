=====
Samba
=====


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