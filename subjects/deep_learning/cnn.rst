============================
Convolutional Neural Network
============================

Intro
=====
.. figure:: /images/deep_learning/CNN_mechamism.jpg
   :align: center
   :alt: alternate text
   :figclass: align-center

   < Mechamisom of CNN. >

CNN is robust for images compared to Regular Neural Nets because images are huge! A single image have millions of features and an image dataset can have millions of images as well.

Convolution operation
#####################



Convolution
===========

Pooling
=======

Data augmentation
=================
It is a technique used in computer vision for increasing training data set based on the given data, in order to build a better model. Keras(ImageDataGenerator), Tensorflow(tflearn.data_augmentation.DataAugmentation) and mxnet(Augmenter) have ready-made implementations

.. figure:: /images/deep_learning/data_augmentation1.png
   :align: center
   :alt: alternate text
   :figclass: align-center

   < Source: [Deep_Learning_Andrew_Ng]_ >

Common techniques [1]_
######################
* Scaling
* Translation
* Rotation (at 90 degrees)
* Rotation (at finer angles)
* Flipping
* Adding Salt and Pepper noise
* Lighting condition
* Perspective transform


Deconvolutional Network
=======================
*"A deconvnet can be thought of as a convnet model that uses the same components (filtering, pooling) but in reverse, so instead of mapping pixels to features does the opposite. In (Zeiler et al., 2011), deconvnets were proposed as a way of performing unsupervised learning. Here, they are not used in any learning capacity, just as a probe of an already trained convnet."* [2]_

In short, deconvolution layer is just a `transposed convolutional layer <https://datascience.stackexchange.com/a/12110>`_.

https://www.youtube.com/watch?v=ByjaPdWXKJ4&feature=youtu.be&t=16m59s

---------------------------------------------

.. rubric:: Reference

.. [Deep_Learning_Andrew_Ng] https://www.coursera.org/learn/convolutional-neural-networks/lecture/AYzbX/data-augmentation?authMode=login
.. [1] https://medium.com/ymedialabs-innovation/data-augmentation-techniques-in-cnn-using-tensorflow-371ae43d5be9
.. [2] https://github.com/YoungxHelsinki/papers/blob/4bc6eee3a68cb7da5277ff66ccefd8815a7f778d/papers/Visualizing%20and%20Understanding%20Convolutional%20Networks.pdf