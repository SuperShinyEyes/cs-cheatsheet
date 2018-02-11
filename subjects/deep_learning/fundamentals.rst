============
Fundamentals
============


Intro
=====
.. figure:: /images/deep_learning/NN_image.png
   :align: center
   :alt: alternate text
   :figclass: align-center

   < Representation of a linear classifier neural network. Source: `cs231n <http://cs231n.github.io>`_ >

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

Score function
==============
A function that takes an input and returns class scores. In linear classifier it would be :math:`Wx + b`.

Cost/Loss Function
==================
Loss function measures how good a given set of weights/parameters are compared to the ground truth labels in the training set. Popular loss functions are

* relu
* svm
* softmax
* sigmoid
* tanh


.. math::
   J(\theta) = - \frac{1}{m} \sum^{m}_{i=1} \sum^{K}_{k=1} [y_k^i \log((h_\theta (x^i))_k) + (1-y_k^i)\log(1-(h_\theta(x^i))_k)] \\
   + \frac{\lambda}{2m} \sum^{L-1}_{l=1} \sum^{S_l}_{i=1} \sum^{S_{l+1}}_{j=1} (\theta_{j,i}^{(l)})^2

Here :math:`\lambda` is regularized term.


Weight optimization
===================

Practically it often works better to compute the numeric gradient using the centered difference formula: :math:`[f(x+h) - f(x-h)] / 2 h` (`Wiki <https://en.wikipedia.org/wiki/Numerical_differentiation>`_).

Gradient Descent
################

Here are pseudo-code for vanila GD and SGD.

.. code-block:: python

  # Source: http://cs231n.github.io/optimization-1/
  # Vanilla Gradient Descent
  while True:
    weights_grad = evaluate_gradient(loss_fun, data, weights)
    weights += - step_size * weights_grad # perform parameter update

.. code-block:: python

  # Vanilla Minibatch Gradient Descent
  while True:
    data_batch = sample_training_data(data, 256) # sample 256 examples
    weights_grad = evaluate_gradient(loss_fun, data_batch, weights)
    weights += - step_size * weights_grad # perform parameter update

There two ways to find gradients

* numeric
* analytic

Numerical solution is simple but doens't give the exact solution but rather an approximation. Analytical solution is fast and give the exact solution but is error-prone as one could make a mistake during mathematical derivation. Therefore, in practice you'd use a gradient check(compare numerical and analytical solution).


Epochs, batches and iterations
==============================

* Epoch: A single through of an entire dataset
* Batch: A single dataset can be divided into batches.
* Iteration: A number of batches to complete an epoch.

.. math::
  \text{A number of dataset $=$ Batch $\times$ Iteration}


Back-propagation
================
It's a `recursive application of a chain rule along a computational graph to compute the gradients of all parameters <https://youtu.be/d14TUNcbn1k?t=7m44s>`_. It's contrary to stockastic gradient descent which is used to perform learning using the gradient. It's an algorithm that computes the chain rule of calculus , with a specific order of operations that is highly efficient [Goodfellow-et-al]_. It modifies the connection weight parameters layer-by-layer starting from the output layer and progressing toward the input layer.

In `a stanford lecture <https://www.youtube.com/watch\?v\=d14TUNcbn1k\&index\=4\&list\=PL3FW7Lu3i5JvHM8ljYj-zLfQRF3EO8sYv>`_  about backpropagation the TA shows that analytical gradient search could be represented as a computational graph.


.. figure:: /images/deep_learning/computational_graph.png
   :align: center
   :alt: alternate text
   :figclass: align-center

   < Source: Stanford cs231n >

Example
#######

.. figure:: /images/deep_learning/back_propagatino_example.jpg
   :align: center
   :alt: alternate text
   :figclass: align-center

   < In the figure, the upper digits are the values of the nodes and the lower its gradient/derivative. The node values are filled by forward pass and the gradients by back propagation. Source: Stanford cs231n >

.. math::
  \begin{align}
  \frac{\delta f}{\delta x} &= \frac{\delta q}{\delta x} \frac{\delta f}{\delta q} = 1 \times -4 = -4 \\
  \frac{\delta f}{\delta y} &= \frac{\delta q}{\delta y} \frac{\delta f}{\delta q} = 1 \times -4 = -4 \\
  \frac{\delta f}{\delta z} &= -2 + 5 = 3 
  \end{align}

So what do we do with the **local gradients** in the computational graph? We send the upstream gradient going down and multiply it by the local gradients in order to get the gradient respect to the input.

.. figure:: /images/deep_learning/back_propagatino_local_gradient.jpg
   :align: center
   :alt: alternate text
   :figclass: align-center

Here's a bit more complicated example.

.. figure:: /images/deep_learning/back_propagatino_example2.png
   :align: center
   :alt: alternate text
   :figclass: align-center

   < Source: Stanford cs231n >

I will show the back propagation step-by-step.

.. math::
  \frac{\delta f}{\delta f} = 1  \\
  \frac{\delta q}{\delta x} = \frac{\delta 1/x}{\delta x} = \frac{-1}{x^2} = \frac{-1}{1.37^2} = -0.53  \\
  \frac{\delta w}{\delta x} = \frac{\delta c + x}{\delta x} = 1, 1 \times -0.53 = -0.53  \\
  \frac{\delta e}{\delta x} = \frac{\delta e^x}{\delta x} = e^x = e^{-1} = 0.37, 0.37 \times -0.53 = -0.2  \\
  \frac{\delta r}{\delta x} = \frac{\delta -x}{\delta x} = -1, -1 \times -0.2 = 0.2  \\
  \frac{\delta t}{\delta x} = \frac{\delta c + x}{\delta x} = 1, 1 \times 0.2 = 0.2  \\
  \frac{\delta y}{\delta x} = \frac{\delta c + x}{\delta x} = 1, 1 \times 0.2 = 0.2  \\
  \frac{\delta u}{\delta x} = \frac{\delta x_0x}{\delta x} = x_0 = -1, -1 \times 0.2 = -0.2  \\
  \frac{\delta p}{\delta x} = \frac{\delta w_0x}{\delta x} = w_0 = 2, 2 \times 0.2 = 0.4  \\
  \frac{\delta s}{\delta x} = \frac{\delta x_1x}{\delta x} = x_1 = -2, -2 \times 0.2 = -0.4  \\
  \cdots

However, there isn't only one way to draw a computational graph. One can decide the level of complexity like in the bottom, in which it substitutes a sigmoid gate with 4 nodes on the right.:

.. figure:: /images/deep_learning/back_propagatino_example3.png
   :align: center
   :alt: alternate text
   :figclass: align-center

   < Source: Stanford cs231n >

Patterns in back propagation
############################
In the example you could observe a **pattern** in the back propagation. The **add** gate distributes gradients. The **mul** gate switches scaler and multiply it to the upstream gradient. So in the above example for w0 local gradient it is :math:`0.2 \times -1`. **max** gate is interesting. It routes the gradient only to the max node.

.. figure:: /images/deep_learning/back_propagatino_example4.png
   :scale: 20%
   :align: center
   :alt: alternate text
   :figclass: align-center

   < Source: Stanford cs231n >

So to summarize:

======== ====================
Gates    Rules
======== ====================
add gate Gradient distributor
max gate Gradient router
mul gate Scaler switcher
======== ====================


Vectorized example
##################

.. figure:: /images/deep_learning/back_propagatino_example5.png
   :align: center
   :alt: alternate text
   :figclass: align-center

   < Source: Stanford cs231n >

The idea is the same with scalar example. For instance, in order to get the gradient of :math:`W`, you follow the **scaler switcher** rule.

.. code-block:: python
  
  np.array([0.2,0.4]).reshape(2,1).dot(np.array([0.44,.52]).reshape(1,2))


.. [Goodfellow-et-al] Deep Learning


Activation functions
====================
In NN, we use non-linear activation functions. `This excellent Stackoverflow answer <https://stackoverflow.com/a/9783865/3067013>`_ explains why we use non-linear activation functions.

  The purpose of the activation function is to introduce **non-linearity into the network**.

  In turn, this allows you to model a response variable (aka target variable, class label, or score) that varies non-linearly with its explanatory variables

  *non-linear* means that the output cannot be reproduced from a linear combination of the inputs (which is not the same as output that renders to a straight line--the word for this is affine).

  another way to think of it: without a non-linear activation function in the network, a NN, **no matter how many layers it had, would behave just like a single-layer perceptron**, because summing these layers would give you just another linear function (see definition just above).

Popular activation functions
############################

.. figure:: /images/deep_learning/activation_functions.png
   :align: center
   :alt: alternate text
   :figclass: align-center

   < Source: Stanford cs231n >

Learning rate vs. Momentum
==========================
When performing gradient descent, **learning rate** measures how much the current situation affects the next step, while **momentum** measures how much past steps affect the next step. [Quara-What-is-the-difference-between-momentum-and-learning-rate]_

.. [Quara-What-is-the-difference-between-momentum-and-learning-rate] https://www.quora.com/What-is-the-difference-between-momentum-and-learning-rate

.. figure:: /images/deep_learning/1obtV.gif
   :align: center
   :alt: alternate text
   :figclass: align-center

   < Momentum and other gradient descent techiques visualized. `Source <Stackoverflow_momentum_gif_>`_  >

.. _Stackoverflow_momentum_gif: https://stackoverflow.com/a/44225502/3067013


-----------------------------

