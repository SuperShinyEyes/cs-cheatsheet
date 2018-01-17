============================
Probabilities and Statistics
============================

Joint distribution
##################

A probability distribution for two or more variables. OR  A joint distribution is a distribution which is joint.

Conditional distribution
########################

For random variables A and B, P(A|B) is the probability distribution which describes the change in A when B is changed.


Marginal distribution 
#####################

Consider a joint probability distribution :math:`P(\theta 1, \theta 2)`.  A marginal distribution is obtained by integrating over one parameter,

.. math::
 P(\theta 1) = \int P(\theta 1, \theta 2)d \theta 2 
 
It gives the probabilities of the variables without reference to the other variables. The contrary of conditional distribution.


Sample space
############

* Set of all possible outcomes of an experiment
* Size of the set is **NOT** the sample space
* Outcomes can be sequence of numbers

Discrete sample space
---------------------

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

------------------------

Covariance
##########
Measures the degree to which two random variables vary together, e.g. height and weight of people.

Random variables :math:`X, Y` with means :math:`\mu_x, \mu_y`.

.. math::
  Cov(X, Y) = E((X - \mu_x)(Y-\mu_y))

Properties
----------
* :math:`Cov(aX + b, cY + d) = ac Cov(X,Y)` for constants :math:`a,b,c,d`
* :math:`Cov(X_1 + X_2, Y) = Cov(X_1,Y)+Cov(X_2,Y)`
* :math:`Cov(X,X) = Var(X)`
* :math:`Cov(X,Y) = E(XY) - \mu_x \mu_y`
* If :math:`X, Y` are independent then :math:`Cov(X, Y) = 0`. **Warning**: The converse is not true, when covariance is 0 the variables might not be independent.

------------------------