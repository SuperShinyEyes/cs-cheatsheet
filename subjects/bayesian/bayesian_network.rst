================
Bayesian Network
================

.. note::

  Here I use :math:`\perp\!\!\!\perp` as an independence symbol.


A Bayesian network(BN) is a directed acyclic graph (DAG) in which nodes represent random variables, whose joint distribution is as follows,

.. math::
  p(x_1, ..., x_D) = \prod_{i=1}^D p\big(x_i| pa(x_i)\big)

where :math:`pa(x_i)` represents the parents of :math:`x_i`.

.. figure:: /images/bayesian/bayesian_network.png
   :align: center
   :alt: alternate text
   :figclass: align-center

   < An example of a bayesian network. Source: Aalto course CS-E4820: Advanced probabilistic methods >

BNs are used in ML, because they are

* a concise way to represent and communicate the structure and assumptions of a model.
* a compact representation of the joint distribution. => efficient!


Independence in Bayesian networks
=================================

Example 1
#########

.. figure:: /images/bayesian/five_nodes.png
   :align: center
   :alt: alternate text
   :figclass: align-center

================================ ======
Statement                        Answer
================================ ======
:math:`A \perp\!\!\!\perp B`     True
:math:`A \perp\!\!\!\perp B | E` True
:math:`D \perp\!\!\!\perp E | C` False
================================ ======


Example 2
#########

.. figure:: /images/bayesian/three_nodes.png
   :align: center
   :alt: alternate text
   :figclass: align-center

   < Conditional Independence >


.. figure:: /images/bayesian/three_nodes2.png
   :align: center
   :alt: alternate text
   :figclass: align-center

   < Marginal Independence >

Here's my real life example about marginal independence. Say you won a $1M lottery(C). You can either buy a $1M house(A) or buy a $1M Ferrari(B). If you've bought a Ferrari, you haven't bought a house for sure.