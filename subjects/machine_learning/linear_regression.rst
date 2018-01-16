=================
Linear Regression
=================

Cost Function
-------------

Gradient Descent
################

Repeat until convergence

.. math::
   \begin{align}
      \theta_j &:= \theta_j - \alpha \frac{\delta}{\delta\theta_j}J(\theta_0, \theta_1),   &\text{(for j = 0 and j = 1)} \nonumber
   \end{align}

Be careful not to update :math:`\theta` separately. Update them all together at the end of each loop. i.e.,

.. math::
    temp_0 &:= \theta_0 - \alpha  \frac{\delta}{\delta\theta_j}J(\theta_0, \theta_1) 

    temp_1 &:= \theta_1 - \alpha  \frac{\delta}{\delta\theta_j}J(\theta_0, \theta_1)

    \theta_0 &:= temp_0 
      
    \theta_1 &:= temp_1 


Bivariate Gradient Descent
##########################

.. math::
   \begin{split}
      \frac{\delta}{\delta\theta_j}J(\theta_0, \theta_1) &= \frac{\delta}{\delta\theta_j}J(\theta_0, \theta_1) \frac{1}{2m} \sum^{m}_{i=1}(h_\theta(x^i) - y^i)^2 \\
      &= \frac{\delta}{\delta\theta_j}J(\theta_0, \theta_1) \frac{1}{2m} \sum^{m}_{i=1}(\theta_0 + \theta_1 x^i - y^i)^2 
   \end{split}

.. math::
   \begin{array}{ll}
      \theta_0 \Rightarrow j = 0 : \frac{\delta}{\delta\theta_j}J(\theta_0, \theta_1) = \frac{1}{m} \sum^{m}_{i=1}(h_\theta(x^i) - y^i) \\
      \theta_1 \Rightarrow j = 1 : \frac{\delta}{\delta\theta_j}J(\theta_0, \theta_1) = \frac{1}{m} \sum^{m}_{i=1}(h_\theta(x^i) - y^i)
   \end{array}


Repeat until convergence

.. math::
   \begin{array}{ll}
      \theta_0 &:= \theta_0 - \alpha \frac{1}{m} \sum^{m}_{i=1}(h_\theta(x^i) - y^i) x_0^i \\  \nonumber
      \theta_1 &:= \theta_1 - \alpha \frac{1}{m} \sum^{m}_{i=1}(h_\theta(x^i) - y^i) x_1^i
   \end{array}

Here :math:`x_0^i` is 0.


Multivariate gradient descent
###############################

Repeat until convergence

.. math::

      \theta_j &:= \theta_j - \alpha \frac{1}{m} \sum^{m}_{i=1}(h_\theta(x^i) - y^i) x_j^i \nonumber



Normal Equation
###############
In linear gression, instead of a loop as above, gradient descent can be expressed in a non-loop equation:

.. math::
   \theta = (X^TX)^{-1}X^Ty


Regularization
--------------
* Overfitting: low bias, high variance
* Underfitting: high bias, low variance

Watch `Andrew Ng's lecture <https://www.coursera.org/learn/machine-learning/lecture/QrMXd/regularized-linear-regression>`_.

Example
#######
Say we want to make the following function more quadratic:

.. math::
   H = \theta_0 + \theta_1x + \theta_2x^2 + \theta_3x^3 + \theta_4x^4

Without actually removing :math:`\theta_3x^3 + \theta_4x^4` or changing the form of :math:`H`, we can modify our cost function:

.. math::
   \text{min}_\theta \frac{1}{2m} \sum^{m}_{i=1}(h_\theta(x^i) - y^i)^2 + 1000 \theta_3^2 + 1000 \theta_4^2

We could also regularize all of our theta parameters in a single summation 

.. math::
   \text{min}_\theta \frac{1}{2m} \sum^{m}_{i=1}(h_\theta(x^i) - y^i)^2 + \lambda \sum^{n}_{j=1}\theta_j^2

The square in the second sum comes from the first sum.


Regularization - Gradient Descent
#################################
Repeat until convergence

.. math::
   \begin{align}
      \theta_0 &:= \theta_0 - \alpha \frac{1}{m} \sum^{m}_{i=1}(h_\theta(x^i) - y^i) x_0^i, &\text{(Don't penalize the intercept $\theta_0$)} \nonumber 
   \end{align}

.. math::
   \begin{align}
   \theta_j &:= \theta_j - \alpha [(\frac{1}{m} \sum^{m}_{i=1}(h_\theta(x^i) - y^i) x_j^i) + \frac{\lambda}{m}\theta_j],   & j \in {1,2,...,n} \nonumber
   \end{align}

:math:`\frac{\lambda}{m}` is a *regularization performer*.

The above can be represented as:

.. math::
   \theta_j &:= \theta_j(1 - \alpha\frac{\lambda}{m}) - \alpha\frac{1}{m} \sum^{m}_{i=1}(h_\theta(x^i) - y^i) x_j^i

:math:`1 - \alpha\frac{\lambda}{m}` is always less than 1. Thus, regularized.


Regularization - Normal Equation
################################


.. math::
   \begin{align}
   X &= 
   \begin{bmatrix}
       (x^1)^T \\
       \vdots\\
       (x^m)^T 
   \end{bmatrix}, & \text{size is $(m)\times(n+1)$}
   \end{align}
   
.. math::
   \begin{align}
   \vec{y} &= 
   \begin{bmatrix}
       y^1 \\
       \vdots\\
       y^m 
   \end{bmatrix}, & \text{size is $(m)\times(1)$}
   \end{align}   

.. math::
   \theta = (X^TX + \lambda L)^{-1}X^T\vec{y}

where L is a pseudo-diagonal matrix of 

.. math::
   \begin{align}
   L &= 
   \begin{bmatrix}
       0       & 0 & 0 & \dots & 0 \\
       0       & 1 & 0 & \dots & 0 \\
       0       & 0 & 1 & \dots & 0 \\
       \hdotsfor{5} \\
       0       & 0 & 0 & \dots & 1
   \end{bmatrix}, & \text{size is $(n+1)\times(n+1)$}
   \end{align}

If :math:`m \leq n`, then :math:`X^TX` is non-invertable and so is :math:`(X^TX + \lambda L)`.
