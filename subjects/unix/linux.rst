=====
Linux
=====

Commands
========

Show process tree

.. code-block:: bash

  pstree -p -s  # p for pid, s for "show parents"


Show hard drive info

.. code-block:: bash

   sudo lshw -C disk


Investigate partitions
  
.. code-block:: bash
    
  parted /mnt/sparsebundle/sparsebundle.dmg unit B print