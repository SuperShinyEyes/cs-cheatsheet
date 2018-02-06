=========
Practices
=========

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
**
In total there are :math:`2^5 = 32` combinations of parameters i.e., states. The last probability could be defined by :math:`1 - sum(31 states)`. Thus the distribution could be defined by 31 combinations of parameters.

b.
**
Let's have alphabets instead of :math:`x_i`.

.. math::
  p(a,b,c,d,e) = p(a)p(b|a)p(c|b)p(d|c)p(e|d)

The distribution needs 2+2+2+2+2 - 1 = 9 parameters. -1 is for the fact that you can deduce from the assumption the distribution sums to 1 and you can get the last probability by subtracting the rest from 1.

c.
**
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

