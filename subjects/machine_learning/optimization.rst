============
Optimization
============

Empirical Risk Minimization
===========================

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

The above quantity is called the **risk** and  **NOTE**: here we have a true distribution :math:`p_{data}}`. In ML we only have empirical distribution so we have to replace it with the empirical distribution :math:`\hat{p}_{data}}` defined by the training set. Minimize the **empirical risk**

.. math::

  \mathbb{E}_{(x,y) {\raise.17ex\hbox{$\scriptstyle\sim$}} p_{data}} [L(f(x;\theta),y)] =
  \frac{1}{m} \sum_{i=1}^m L(f(x^{(i)};\theta),y^{(i)})

where *m* is the number of training examples.



* :math:``:
* :math:``:
* :math:``:
