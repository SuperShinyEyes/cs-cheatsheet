=====
Basic
=====


Intro
=====
.. figure:: /images/deep_learning/NN_image.jpg
   :align: center
   :alt: alternate text
   :figclass: align-center

   < Simple Neural Network with 1 hidden layer. In the hidden layer, a sub-digit represents neuron unit number, a exponential-digit layer number. >

:math:`\Theta^{(j)}`: matrix of weights controlling function mapping from layer *j* to layer *j+1*.

The values for each of the *activation* nodes is obtained as follows:

.. math::

   \text{3x4 matrix} \left\{
      \begin{array}{lr}
         a_1^{(2)} &= g(\Theta_{10}^{(1)}x_0 + \Theta_{11}^{(1)}x_1 + \Theta_{12}^{(1)}x_2 + \Theta_{13}^{(1)}x_3) \\
         a_2^{(2)} &= g(\Theta_{20}^{(1)}x_0 + \Theta_{21}^{(1)}x_2 + \Theta_{22}^{(1)}x_2 + \Theta_{23}^{(1)}x_3) \\
         a_3^{(2)} &= g(\Theta_{30}^{(1)}x_0 + \Theta_{31}^{(1)}x_3 + \Theta_{32}^{(1)}x_2 + \Theta_{33}^{(1)}x_3)
      \end{array}
    \right.

.. math::

   h_\Theta(x) &= a_1^{(3)}

   a_1^{(3)} &= g(\Theta_{10}^{(2)}a_0^{(2)} + \Theta_{11}^{(2)}a_1^{(2)} + \Theta_{12}^{(2)}a_2^{(2)} + \Theta_{13}^{(2)}a_3^{(2)})


As you can see each layer gets its own matrix of weights, :math:`\Theta^{(j)}`.

--------

If a network has :math:`S_j` units in layer *j* and :math:`S_{j+1}` units in layer *j+1*, then :math:`\Theta^{(j)}` will be of dimension :math:`S_{j+1} \times (S_j + 1)`. (1 is a bias node, :math:`x_0`)


Cost Function
=============

.. math::
   J(\theta) = - \frac{1}{m} \sum^{m}_{i=1} \sum^{K}_{k=1} [y_k^i \log((h_\theta (x^i))_k) + (1-y_k^i)\log(1-(h_\theta(x^i))_k)] \\
   + \frac{\lambda}{2m} \sum^{L-1}_{l=1} \sum^{S_l}_{i=1} \sum^{S_{l+1}}_{j=1} (\theta_{j,i}^{(l)})^2

Here :math:`\lambda` is regularized term.


Epochs, batches and iterations
==============================

* Epoch: A single through of an entire dataset
* Batch: A single dataset can be divided into batches.
* Iteration: A number of batches to complete an epoch.

.. math::
  \text{A number of dataset $=$ Batch $\times$ Iteration}


Back-propagation
================
Method for computing the gradient for training the network. It's contrary to stockastic gradient descent which is used to perform learning using the gradient. It's an algorithm that computes the chain rule of calculus , with a specific order of operations that is highly efficient [Goodfellow-et-al]_. It modifies the connection weight parameters layer-by-layer starting from the output layer and progressing toward the input layer.

.. [Goodfellow-et-al] Deep Learning
