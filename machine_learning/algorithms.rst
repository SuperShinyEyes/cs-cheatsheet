==========
Algorithms
==========

K-Nearest Neighbors: Distance Metric
====================================

L1(manhattan) distance
^^^^^^^^^^^^^^^^^^^^^^
.. figure:: /images/machine_learning/distances.png
   :align: center
   :alt: alternate text
   :figclass: align-center

   < L1, L2, Chebyshev distances >

.. math::
   d_1(I_1, I_2) = \sum_{p} \left|I_1^p - I_2^p\right|

*L1* value may change if the coordinate system changes.


L2(Euclidean) distance
^^^^^^^^^^^^^^^^^^^^^^
.. math::
   d_1(I_1, I_2) = \sqrt{\sum_{p} (I_1^p - I_2^p)^2}

More generic. Values stay the same even when the coordinate system changes.

