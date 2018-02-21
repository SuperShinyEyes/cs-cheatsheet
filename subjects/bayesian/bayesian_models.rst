===============
Bayesian Models
===============

Linear Parameter Models
=======================

Determining hyperparameters: ML-II
##################################

The hyperparameter posterior distribution is 

.. math::
  p(\Gamma|\mathcal{D}) \propto p(\Gamma|\mathcal{D}) p(\Gamma) 

If :math:`p(\Gamma) \approx const` this is equivalent to 

.. math::
  \Gamma^* = \arg\max_{\Gamma} p(\mathcal{D}|\Gamma),

where the **marginal likelihood**

.. math::
  p(\mathcal{D}|\Gamma) = \int p(\mathcal{D}|\Gamma, \boldsymbol w) p(\boldsymbol w|\Gamma)d\boldsymbol w  


ML vs. ML-II
############
In ML(Maximum likelihood), we select parameter values **w** that maximize the log-likelihood

.. math::
  \begin{align}
    \log p(y|\textbf{w}.\textbf{x}) &= \sum_{i=1}^{N} \log N(y_i|\textbf{w}^T \psi(\textbf{x}_i), \beta^{-1} ) \\
    \hat{\textbf{w}} &= \arg\max_{\textbf{w}} { \log p(y| \textbf{w},\textbf{x} ) } \quad \text{(does not depend on $\beta$)}
  \end{align}

In ML-II, we select hyperparameter values :math:`\alpha` and :math:`\beta` that maximize the
(log-)marginal likelihood (parameters **w** integrated out)