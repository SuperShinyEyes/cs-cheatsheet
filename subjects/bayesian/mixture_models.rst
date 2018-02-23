=============
Mixture Model
=============

Gaussian mixture models are a probabilistic model for representing normally distributed subpopulations within an overall population. Mixture models in general don't require knowing which subpopulation a data point belongs to, allowing the model to learn the subpopulations automatically. Since subpopulation assignment is not known, this constitutes a form of unsupervised learning. [1]_


.. figure:: /images/bayesian/gmm.png
  :align: center
  :alt: alternate text
  :figclass: align-center

  < (Left) Fit with one Gaussian distribution (Right) Fit with Gaussian mixture model with two components [1]_ >

**One-dimensional Model**

.. math::
  \begin{align} p(x) &= \sum_{i=1}^K\phi_i \mathcal{N}(x \;|\; \mu_i, \sigma_i)\\ \mathcal{N}(x \;|\; \mu_i, \sigma_i) &= \frac{1}{\sigma_i\sqrt{2\pi}} \exp\left(-\frac{(x-\mu_i)^2}{2\sigma_i^2}\right)\\ \sum_{i=1}^K\phi_i &= 1 
  \end{align}

**Multi-dimensional Model**

.. math::
  \begin{align} p(\vec{x}) &= \sum_{i=1}^K\phi_i \mathcal{N}(\vec{x} \;|\; \vec{\mu}_i, \Sigma_i)\\ \mathcal{N}(\vec{x} \;|\; \vec{\mu}_i, \Sigma_i) &= \frac{1}{\sqrt{(2\pi)^K|\Sigma_i|}} \exp\left(-\frac{1}{2}(\vec{x}-\vec{\mu}_i)^\mathrm{T}{\Sigma_i}^{-1}(\vec{x}-\vec{\mu}_i)\right)\\ \sum_{i=1}^K\phi_i &= 1 
  \end{align}








.. rubric:: Reference

.. [1] https://brilliant.org/wiki/gaussian-mixture-model/