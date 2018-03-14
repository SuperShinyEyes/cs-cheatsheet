=====================
Variational Inference
=====================
*Variational* means you vary parameters in each step. 


Mixture models
==============
* Probabilisitically-grounded way of doing soft clustering contrast to k-mean.
* Each cluster: a generative model(Gaussian or multinomial)
* Parameters(e.g. mean/covariance are unknown)

EM(Expectation Maximization) algorithm automatically discover all parameters for the K "sources".


Gaussian mixture models
=======================
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


EM algorithm
============
EM solves a chicken and egg problem, [2]_

* Need :math:`\left( \mu _ { a ^ { \prime } } \sigma _ { a } ^ { 2} \right)` and :math:`\left( \mu _ { b ^ { \prime } } \sigma _ { b } ^ { 2} \right)` to guess source of points
* Need to know the sources to estimate :math:`\left( \mu _ { a ^ { \prime } } \sigma _ { a } ^ { 2} \right)` and :math:`\left( \mu _ { b ^ { \prime } } \sigma _ { b } ^ { 2} \right)`


Idea
####
1. Start with two randomly placed Gaussians :math:`\left( \mu _ { a ^ { \prime } } \sigma _ { a } ^ { 2} \right)` and :math:`\left( \mu _ { b ^ { \prime } } \sigma _ { b } ^ { 2} \right)`
2. (E-step) for each point :math:`P \left( b | x _ { i } \right)` does it look like it came from b?
3. (M-step) adjust :math:`\left( \mu _ { a ^ { \prime } } \sigma _ { a } ^ { 2} \right)` and :math:`\left( \mu _ { b ^ { \prime } } \sigma _ { b } ^ { 2} \right)` to fit points assigned to them
4. Iterate until convergence


.. rubric:: Reference

.. [1] https://brilliant.org/wiki/gaussian-mixture-model/
.. [2] https://www.youtube.com/watch?v=REypj2sy_5U