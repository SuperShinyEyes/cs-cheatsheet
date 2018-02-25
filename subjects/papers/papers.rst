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
Authors: Tero Karras, Timo Aila, Samuli Laine, Jaakko Lehtinen, 2018

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
* Progressive variant offers two main benefits: it converges to a considerably better optimum and also reduces the total training time by about a factor of two.

.. _paper_1: https://github.com/YoungxHelsinki/papers/blob/961603b8eccf5352580871dd43052164ae540962/papers/PROGRESSIVE%20GROWING%20OF%20GANS%20FOR%20IMPROVED%20QUALITY%2C%20STABILITY%2C%20AND%20VARIATION.pdf
.. _celeba: http://mmlab.ie.cuhk.edu.hk/projects/CelebA.html
.. _Krizhevsky_et_al_2012: https://github.com/YoungxHelsinki/papers/blob/10de999c78d6915ee05af6e3a5c72937782d0df1/papers/imagenet-classification-with-deep-convolutional-neural-networks.pdf
.. _Odena_et_al_2017: https://github.com/YoungxHelsinki/papers/blob/bff51d631a512b10507458d5d1e9f28db5a6192f/papers/Conditional_Image_Synthesis_with_Auxiliary_Classifier_GANs.pdf
.. _Deep_Residual_Learning_for_Image_Recognition: https://github.com/YoungxHelsinki/papers/blob/df81a25a4e33d9b96b33e46fa6523ddc30a96f69/papers/Deep_Residual_Learning_for_Image_Recognition.pdf

------------------------------------------------------

`UNSUPERVISED REPRESENTATION LEARNING WITH DEEP CONVOLUTIONAL GENERATIVE ADVERSARIAL NETWORKS <papar_2>`_
=========================================================================================================
Authors: Alec Radford & Luke Metz, Soumith Chintala, 2016

Related work on unsupervised representation learning
####################################################

* :red:`Hierarchical clustring` of image patches(Coates & Ng 2012)
* :red:`auto-encoder training` (Vincent et al. 2010), (Zhao et al., 2015), (Rasmus et al., 2015)
* :red:`Deep belief networks` (Lee et al., 2009)

Generating natural images
#########################
* parametric

  * samples often suffer from being blurry
  * iterative forward diffusion process (Sohl-Dickstein et al., 2015)
  * GAN (`Goodfellow et al., 2014 <Goodfellow_et_al_2014>`_) suffers from being noisy and incomprehensible.
    
    * A :red:`laplacian pyramid` extension to this approach (Denton et al., 2015) showed higher quality images, but they still suffered from the objects looking wobbly because of noise introduced in chaining multiple models. 
    * A recurrent network approach (Gregor et al., 2015) and a deconvolution network approach (Dosovitskiy et al., 2014) have also recently had some success with generating natural images. However, they have not leveraged the generators for supervised tasks.

* non-parametric

  * do matching from a database of existing images

Approach & Architecture
#######################
Until LAPGAN (Denton et al., 2015) appeared GANs using CNNs to model images was not scalable. LAPGAN is an alternative approach to iteratively upscale low resolution generated images which can be modeled more reliably.

* Used convolutional net (Springenberg et al., 2014) which replaces :red:`deterministic spatial pooling` functions (such as maxpooling) with strided convolutions, allowing the network to learn its own spatial downsampling/upsampling. Used in generators and discriminators.

* Eliminated fully connected layers on top of convolutional features. (Mordvintsev et al.) used this approach in their art image classifiers with global average pooling.

* :red:`Batch Normalization` (Ioffe & Szegedy, 2015) which stabilizes learning by normalizing the input to each unit to have zero mean and unit variance. This helps deal with training problems that arise due to poor initialization and helps gradient flow in deeper models. Applying batchnorm to all layers however, resulted in sample oscillation and model instability. This was avoided by not applying batchnorm to the generator output layer and the discriminator input layer.

* Architecture guidelines for stable Deep Convolutional GANs

  • Replace any pooling layers with strided convolutions (discriminator) and fractional-strided convolutions (generator).
  • Use batchnorm in both the generator and the discriminator.
  • Remove fully connected hidden layers for deeper architectures.
  • Use ReLU activation in generator for all layers except for the output, which uses Tanh.
  • Use LeakyReLU activation in the discriminator for all layers.

Vector arithmetic for visual concepts
#####################################
.. figure:: /images/deep_learning/Vector_arithmetic_for_visual_concepts.png
   :align: center
   :alt: alternate text
   :figclass: align-center

.. _paper_2: https://github.com/YoungxHelsinki/papers/blob/b3ce367a97973b679d35b09baabb1320fd668a76/papers/UNSUPERVISED%20REPRESENTATION%20LEARNING%20WITH%20DEEP%20CONVOLUTIONAL%20GENERATIVE%20ADVERSARIAL%20NETWORKS.pdf

.. _ Goodfellow_et_al_2014: https://github.com/YoungxHelsinki/papers/blob/b3ce367a97973b679d35b09baabb1320fd668a76/papers/%20Generative%20Adversarial%20Nets.pdf