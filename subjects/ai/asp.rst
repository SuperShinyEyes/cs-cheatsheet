
===========================
Answer Set Programming(ASP)
===========================
ASP is a declarative programming method emphasizing on **what** to be computed rather than **how** to compute.

.. figure:: /images/ai/declarative1.svg
   :align: center
   :alt: alternate text
   :figclass: align-center

   < Formulas as rules and satisfying assignments as answer sets. Source: Aalto AI [1]_ >



Theory
======

Answer sets
###########
1. The set is closed under the rules of the program.
2. If some particular atom belongs to the set, then there is at least one supporting rule instance having the atom in question as its head and the body of the rule is satisfied by the set. In other words, atoms cannot be true without a reason.
3. The set is minimal in this sense. In other words, atoms are false by default.


Positive programs
#################
A positive rule is is an expression of the form

.. math::
  a \leftarrow b_1, \cdots ,b_n

where the head atom :math:`a` can be inferred if the body atoms :math:`b_1, \cdots ,b_n` have been inferred by other rules in the program. A rule with an empty body (:math:`n=0`) is called a fact.

.. topic:: Definition
  The (unique) answer set of a positive program is the least set :math:`S` of ground atoms which is closed under the ground instances of its rules:

  1. If there is a ground instance :math:`h(t)\leftarrow b_1(t_1) \cdots b_n(t_n)` of some rule in the program such that :math:`b_1(t_1)\in S, \ldots, b_n(t_n)\in S`, then also :math:`h(t) \in S`
  2. If some other set :math:`S'\subseteq S` is closed in this way, then :math:`S' = S`



ASP tutorial
============

Graph coloring [1]_
###################
.. code-block::

	# Define three colors
	col(r). col(g). col(b). 

	# Each node has a unique color
	# The '== 1' is cardinality of the set.
	{ color(X,C) : col(C) } == 1 :- node(X).

	# The above is equal to the "fact" below:
	# { color(3,r), color(3,g), color(3,b) } == 1.



	# Two connected nodes must not have the same color
	:- edge(X,Y), color(X,C), color(Y,C).

	#show color/2. 

n-Queen problem [2]_
####################
Basic

.. code-block::
	
	#const n = 8.

	{ queen(1..n,1..n) }.

	% There must be n queens only
	 :- not {queen(I,J)} == n.

	 :- queen(I,J),queen(I,JJ), J != JJ.
	 :- queen(I,J),queen(II,J), I != II.
	 :- queen(I,J),queen(II,JJ), (I,J) != (II,JJ), I-J == II-JJ. 
	 :- queen(I,J),queen(II,JJ), (I,J) != (II,JJ), I+J == II+JJ. 
	 
	#show queen/.
	
Advanced

.. code-block::

	#const n = 8.

	% Now the generator is a combination of few testers above.
	% There is only one queen in a row
	{ queen(I,1..n) } = 1 :- I = 1..n.

	{ queen(1..n,J) } = 1 :- J = 1..n.

	% There must be n queens only
	:- { queen(I,J) : D = I+J-1 } >= 2, D=1,..2*n-1.
	:- { queen(I,J) : D = I-J+n } >= 2, D=1,..2*n-1.

	#show queen/2

The above code is much faster than the basic one but it will still take much time on solving due to ``:- { queen(I,J) : D = I+J-1 } >= 2, D=1,..2*n-1.``. It is an exhaustive calculation. Let's be more efficient.

.. code-block::

	#const n = 8.

	% Now the generator is a combination of few testers above.
	% There is only one queen in a row
	{ queen(I,1..n) } = 1 :- I = 1..n.

	{ queen(1..n,J) } = 1 :- J = 1..n.

	% There must be n queens only
	:- { queen(I,J) : diag1(I,J,D) } >= 2, D=1,..2*n-1.
	:- { queen(I,J) : diag2(I,J,D) } >= 2, D=1,..2*n-1.

	diag1(I,J,I+J-1) :- I = 1..n, J = 1..n.
	diag2(I,J,I-J+n) :- I = 1..n, J = 1..n.
	
	#show queen/2

---------------------------------------------------------

.. rubric:: References
.. [1] https://www.youtube.com/watch?v=kdcd7Je2glc
.. [2] https://www.youtube.com/watch?v=d3arlJlGRTk&feature=youtu.be