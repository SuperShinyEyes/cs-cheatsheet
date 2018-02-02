============
Optimization
============

Empirical Risk Minimization
===========================
This part is mostly from `Deep Learning by Goodfellow et al. <Deep Learning_>`_

.. _Deep Learning: http://www.deeplearningbook.org/contents/optimization.html

In ML, we want to optimize  performance measure :math:`P` by reducing a different cost function :math:`J(\theta)`.

.. math::

  J(\theta) = \mathbb{E}_{(x,y) {\raise.17ex\hbox{$\scriptstyle\sim$}} \hat{p}_{data}} L(f(x;\theta),y)

* :math:`L`: per-example loss function.
* :math:`f(x;\theta)`: predicted output when the input is :math:`x`.
* :math:`\hat{p}_{data}`: empirical distribution
* :math:`y`: target output in the supervised learning

The above defines an objective function with respect to the training set. We would prefer to minimize the corresponding objective function where the expectation is taken across *the data-generating distribution* :math:`p_{data}` rather than just over the finite training set:

.. math::

  J^*(\theta) = \mathbb{E}_{(x,y) {\raise.17ex\hbox{$\scriptstyle\sim$}} p_{data}} L(f(x;\theta),y)

The above quantity is called the **risk**.  **NOTE**: here we have a true distribution :math:`p_{data}`. In ML we only have empirical distribution so we have to replace it with the empirical distribution :math:`\hat{p}_{data}` defined by the training set. Minimize the **empirical risk**

.. math::

  \mathbb{E}_{(x,y) {\raise.17ex\hbox{$\scriptstyle\sim$}} p_{data}} [L(f(x;\theta),y)] =
  \frac{1}{m} \sum_{i=1}^m L(f(x^{(i)};\theta),y^{(i)})

where *m* is the number of training examples.

Remember, empirical risk minimization is prone to overfitting. In DL, we rarely use empirical risk minimization but algorithms based on gradient descent.

-----------------------------------------------------------------------------------------


Batch and Minibatch Algorithms
==============================

Optimization algorithms that use the entire training set are called **batch** or **deterministic** gradient methods, because they process all the training examples simultaneously in a large batch. Algorithms that use only a **single** example at a time are called **stochastic**. **However**, most algorithms used for DL fall somewhere in between, using more than one but fewer than all the training examples. They are called **minibatch stochastic** or simply **stochastic** methods.

Minibatch size selection factors
################################

.. warning::
  
  Ask a TA for detailed explanation.


* Larger batches provide a more accurate estimate of the gradient, but with less than linear returns.

* Multicore architectures are usually underutilized by extremely small batches. For a fix, use an absolute minimum batch size.

* If all examples in the batch are to be processed in parallel, then the amount of memoery scales with the batch size. Usually this is the limiting factor in batch size.

* Some kinds of HW achieve better runtime with specific sizes of arrays. Especially when using GPUs, it is common for power of 2 batch sizes to offer better runtime(typically from 32 to 256. You may try 16 for large models).

* Small batches can offer a regularizing effect (Wilson and Martinez, 2003), perhaps due to the noise they add to the learning process. Generalization error is often best for a batch size of 1. Training with such a small batch size might require a small learning rate to maintain stability because of the high variance in the estimate of the gradient. The total runtime can be very high as a result of the need to make more steps.

