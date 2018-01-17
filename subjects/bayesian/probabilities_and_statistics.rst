============================
Probabilities and Statistics
============================

Variance and Standard Deviation
###############################
Spread of probability mass about the mean.

Variance
********

.. math::
  Var(X) = E[(X- E[X])^2] = E[X^2] - E[X]^2

Computation as sum:

.. math::
  Var(X) = \sum_{i=1}^n p(x_i) (x_i 0 \mu)^2

Standard Deviation
******************
.. math::
  \sigma = \sqrt{Var(X)}

--------------------

Likelihood
##########
A function of the parameters of a statistical model given data.

Joint distribution
##################

A probability distribution for two or more variables. OR  A joint distribution is a distribution which is joint.

Conditional distribution
########################

For random variables A and B, :math:`P(A|B)` is the probability distribution which describes the change in A when B is changed.

Conditional independence
************************
:math:`X \perp\!\!\!\perp Y | Z` denotes that variable X and Y are conditionally independent of each other, given the state of variable Z.

.. math::
  P_{X,Y |Z}(x,y|z) = P_{X|Z}(x|z)P_{Y|Z}(y|z)

Intuitively, this means that if we know the value of Z, knowing in addition the value of Y does not provide any information about the value of X. Indeed, provided :math:`P(y,z) > 0`, we have

.. math::
  X \perp\!\!\!\perp Y | Z \Longrightarrow P_{X,Y |Z}(x,y|z) = P_{X|Z}(x|z)

Proof:

.. math::
  \begin{align}
  p(x|y,z) &= \frac{p(x,y,z)}{ p(y,z) } = \frac{p(x,y|z)p(z)}{p(z)p(y|z)} \\
          &= \frac{p(x|z)p(y|z)p(z)}{p(z)p(y|z)} = p(x|z)\\
  p(x,y,z) &= p(xjy,z)p(yjz)p(z) \quad \text{(General chain rule of probability)}
  \end{align}

Intuitive examples
^^^^^^^^^^^^^^^^^^
1. Let :math:`X_1,X_2,...,X_n` denote the cumulative sum of *n* dice throws, such that :math:`dom(X_1) = {1,...,6}, dom(X_2) = {2,...,12}`, etc.

  * Is :math:`X_{n+1}` independent of :math:`X_{n-1}`? **NO**.
  * Is :math:`X_{n+1}` conditionally independent of :math:`X_{n-1}` given :math:`X_{n}`? **YES**.

2. X ='Location of an airplane now', Y ='Location of the plane 15s ago', Z='Location 15s from now'

  * Is Y independent of Z? **NO**
  * Is Y conditionally independent of Z given X? **NO**

Marginal distribution
#####################

Consider a joint probability distribution :math:`P(\theta 1, \theta 2)`.  A marginal distribution is obtained by integrating over one parameter,

.. math::
 P(\theta_1) = \int P(\theta_1, \theta_2)d \theta_2

It gives the probabilities of the variables without reference to the other variables. The contrary of conditional distribution.

Marginal independence
*********************
Random variable X is marginally independent of random variable Y if, for all :math:`x_i \in dom(X), y_j \in dom(Y), y_k \in dom(Y),`,

.. math::
  P(X=x_i|Y=y_j) = P(X=x_i|Y=y_k) = P(X=x_i) \\
  \text{(NOTE: the differences in *j* and *k*)}

That is, knowledge of Y’s value doesn’t affect your belief in the value of X.

Sample space
############

* Set of all possible outcomes of an experiment
* Size of the set is **NOT** the sample space
* Outcomes can be sequence of numbers

Discrete sample space
*********************

.. math::
  \text{Discrete = listable} \\

e.g.

.. math::
  \begin{align}
  {a, b, c}       & \quad \text{(finite)} \\
  {0, 1, 2, ... } & \quad \text{(infinite)}
  \end{align}

------------------------

Independence
############

* Events A & B are independent if :math:`P(A \cap B) = P(A) \times P(B)`
* Random variables X and Y are independent if :math:`F(x, y) = F_X(x) F_Y(y)`
* Discrete random variables X and Y are independent if :math:`P(x_i, y_j) = P_X(x_i) P_Y(y_j)`
* Continuous random variables X and Y are independent if :math:`f(x, y) = f_X(x) f_Y(y)`
* :math:`cov(X, Y) = 0 \iff E[XY] = E[X]E[Y]`

------------------------


Covariance and Correlation
##########################

  The two are very similar. Both describe the degree to which two random variables or sets of random variables tend to deviate from their expected values in similar ways.
  `- Wikipedia <Covariance and Correlation_>`_

.. _Covariance and Correlation: https://en.wikipedia.org/wiki/Covariance_and_correlation

Covariance
**********

Measures the degree to which two random variables vary together, e.g. height and weight of people.

Random variables :math:`X, Y` with means :math:`\mu_x, \mu_y`.

.. math::
  \sigma_{X,Y} cov(X, Y) = E((X - \mu_x)(Y-\mu_y))

Properties
^^^^^^^^^^
* :math:`cov(aX + b, cY + d) = ac cov(X,Y)` for constants :math:`a,b,c,d`
* :math:`cov(X_1 + X_2, Y) = cov(X_1,Y)+cov(X_2,Y)`
* :math:`cov(X,X) = Var(X)`
* :math:`cov(X,Y) = E(XY) - \mu_x \mu_y`
* If :math:`X, Y` are independent then :math:`Cov(X, Y) = 0`. **Warning**: The converse is not true, when covariance is 0 the variables might not be independent.

Correlation
***********

It's like covariance, but it removes the scale. The population correlation coefficient :math:`\rho_{X,Y}` between X and Y is defined by

.. math::
  \rho_{X,Y} = corr(X, Y) = \frac{cov(X,Y)}{\sigma_X \sigma_Y} = \frac{E[(X - \mu_X)(Y - \mu_Y)]}{\sigma_X \sigma_Y}

**WARNING**: It's not causation.

-------------------

Standardization
###############

.. math::
  Y = \frac{X-\mu}{\sigma}

* :math:`Y` has mean 0 and :math:`\sigma_Y = 1`
* Standardizing any normal random variable produces the standard normal.
* If :math:`X \approx normal`, then standardized :math:`X \approx` standardized normal
* :math:`Z`: standardized normal random variable.

----------------

Random Variables
################

Random Variable(RV)
*******************

.. math::
  X: \Omega \longrightarrow \mathbb{R}

Probability Mass Function(PMF)
******************************

.. math::
  f_X(x) = P[X = x] = P[{\omega \in \Omega: X(\omega) = x}]

Probability Density Function(PDF)
*********************************

.. math::
  P[a \leq X \leq b] = \int_a^b f(x) dx

Cumulative Distribution Function(CDF)
*************************************

.. math::
  F_X: \mathbb{R} \longrightarrow [0, 1] \quad F_X(x) = P[X \leq x]
