================
Cross-validation
================

A methodology for tuning hyperparameters without overfitting.

Divide the dataset into 3 partitions: training, validation and test.
Use the training data for learning parameters and evaluate the hyperparameters using the validation set. Use the test set at very last only once as it would represent general data.

Exhuastive cross-validation
^^^^^^^^^^^^^^^^^^^^^^^^^^^

Leave-p-out cross-validation
----------------------------

Leave-one-out cross-validation
------------------------------

Non-exhuastive cross-validation
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

k-fold cross-validation
-----------------------

.. figure:: /images/machine_learning/k_fold_crossvalidation.png
   :align: center
   :alt: alternate text
   :figclass: align-center

   < An image from a Stanford course, `Neural Networks for Visual Recognition`_ >

.. _`Neural Networks for Visual Recognition`: http://cs231n.stanford.edu/slides/2017/cs231n_2017_lecture2.pdf
