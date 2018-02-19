======
Papers
======
This is a collection of the papaers I've read.

.. contents::
    :local:
    :depth: 2
    
.. role:: red

-------------------------------------------

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
* They used minibatch discrimination in order to compute feature statistics across the minibatch which is added towards the end of the discriminator. 
* Used :math:`\mathcal{N}(0,1)` for weight initialization and then scale at runtime (`He et al., 2015  <Deep_Residual_Learning_for_Image_Recognition_>`_).
* In order to prevent the escalation of signal magnitudes, used a variant of "local response normalization"(`Krizhevsky et al., 2012 <Krizhevsky_et_al_2012_>`_), :math:`b_{x,y} = a_{x,y} \Big/ \sqrt{\frac{1}{N} \sum_{j=0}^{N-1} (a_{x,y}^j)^2 + \epsilon }` where 

  * :math:`\epsilon = 10^{-s}`
  * :math:`N`, the number of feature maps
  * :math:`a_{x,y}, b_{x,y}` original and normalized feature vector in pixel (x, y)

* Used sliced Wasserstein distance(SWD) and multi-scale structural similarity(MS- SSIM) (`Odena_et_al_2017 <Odena_et_al_2017_>`_) to evaluate the importance our individual contributions, and also percep- tually validate the metrics themselves

.. _paper_1: https://github.com/YoungxHelsinki/papers/blob/961603b8eccf5352580871dd43052164ae540962/papers/PROGRESSIVE%20GROWING%20OF%20GANS%20FOR%20IMPROVED%20QUALITY%2C%20STABILITY%2C%20AND%20VARIATION.pdf
.. _celeba: http://mmlab.ie.cuhk.edu.hk/projects/CelebA.html
.. _Krizhevsky_et_al_2012: https://github.com/YoungxHelsinki/papers/blob/10de999c78d6915ee05af6e3a5c72937782d0df1/papers/imagenet-classification-with-deep-convolutional-neural-networks.pdf
.. _Odena_et_al_2017: https://github.com/YoungxHelsinki/papers/blob/bff51d631a512b10507458d5d1e9f28db5a6192f/papers/Conditional_Image_Synthesis_with_Auxiliary_Classifier_GANs.pdf
.. _Deep_Residual_Learning_for_Image_Recognition: https://github.com/YoungxHelsinki/papers/blob/df81a25a4e33d9b96b33e46fa6523ddc30a96f69/papers/Deep_Residual_Learning_for_Image_Recognition.pdf
