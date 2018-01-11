=================
Linear Regression
=================

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
--------------------------

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
-----------------------------

Repeat until convergence

.. math::

      \theta_j &:= \theta_j - \alpha \frac{1}{m} \sum^{m}_{i=1}(h_\theta(x^i) - y^i) x_j^i \nonumber



Normal Equation
###############
In linear gression, instead of a loop as above, gradient descent can be expressed in a non-loop equation:

.. math::
   \theta = (X^TX)^{-1}X^Ty