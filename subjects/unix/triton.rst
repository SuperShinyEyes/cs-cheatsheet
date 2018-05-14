======
Triton
======

File system
===========
Lustre(scratch). Avoid small files. If necessary explicitly tell the system not to spread the data:

.. code-block:: bash
	
	lfs setstripe -c 1 /scratch/path/to/dir
	cp somefile /scratch/path/to/dir/newfile