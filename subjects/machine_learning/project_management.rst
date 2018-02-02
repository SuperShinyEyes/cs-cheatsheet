==================
Project Management
==================

This post is a machine learning project management guideline. This assumes you have chosen your dataset.

Pull existing Kernels
=====================
For just a sake of a little kick to move your project going, pull existing kernels from dataset owner or Kaggle and see what people have done. Play around with it a little.

Analyze the data
================
Super important. It's very easy to just dive into algorithms and treat data as just digits but it is cruicial that you comprehend your data; distribution, symmetry, correlation, pixel aggregation(for visual data) and etc. Visualize with histograms, `violin <violin_plot_>`_, `swarm <swarm_plot_>`_ or heatmaps.


.. _violin_plot: https://seaborn.pydata.org/generated/seaborn.violinplot.html
.. _swarm_plot: https://seaborn.pydata.org/generated/seaborn.swarmplot.html

.. figure:: /images/machine_learning/heatmap_correlation.png
   :align: center
   :scale: 20%
   :alt: alternate text
   :figclass: align-center

   < Correlation heatmap of Fashion-MNIST >

.. figure:: /images/machine_learning/pixel_intensity_by_labels.png
  :scale: 50%
  :align: center
  :alt: alternate text
  :figclass: align-center

  < Pixel intensity distribution of Fashion-MNIST >



.. figure:: /images/machine_learning/symmetry_by_labels.png
  :scale: 50%
  :align: center
  :alt: alternate text
  :figclass: align-center

  < Symmetry per category of Fashion-MNIST >



.. figure:: /images/machine_learning/Pixel_aggregation_over_train_data_by_labels.png
   :scale: 50%
   :align: center
   :alt: alternate text
   :figclass: align-center

   < Pixel aggregation per category of Fashion-MNIST >


Write related work   
==================
Write **Related work** section like you would in your thesis. During this phase you will read existing works and get some idea from academia. Definitely checkout dataset owner as they usually have many related papers as references. For instance, `Zalando's Fashion-MNIST`_ has good number of references.
   
.. _Zalando's Fashion-MNIST: https://github.com/zalandoresearch/fashion-mnist


Tune your hyperparameters
=========================
This is a very time consuming phase. You could run grid search but they can be exhuasting. Instead, you could do a step-by-step tuning. For a CNN model for Fashion-MNIST my team tuned in the following order:

  1. regularization parameters
  2. activation functions
  3. dropout rates
  4. optimization methods


Check which class of data is hard to train
==========================================
Check and fix


Make a Telegram notification bot training models
================================================
Send a message of a output file name with plots of the data. It saves you time and also it's a good log.


Benchmark your model on different dataset
=========================================
If you are doing image classification run it on MNIST or CIFAR10. 