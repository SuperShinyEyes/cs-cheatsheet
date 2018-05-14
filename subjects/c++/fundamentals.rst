============
Fundamentals
============

GCC
===
Here I'm using GCC version 5.4.0.

.. code-block:: bash

	# -g -03 for heavy optimization
	# -marge=native for producing code that is tailored 
	# for this specific CPU 
	g++ -g -O3 -march=native -std=c++17 v0.cc

	# -S for seeing the assembly code
	# It will produce v0.s
	g++ -S -g -O3 -march=native -std=c++17 v0.cc


OpenMP
======

.. code-block:: bash

	g++ -fopenmp -g -O3 -march=native -std=c++17 v0.cc


.. code-block:: cpp

	#pragma omp parallel for
	for (int i = 0; i < 10; ++i) {
	    c(i);
	}


.. code-block:: bash


.. code-block:: bash


.. code-block:: bash


.. code-block:: bash


.. code-block:: bash


.. code-block:: bash


.. code-block:: bash


