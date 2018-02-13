======
Papers
======

.. contents::
    :local:
    :depth: 2
    
.. role:: red

`PROGRESSIVE GROWING OF GANS FOR IMPROVED QUALITY, STABILITY, AND VARIATION <paper_1_>`_
========================================================================================
Authors: Tero Karras, Timo Aila, Samuli Laine, Jaakko Lehtinen

.. figure:: /images/papers/progressive_gan.png
   :align: center
   :alt: alternate text
   :figclass: align-center

* `Github <https://github.com/tkarras/progressive_growing_of_gans>`_, `Youtube <https://www.youtube.com/watch?v=XOxxPcy5Gr4&feature=youtu.be>`_, `Trained network <https://drive.google.com/drive/folders/0B4qLcYyJmiz0NHFULTdYc05lX0U>`_
* Image generation from `CELEBA <celeba_>`_ with GAN(Generative Adversarial Networks) using generator and discriminator progressively. 
* 8.80 in unsupervised CIFAR10
* GAN is differentiable and this allows us to guide generators and discriminators to the right direction. 
* High-resolution image generation is difficult as discriminator can more easily distinguish fakes from real images, thus :red:`amplifies gradient problem`.
* The paper used low-resolution training sets in the beginning, and add new layers that introduce higher-resolution details as the training progresses.

.. _paper_1: https://github.com/YoungxHelsinki/papers/blob/master/PROGRESSIVE%20GROWING%20OF%20GANS%20FOR%20IMPROVED%20QUALITY%2C%20STABILITY%2C%20AND%20VARIATION.pdf
.. _celeba: http://mmlab.ie.cuhk.edu.hk/projects/CelebA.html