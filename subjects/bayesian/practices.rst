=========
Practices
=========

.. contents::
    :local:
    :depth: 2
    
Q. Conditional independence from Bayesian network
=================================================

.. figure:: /images/bayesian/ConditionalindependencefromBayesiannetwork.png
   :align: center
   :alt: alternate text
   :figclass: align-center

   < Source: Aalto course CS-E4820: Advanced probabilistic methods >

* :math:`A \perp\!\!\!\perp B | C` 
  
  * False. Collider C is in the path ACB and is in the conditioning set. 

* :math:`A \perp\!\!\!\perp B | \emptyset` 
  
  * True

* :math:`C \perp\!\!\!\perp E | B,D` 
  
  * False. Collider D is in the path CDAFE  and is in the conditioning set. 

* :math:`C \perp\!\!\!\perp D | A,B` 
  
  * False. Path C – D is not blocked. Therfore it is not conditionally dependent.

* :math:`B \perp\!\!\!\perp F | A,C` 
  
  * True 

* :math:`A \perp\!\!\!\perp E | D,F` 
  
  * False. In path ACBE, there is a collider C, and its descendent D is in the conditioning set. 


.. figure:: /images/bayesian/MarkovEquivalentGraph.png
  :scale: 50%
  :align: center
  :alt: alternate text
  :figclass: align-center

  < Markov Equivalent Graph >

--------------

Q. Burden of specification
==========================

.. figure:: /images/bayesian/Burden_of_specification.png
   :align: center
   :alt: alternate text
   :figclass: align-center

   < Source: Aalto course CS-E4820: Advanced probabilistic methods >

a.
##
In total there are :math:`2^5 = 32` combinations of parameters i.e., states. The last probability could be defined by :math:`1 - sum(31 states)`. Thus the distribution could be defined by 31 combinations of parameters.

b.
##
Let's have alphabets instead of :math:`x_i`.

.. math::
  p(a,b,c,d,e) = p(a)p(b|a)p(c|b)p(d|c)p(e|d)

The distribution needs 2+2+2+2+2 - 1 = 9 parameters. -1 is for the fact that you can deduce from the assumption the distribution sums to 1 and you can get the last probability by subtracting the rest from 1.

c.
##
.. math::
  p(a,b,c,d,e) = p(a)p(b|a)p(b|a)p(b|a)p(b|a)

The distribution needs 2+2 - 1 = 3 parameters.

-------------

Q. DAG representation
=====================

.. figure:: /images/bayesian/DAG_representation.png
   :align: center
   :alt: alternate text
   :figclass: align-center

   < Source: Aalto course CS-E4820: Advanced probabilistic methods >

Probability of severe headaches P(E = 1) depends only on the fact whether a person has a brain tumor (C) or not. On the other hand, if one knows the blood calcium level (B) and whether the person has a tumor or not (C), one can specify the probability of unconsciousness seizures P(D = 1). In this case, the probability of D doesn’t depend on the presence of the headaches (E) or (directly) on the fact whether the person has brain cancer or not (A). The probability of a brain tumor (C) depends directly only on the fact, whether the person has brain cancer or not (A).

.. figure:: /images/bayesian/DAG_answer.png
   :scale: 50%
   :align: center
   :alt: alternate text
   :figclass: align-center

   < DAG representation >


-----------------------------------------------------------------------------------------

Q. Derivation of the posterior distribution
===========================================
http://halweb.uc3m.es/esp/Personal/personas/mwiper/docencia/English/PhD_Bayesian_Statistics/ch3_2009.pdf
https://www.youtube.com/watch\?v\=0XD6C_MQXXE


Q. Multivariate Gaussian
========================
Reference: https://learnbayes.org/index.php?option=com_content&view=article&id=77%3Acompletesquare&catid=83&Itemid=479&showall=1&limitstart=


part a
######
We have a model,

.. math::
  \begin{eqnarray}
  \boldsymbol x_i&\stackrel{iid}{\sim}&\mbox{Multivariate Normal}_p(\boldsymbol\mu,\boldsymbol\Sigma)   \\
  \boldsymbol\mu&\sim&\mbox{Multivariate Normal}_p(\boldsymbol m_0,\boldsymbol S_0)
  \end{eqnarray}   \\

Here's the general Multivariate Gaussian Distrubution(MVN):

.. math::
  p(\boldsymbol x\mid \boldsymbol\mu, \boldsymbol\Sigma) = \left(2\pi\right)^{-\frac{p}{2}}\left|\boldsymbol\Sigma\right|^{-\frac{1}{2}}\exp\left\{-\frac{1}{2}\left(\boldsymbol x - \boldsymbol\mu\right)^T\boldsymbol\Sigma^{-1}\left(\boldsymbol x - \boldsymbol\mu\right)\right\}

Our posterior is proportional to the product of the likelihood and prior.

.. math::
  \begin{align}
  p(\mu \mid \boldsymbol X) &\propto \text{likelihood $\times$ prior} \\
  &\propto -\frac{1}{2}\left(\boldsymbol\mu^T\left(N\boldsymbol\Sigma^{-1} + \boldsymbol S_0^{-1}\right)\boldsymbol\mu - \boldsymbol\mu^T\left(N\boldsymbol\Sigma^{-1}\bar{\boldsymbol x} + \boldsymbol S_0^{-1}\boldsymbol m_0\right) - \left(N\boldsymbol\Sigma^{-1}\bar{\boldsymbol x} + \boldsymbol S_0^{-1}\boldsymbol m_0\right)^T\boldsymbol\mu\right)
  \end{align}   \\  

where, :math:`\bar{\boldsymbol x} = \frac{1}{N}\sum_{i=1}^N\boldsymbol x`. In order to interpret the above as the general form we need to substitute some terms. Let :math:`\boldsymbol A = N\boldsymbol\Sigma^{-1} + \boldsymbol S_0^{-1}` and let :math:`\boldsymbol b = N\boldsymbol\Sigma^{-1}\bar{\boldsymbol x} + \boldsymbol S_0^{-1}\boldsymbol m_0`.

.. math::
  p(\mu \mid \boldsymbol X) \propto -\frac{1}{2}\left(\boldsymbol\mu^T\boldsymbol A\boldsymbol\mu - \boldsymbol\mu^T\boldsymbol b - \boldsymbol b^T\boldsymbol\mu\right).

In order to complete the square we can add some helper term that is not dependent on :math:`\mu`:

.. math::
  p(\mu \mid \boldsymbol X) \propto
  -\frac{1}{2}\left(\boldsymbol\mu^T\boldsymbol A\boldsymbol\mu - \boldsymbol\mu^T\boldsymbol b - \boldsymbol b^T\boldsymbol\mu + \boldsymbol b^T\boldsymbol A^{-1}\boldsymbol b\right)

Remember that :math:`A` is symmetric – it's a weighted sum of symmetric matrices – and invertible – it's a sum of full-rank covariance matrices. Hence, the above is rewritten as

.. math::
  p(\mu \mid \boldsymbol X) \propto
  -\frac{1}{2}\left(\boldsymbol\mu^T\boldsymbol A\boldsymbol\mu - \boldsymbol\mu^T\boldsymbol A\boldsymbol A^{-1}\boldsymbol b - \boldsymbol b^T\boldsymbol A^{-1}\boldsymbol A\boldsymbol\mu + \boldsymbol b^T\boldsymbol A^{-1}\boldsymbol A\boldsymbol A^{-1}\boldsymbol b\right)

We introduce new helper terms in order to complete the square. Let :math:`\boldsymbol\Sigma_n = \boldsymbol A^{-1}` and :math:`\boldsymbol\mu_n = \boldsymbol A^{-1}\boldsymbol b`. The above is rewritten as

.. math::
  p(\mu \mid \boldsymbol X) \propto
  -\frac{1}{2}\left(\boldsymbol\mu^T\boldsymbol \Sigma_n^{-1}\boldsymbol\mu - \boldsymbol\mu^T\boldsymbol \Sigma_n^{-1}\boldsymbol \mu_n - \boldsymbol \mu_n^T\boldsymbol \Sigma_n^{-1}\boldsymbol\mu + \boldsymbol \mu_n^T\boldsymbol \Sigma_n^{-1}\boldsymbol \mu_n\right)

Organize the terms

.. math::
  p(\mu \mid \boldsymbol X) \propto
  -\frac{1}{2}\left(\boldsymbol\mu - \boldsymbol\mu_n\right)^T\boldsymbol\Sigma_n^{-1}\left(\boldsymbol\mu - \boldsymbol\mu_n\right)

So here is the posterior distribution

.. math::
  \boldsymbol\mu\mid \boldsymbol X \sim \mbox{Multivariate Normal}_p\left(\boldsymbol\mu_n,\boldsymbol\Sigma_n\right),

where

.. math::
  \begin{eqnarray}\boldsymbol\Sigma_n = \boldsymbol A^{-1}&=&\left(N\boldsymbol\Sigma^{-1} + \boldsymbol S_0^{-1}\right)^{-1},\\\boldsymbol\mu_n = \boldsymbol A^{-1}\boldsymbol b&=&\boldsymbol\Sigma_n\left(N\boldsymbol\Sigma^{-1}\bar{\boldsymbol x} + \boldsymbol S_0^{-1}\boldsymbol m_0\right)
  \end{eqnarray}



Part-b
######

Here's code:

.. code-block:: python

  m_0 = np.array([0,0]).T
  s_0 = np.array([0.1,0,0,0.1]).reshape(2,2)

  mu = np.array([0.0,0.0])
  sigma = np.array([1.0,0,0,1]).reshape(2,2)

  def mle():
    t = np.array([0.0,0])
    for _ in range(10):
        t += np.random.multivariate_normal(u, var)
    return t / 10

  def inv(m):
    return np.linalg.inv(m)

  N = 10
  x_mean = mle()
  sigma_n = inv( N* inv(sigma) + inv(s_0) )
  mu_n = sigma_n.dot( N* inv(sigma).dot(x_mean) + inv(s_0).dot(m_0) ) 
  print(x_mean)   # [-0.20467891  0.24346118]
  print(mu_n)     # [-0.10233945  0.12173059]

The Bayesian estimate is half of MLE hence, it is closer to the tru value.

-----------------------------------------------


Q3. Wishart distribution
========================

part a
######
* Mean: :math:`\nu W`
* Variance: :math:`Var(\Lambda_{ij}) = \nu ( V_{ij}^2 + V_{ii}V_{jj} )`



part b&c
########
As we are given a mean :math:`A` and we know that mean is equal to :math:`\nu W`, we only need to set on parameter, the degree-of-freedom. We run grid search for degree-of-freedom and sample_size.

Below is a grid search plot I ran for parameter tuning for sample_sizes = [1, 10, 1000] and degree-of-freedom = range(2,60,2). For this example I found the degree-of-freedom 52 and sample size 1000 is the most optimal. However, it's not straightforward which parameters are the best. For instance, degree-of-freedom 2 seems to be really bad for the sample size 1 but it is okay for the sample size 10. So the decision for parameters are left to a stakeholder.

.. figure:: /images/bayesian/wishart_grid_search.png
  :scale: 50%
  :align: center
  :alt: alternate text
  :figclass: align-center

  < The x-axis: sample sizes, y-axis: degree-of-freedom  >


CODE
^^^^

.. code-block:: python

  A = np.array([2,0.3,0.3,0.5]).reshape(2,2)

  sample_sizes = [1,10,1000]
  distances = []
  def run_wishart(df):
      for ss in sample_sizes:
          V = A/df
          samples = wishart.rvs(df, V , ss)
          if ss == 1:
              distances.append( np.linalg.norm(A - samples)) 
          else:
              distances.append( np.linalg.norm(A - samples.mean(axis=0))) 
      
      return samples.mean(axis=0)

  for df in range(2,60,2):
      l = run_wishart(df)

  data = np.array(distances).reshape(3, 29).T; data

  from matplotlib import cm
  from numpy.random import randn

  font = {'family' : 'normal',
          'weight' : 'bold',
          'size'   : 32}

  import matplotlib
  matplotlib.rc('font', **font)

  fig, ax = plt.subplots(figsize = (5, 55))


  # data = np.clip(randn(10, 10), -1, 1)

  cax = ax.imshow(data, interpolation='nearest', cmap=cm.coolwarm)
  ax.set_title('Gaussian noise with vertical colorbar')
  ax.set_xticks(range(3))
  ax.set_xticklabels(sample_sizes)
  ax.set_yticks(range(29))
  ax.set_yticklabels(list(range(2,60,2)))



  # Add colorbar, make sure to specify tick locations to match desired ticklabels
  cbar = fig.colorbar(cax, ticks=[0.02, 0.95, 1.9])
  cbar.ax.set_yticklabels(['< 0.02', '0.95', '> 1.9'])  # vertically oriented colorbar

  plt.show()

-------------------

