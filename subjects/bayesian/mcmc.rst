========================
Markov Chain Monte Carlo
========================

Introduction
============

  From 1990 a revolution took place in Bayesian computing. The main problem in the application of Bayesian methods was until then that computation was difficult. With conjugate priors analytical results could sometimes be obtained, but in complex models even that did not bring much relief. With the increasing speed of computers however, simulation methods became more and more feasible. Methods like ”importance sampling ”. But in 1990 the ”Markov chain Monte Carlo ” (MCMC) methods were found. **A Markov chain means a simulation where the next draw only depends on the previous one.** The technique has in itself nothing to do with Bayesian analysis. The question is whether it is possible to simulate from a complex distribution. As in Bayesian inference the posterior is known if prior and likelihood are known it is possible to write down the formula for it. And if there is a trick to simulate given the formula, that is sufficient. [1]_


Schemes
=======

Gibbs sampler
#############

Algorithm
^^^^^^^^^

* Suppose there are parameters :math:`\theta_1 \cdots \theta_n` and that the distribution of each :math:`\theta_i`, conditional upon the other :math:`\theta`'s is known.
* Draw in each step :math:`\theta_i | \theta_1 \cdots \theta_{i-1}, \theta_{i+1} \cdots \theta_n`, and cycle through :math:`\theta_i` (so :math:`\theta_1, \theta_2 \cdots \theta_n, \theta_1, \theta_2 \cdots`)

This is very efficient but requires the conditional distribution must be known. This preperty requires a lot of work and in some cases it is not analytically possible, the main problem being the normalizaing constant.

Metropolis-Hastings(aka MCMCMC)
###############################

Algorithm
^^^^^^^^^
* Start with some :math:`\theta_1(i=1)`
* The proposal for :math:`\theta_{i+1}, \theta_{i+1}^{p}` is drawn from uniform(:math:`\theta_{i}-a, \theta_{i}+a`)
* If :math:`f(\theta_{i+1}^{p}) > f(\theta_{i})` then the proposal is accepted: :math:`\theta_{i+1} := \theta_{i+1}^{p}` else then proposal is accepted with probability :math:`\alpha = f(\theta_{i+1}^{p}) \big/ f(\theta_{i})`. If not accepted, :math:`\theta_{i+1} := \theta_{i}`

We only need likelihood and the prior unlike Gibbs. The posterior normalizing constant, :math:`P(E)` denominator in Bayes' theorem, can be unknown.


----------------------------------------------------------------------------------

.. rubric:: References

.. [1] https://github.com/YoungxHelsinki/papers/blob/961603b8eccf5352580871dd43052164ae540962/tutorials/primer.pdf