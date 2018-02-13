=======================
Bayesian – Fundamentals
=======================

.. contents::
    :local:
    :depth: 2

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


Bayes' Theorem
==============
Bayes' theorem describes probabilities of an event, based on prior knowledge – called *hypothesis* in Bayesian inference – that might be related to the event – called *evidence* in Bayesian inference.

.. math::
  \begin{align}
  P(A|B) &= \frac{P(B|A)}{P(B)}  \times P(A) \\
  \text{posterior} &= \frac{\text{Likelihood}}{\text{Marginal likelihood/Evidence}} \times \text{prior}
  \end{align}

.. figure:: /images/bayesian/bayesian_outline.png
   :align: center
   :alt: alternate text
   :figclass: align-center

   < `Yee Whye Teh: On Bayesian Deep Learning and Deep Bayesian Learning`_ >

.. _`Yee Whye Teh: On Bayesian Deep Learning and Deep Bayesian Learning`: https://youtu.be/9saauSBgmcQ?t=374


Bayesian vs. Frequentist
########################

* In Probability domain
  They all use Bayes' formula when a prior :math:`p(\theta)` is known.

* In Statistics domain
  In statistics prior is unknown and it's where the two diverge.

  * Bayesians: they need a prior, so they develop one from the best information they have.
  * Frequentists: They draw inferences from likelihood func.

.. figure:: /images/bayesian/Bayes_theorem_tree_diagrams.svg
  :align: center
  :alt: alternate text
  :figclass: align-center

  < Using Bayes' theorem one can inverse conditional probabilities. The red items are new information. Source: `Gnathan87 CC0 <https://commons.wikimedia.org/w/index.php?curid=15833490>`_ > 


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

------------------------

Bayesian Inference
==================
Bayesian inference is just one application of Bayes' theorem. We use it when we don't have as much data as you wish and want to maximize your predictive strenth. The bayes' theorem could be interpreted in the following way.

.. math::
  P(H|E) = \frac{P(E|H)P(H)}{P(E)}

* :math:`H`: Hypothesis. 
  
  * A hypothesis is a possible answer to the question being asked. It can be affected by new data or Evidence, :math:`E`. There can be multiple hypotheses and our job is to get the best one.

* :math:`E`: Evidence. 
  
  * It's new data observed.

* :math:`P(H)`: Prior probability. 
  
  * The probability of the hypothesis **before** the current evidence/data/:math:`E` is observed. It is the initial degree of belief in :math:`H`.

* :math:`P(H|E)`: Posterior probability. 

  * The probability of the hypothesis **after** the current evidence/data/:math:`E` is observed. This is what we want to know ultimately. The higher the posterior is our hypothesis well-fits to new data. It is a degree of belief having accounted for :math:`E`.

* :math:`P(E|H)`: Likelihood. 
  
  * The probability of current evidence/data/:math:`E` given hypothesis. It's just another fancy name for conditional probability i.e., :math:`P(A|B)` could be read as *"How A is likely to occur given B?"*

* :math:`P(E)`: Marginal likelihood or model evidence. 
  
  * This factor is the same for all hypotheses i.e., this does not enter into determining the relative probabilities of different hypotheses. 

The Bayes' rule can be rewritten as 

.. math::
  P(H|E) = \frac{P(E|H)}{P(E)}\cdot{P(H)}

where the factor :math:`\frac{P(E|H)}{P(E)}` represents the impact/support of :math:`E` on :math:`H`. This statement may be confusing but if one looks at the theorem it becomes obvious.

.. math::
  \begin{align}
  P(H|E) &\propto \frac{P(E|H)}{P(E)}, \quad \text{AND} \\
  P(H|E) &\propto P(H) \quad \text{i.e.,} \\
  \end{align}

.. math:: \text{Both $P(H)$ and $\frac{P(E|H)}{P(E)}$ are factors of the posterior}


Everyday-life example
#####################
I found a very intuitive example which is about inferring gender of a person with long hair [3]_. Suppose you saw a person dropped his/her wallet. You try to tell the person about it. The person is facing away from you and has long hair. You'd assume it's a woman because women generally have long hair while men don't. 

We can build 

* hypothesis/prior :math:`P(H)`, *"is a woman"*
* data/evidence :math:`P(E)`, *"has long hair"*

Let's make a naive assumption. There are 100 people in a shopping mall building. 50% of them are women and every woman has long hair. 10 men have long hair. Then we get

* likelihood :math:`P(E|H) = \frac{50}{50} = 1.0`
* posterior :math:`P(H|E) = \frac{50}{60} = 0.83`

In the shopping mall, the hypothesis is strong given the evidence.

With Bayes' theorem, you can update the degree of belief by updating prior knowledge/hypothesis. Let's update our naive assumption. Now we are in a rock festival with full of Finnish heavy metal bands. Virtually **everyone** has long hair. There are 100 people in the room, 90 men and 10 women. We now get

* hypothesis/prior :math:`P(H) = 0.1`, 
* data/evidence :math:`P(E) = 1.0`
* likelihood :math:`P(E|H) = \frac{10}{10} = 1`
* posterior :math:`P(H|E) = \frac{10}{100} = 0.1`

In the rock festival, the hypothesis is weak given the evidence.

If you're still confused with basic probability theory, we can talk about other concepts using the same example as well.

.. rubric:: Joint probability

Joint probability literally means *"joint"* probability such as *“What is the probability that someone is a woman with short hair?”*, :math:`P(woman)\cdot{P(short-haired|woman)}`.

.. rubric:: Marginal probability

Marginal probability is just a sum of probabilities. Let's say we want to get marginal probability, *"What is the probability that a person has short hair?"*. It is expressed as

.. math:: P(\text{person has short hair}) = P(\text{is female $\cap$ has short hair}) + P(\text{is male $\cap$ has short hair}) 
----------------------------------------------------------------------------------------

.. rubric:: References

.. [1] https://en.wikipedia.org/wiki/Bayesian_inference
.. [2] https://en.wikipedia.org/wiki/Bayes%27_theorem
.. [3] https://brohrer.github.io/how_bayesian_inference_works.html