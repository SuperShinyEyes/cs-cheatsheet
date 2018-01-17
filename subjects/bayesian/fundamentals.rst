=======================
Fundamentals – Bayesian
=======================

Probabilistic modeling
======================

The goal of probabilistic modeling:

* Classify the samples into groups
* Create prediction for future observations
* Select between competing hypothesis
* Estimate a parameter, such as the mean of a population

How do we run probabilistic modeling?

#. Models: Bayesian networks, Sparse Bayesian linear regression, Gaussian mixture models, latent linear models
#. Methods for inference: max likelihood, max a posteriori(MAP), Laplace approx., expectation maximization(EM), Variational Bayes(VB), Stochastic variational inference(SVI)
#. And choose a way to select different models based on your inference and update your model!


Bayesian Theory
===============

.. figure:: /images/bayesian/bayesian_outline.png
   :align: center
   :alt: alternate text
   :figclass: align-center

   < `Yee Whye Teh: On Bayesian Deep Learning and Deep Bayesian Learning`_ >

.. _`Yee Whye Teh: On Bayesian Deep Learning and Deep Bayesian Learning`: https://youtu.be/9saauSBgmcQ?t=374

Terminologies
#############

* Data: The data are results of the experiment.
* Hypothesis: A possible answer to the question being asked.
* Likelihood: The likelihood given a hypothesis is the probability of data given the hypothesis. Likelihood is **NOT** a probability of the hypothesis.
* Prior of the hypothesis: probability of the hypothesis prior to collecting data.
* Posterior of the hypothesis: probability of the hypothesis given the data.

Bayesian vs. Frequentist
########################

* In Probability domain
  They all use Bayes' formula when a prior :math:`p(\theta)` is known.

* In Statistics domain
  In statistics prior is unknown and it's where the two diverge.

  * Bayesians: they need a prior, so they develop one from the best information they have.
  * Frequentists: They draw inferences from likelihood func.

Conjugate priors
################

Suppose we have data with likelihood function :math:`p(x|\theta)` depending on a hypothesized parameter. Also suppose the prior distribution for :math:`\theta` is one of a family of parameterized distributions. If the posterior distribution for :math:`\theta` is in this family then we say the prior is a conjugate prior for the likelihood.

Examples
^^^^^^^^
* *Beta* is conjugate to Bernoulli
* Gaussian is conjugate to Gaussian
* Any exponential family has a conjugate prior

==================================================  ====================================  ====  ====================================  ==================================================
Prior                                               Hypothesis                            data  Likelihood                            Posterior
==================================================  ====================================  ====  ====================================  ==================================================
:math:`beta(a,b)`                                   :math:`\theta \in [0,1]`              x     :math:`Bernoulli(\theta)`             :math:`beta(a+1, b)` or :math:`beta(a, b+1)`
:math:`beta(a,b)`                                   :math:`\theta \in [0,1]`              x     :math:`Binomial(N, \theta)`           :math:`beta(a+x, b+N-x)`
:math:`beta(a,b)`                                   :math:`\theta \in [0,1]`              x     :math:`geometric(\theta)`             :math:`beta(a+x, b+1)`
:math:`\mathcal{N}(\mu_{prior}, \sigma_{prior}^2)`  :math:`\theta \in (-\infty,\infty)`   x     :math:`\mathcal{N}(\theta,\sigma^2)`  :math:`\mathcal{N}(\theta_{post},\sigma_{post}^2)`
==================================================  ====================================  ====  ====================================  ==================================================
