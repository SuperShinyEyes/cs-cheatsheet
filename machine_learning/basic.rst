=====
Basic
=====

Gradient Descent
----------------

Repeat until convergence

.. math::
   \begin{align}
      \theta_j &:= \theta_j - \alpha \frac{\delta}{\delta\theta_j}J(\theta_0, \theta_1),   &\text{(for j = 0 and j = 1)} \nonumber
   \end{align}

Be careful not to update :math:`\theta` separately. Update them all together at the end of each loop.

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
