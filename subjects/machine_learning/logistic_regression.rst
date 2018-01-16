===================
Logistic Regression
===================

Used for classification. We want :math:`\theta \leq h_\theta (x) \leq 1`.

.. math::
   h_\theta(x) = g(\theta^T x) = g(z) = \frac{1}{1 + e^z} \Rightarrow h_\theta(x) = \frac{1}{1 + e^{-\theta^T x}}

:math:`\frac{1}{1 + e^{-\theta^T x}}` is a `sigmoid/logistic function <sigmoid_>`_

.. figure:: /images/machine_learning/Logistic-curve.svg
   :align: center
   :alt: alternate text
   :figclass: align-center

   < Logistic function plot >

.. _sigmoid: https://en.wikipedia.org/wiki/Sigmoid_function

The idea of logistic regression is

* Suppose predict :math:`y = 1` if :math:`h_\theta (x) \geq 0.5`
* Suppose predict :math:`y = 0` if :math:`h_\theta (x) < 0.5`


Cost Function
-------------

.. math::
      \text{cost}(h_\theta(x), y) = \left\{
        \begin{array}{lr}
          - \log(h_\theta(x))  & \text{if $y=1$ }\\
          - \log(1 - h_\theta(x))  & \text{if $y=0$ }
        \end{array}
      \right.

We use a separate cost function for logistic regression which differs from linear regression because otherwise it will be too wavy; cause too many local optima.

.. math::
   J(\theta) = \frac{1}{m} \sum^{m}_{i=1} \text{cost}(h_\theta(x^i), y^i)


Simplified Logistic Regression Cost Function
############################################

.. math ::
   \text{cost}(h_\theta(x^i), y^i) = -y \log(h_\theta(x)) - (1-y) \log(1-h_\theta(x))

   J(\theta) = - \frac{1}{m} [ \sum^{m}_{i=1} y^i \log(h_\theta (x^i)) + (1-y^i)\log(1-h_\theta(x^i))]


Logistic Regression Cost Function Gradient Descent
##################################################
To minimize :math:`J(\theta)`, repeat until convergence:

.. math::
   \theta_j := \theta_j - \alpha \sum^{m}_{i=1} (h_\theta (x^i) - y^i)x_j^i

This algorithm is identical to linear regression.


Normal Equation of Gradient Descent
###################################

.. math::
   \theta := \theta - \frac{\alpha}{m}X^T (g(X\theta) - \vec{y})


Regularization
--------------

.. math::
   J(\theta) = - \frac{1}{m} \sum^{m}_{i=1} [y^i \log(h_\theta (x^i)) + (1-y^i)\log(1-h_\theta(x^i))] + \frac{\lambda}{2m} \sum^{n}_{j=1}\theta_j^2