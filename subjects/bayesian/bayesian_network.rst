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

====================================== ============
Independent?                           D connection
====================================== ============
:math:`A \perp\!\!\!\perp B`           –
:math:`A \perp\!\!\!\perp B | C`       Separated
:math:`A \perp\!\!\!\perp B | E`       Separated
:math:`D \not\!\perp\!\!\!\perp E | C` Connected
====================================== ============


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


------------------------

Collider
========
A cllider (v-structure, head-to-head meeting) has two incoming arrows along a chosen path.

.. figure:: /images/bayesian/collider.png
   :align: center
   :alt: alternate text
   :figclass: align-center

   < Source: Aalto course CS-E4820: Advanced probabilistic methods >



-----------------------------------------------------------------------------------------

D-Connection & D-Separation
===========================

* **A path** between variables *A* and *B* **is blocked** by a set of variables :math:`\mathcal{C}`, if 

  * there is a collider in the path such that neither the collider nor any of its descendasnts is in the conditioning **set** :math:`\mathcal{C}`.
  * there is a non-collider in the path that is in the conditioning **set** :math:`\mathcal{C}`.

* Sets of variables :math:`\mathcal{A}` and :math:`\mathcal{B}` are **d-separated** by :math:`\mathcal{C}` if all paths between :math:`\mathcal{A}` and :math:`\mathcal{B}` are blocked by :math:`\mathcal{C}`.
  
  * d-separation implies :math:`A \perp\!\!\!\perp B | C` 

  * :math:`\mathcal{X}` and :math:`\mathcal{Y}` are d-separated by :math:`\mathcal{Z}` in :math:`G` iff they are not d-connected by :math:`\mathcal{Z}` in :math:`G`.


Bottom line
###########
* Non-collider in the conditioning set :math:`\Rightarrow` Blocked :math:`\Rightarrow` d-separated :math:`\Rightarrow` **conditionally independent** *BUT* **unconditionally dependent**
* Collider or its descendants in the conditioning set :math:`\Rightarrow` Not blocked :math:`\Rightarrow` d-connected :math:`\Rightarrow` **conditionally dependent** *BUT* **unconditionally independent**

Examples
########

.. figure:: /images/bayesian/d_separation_1.png
  :scale: 50 %
  :align: center
  :alt: alternate text
  :figclass: align-center

  < *b* d-separates *a* from *e*. *{b,d}* d-connect *a* from *e*. >

.. figure:: /images/bayesian/d_separation_2.png
  :scale: 50 %
  :align: center
  :alt: alternate text
  :figclass: align-center

  < *c* and *e* are (unconditionally) d-connected. *b* d-connects *a* and *e* >

.. figure:: /images/bayesian/d_separation_3.png
  :scale: 50 %
  :align: center
  :alt: alternate text
  :figclass: align-center

  < *t* and *f* are d-connected by *g* >

.. figure:: /images/bayesian/d_separation_4.png
  :scale: 50 %
  :align: center
  :alt: alternate text
  :figclass: align-center

  < *b* and *f* are d-separated by *u* >



Markov equivalence
==================

Two graphs are **Markov equivalent** if they
  
  * entail(need) the same conditional independencies
  * equivalently have the same d-separations

.. figure:: /images/bayesian/markov_equivalent.png
   :align: center
   :alt: alternate text
   :figclass: align-center

   < A markov equivalent example >


Graph
==============

.. figure:: /images/bayesian/dag.png
  :scale: 50 %
  :align: center
  :alt: alternate text
  :figclass: align-center

* Parent: pa(D) = {A,C}
* Children: ch(D) = E
* Family: A node itself and its parents. 
  
  * fa(E) = {B,D,E,F}

* Markov blanket: A node itself, its parents, children and the parents of its children. 
  
  * MB(B) = {A,B,C,D,E,F}

-----------------------------------------------------------------------------------------
