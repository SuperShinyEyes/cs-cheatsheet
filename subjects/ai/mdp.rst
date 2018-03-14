=======================
Markov Decision Process
=======================

An MDP is defined by:
  * :math:`s \in S` – a set of states 
  * :math:`a \in A` – a set of actions 
  * :math:`T(s,a,s')` – A transition_function/model/dynamics

    * prob that :math:`a` from :math:`s` leads to :math:`s'`, i.e., :math:`P(s'|a,s)`

  * :math:`R(s,a,s')` – a reward(cost) function 

    * aka :math:`R(s')` or :math:`R(s)`
    * We want to maximize the reward/cost.

  * :math:`\alpha` – a start state
  * :math:`\gamma` – a discount factor
  * Maybe a terminal state

* MDPs are non-deterministic/stochastic search problems

Markov Property
===============
The future is independent of the past given the present. [RL_course_mdp]_

A state :math:`S_t` is *Markov* if and only if

.. math::
  \mathbb { P } \left[ S _ { t + 1} | S _ { t } \right] = \mathbb { P } \left[ S _ { t + 1} | S _ { 1} ,\dots ,S _ { t } \right]

* The state :math:`S_t` captures **all relevant information from the history** i.e., the state :math:`S_{t+1}` is only dependent on :math:`S_t` not on states before that.
* Once the state is known, the history may be thrown away i.e., the state is a sufficient statistic of the future

State transition matrix
#######################
For a Markov state s and successor state s', the state transition probability is defined by 

.. math::
  \mathcal { P } _ { S S ^ { \prime } } = \mathbb { P } \left[ S _ { t + 1} = s ^ { \prime } | S _ { t } = s \right]

State transition matrix :math:`\mathcal { P }` defines transition probabilities from all states s to all successor states s'.

.. math::
  \mathcal { P } = \text{ from } \left[ \begin{array} { c c c } { \mathcal { P } _ { 11} } & { \dots } & { P _ { 1n } } \\ { \vdots } & { } & { } \\ { \mathcal { P } _ { 11} } & { \cdots } & { P _ { n n } } \end{array} \right]
  
where each row of the matrix sum to 1.

Markov Process
##############

A Markov process is a memoryless random process, i.e., a sequence of random states :math:`S_1,S_2,...` with the Markov property.

* A Markov process/Chain is a tuple :math:`<\mathcal{S}, \mathcal{P}>`
  
  * :math:`\mathcal{S}` is a finite set of states
  * :math:`\mathcal{P}` is a state transition probability matrix, :math:`\mathcal { P } _ { S S ^ { \prime } } = \mathbb { P } \left[ S _ { t + 1} = s ^ { \prime } | S _ { t } = 5\right]`


Markov Reward Process
=====================
A Markov reward process is a Markov chain with values, :math:`\langle \mathcal { S } ,\mathcal { P } ,\mathcal { R } ,\gamma \rangle`

* :math:`\mathcal{R}` is a reward function, :math:`\mathcal { R } _ { s } = \mathbb { E } \left[ R _ { t + 1} | S _ { t } = s \right]`
* :math:`\gamma` is a discount factor, :math:`\gamma \in [ 0,1]`

Return
######
The return :math:`G_t` is the total discounted reward from time-step t.

.. math::
  G _ { t } = R _ { t + 1} + \gamma R _ { t + 2} + \ldots = \sum _ { k = 0} ^ { \infty } \gamma _ { t + k + 1}


Value Function
##############
The value function :math:`v(s)` gives the long-term value of state :math:`s`.  It is the expected return starting from state :math:`s`

.. math::
  v ( s ) = E \left[ G _ { t } | S _ { t } = s \right]

.. math::

Bellman Equation in Matrix Form
###############################
The Bellman equation can be expressed concisely using matrices.

.. math::
  v = \mathcal { R } + \gamma \mathcal { P } \mathbf { v }

.. math::
  \left[ \begin{array} { l } { v ( 1) } \\ { \vdots } \\ { v ( n ) } \end{array} \right] = \left[ \begin{array} { l } { \mathcal { R } _ { 1} } \\ { \vdots } \\ { \mathcal { R } _ { n } } \end{array} \right] + \gamma \left[ \begin{array} { c c c } { \mathcal { P } _ { 11} } & { \dots } & { P _ { 1n } } \\ { P _ { 11} } & { \dots } & { P _ { n n } } \end{array} \right] \left[ \begin{array} { l } { v ( 1) } \\ { \vdots } \\ { v ( n ) } \end{array} \right]

You can solve the Bellman equation as a simple linear equation. The complexity is :math:`O \left( n ^ { 3} \right)` for n states. 

.. math::
  \begin{align}
  v &= \mathcal { R } + \gamma P _ { \nu } \\
  ( 1- \gamma P ) v &= \mathcal { R } \\
  v &= ( 1- \gamma P ) ^ { - 1} R
  \end{align}

As we are using inversion, the direct solution is possible only for small MRPs. Other iterative solutions are

* Dynamic programming
* Monte-Carlo evalution
* Temporal-Difference Learning

Markov Decision Process
=======================
MDP is a MRP with decisions. It is an environment in which all states are Markov.


Policies
########
A policy :math:`\pi` is a distribution over actions given states. It's a stochastic transition matrix.

.. math::
  \pi ( a | s ) = \mathbb { P } \left[ A _ { t } = a | S _ { t } = s \right]


* A policy defines the behaviour of an agent.
* It depends on the current state.
* Policy is stationary(time-independnet)

Value Functions
###############
The state-value function :math:`V _ { \pi } ( s )` of and MDP is the expected return starting from state s, and then following policy :math:`\pi`.

.. math::
  v _ { \pi } ( s ) = \mathbb { E } _ { \pi } \left[ G _ { t } | S _ { t } = s \right]

The action-value function :math:`q _ { \pi } ( s,a )` is the expected return starting from state s, taking action a, and then following policy :math:`\pi`.

.. math::
  q _ { \pi } ( s,a ) = \mathbb { E } _ { \pi } \left[ G _ { t } | S _ { t } = s ,A _ { t } = a \right]


Bellman expectation equation
############################
The state-value function can again be decomposed into immediate reward plus discounted value of successor state.

.. math::
  \begin{align}
  v _ { \pi } ( s ) &= \mathbb { E } _ { \pi } \left[ R _ { t + 1} + \gamma v _ { \pi } \left( S _ { t + 1} \right) | S _ { t } = s \right] \\
  &= \sum _ { a \in A } \pi ( a | s )   \big( \mathcal{ R }_{ s } ^ { a } +   \gamma \sum _ { s^{ \prime } \in S } \mathcal { P } _ { \text{ ss' } } ^ { 2} v _ { \pi } ( s' ) \big)
  \end{align}

The action-value function can similarly be decomposed.

.. math::
  \begin{align}
  q _ { \pi } ( s,a ) &= \mathbb { E } _ { \pi } \left[ R _ { t + 1} + \gamma q _ { \pi } \left( S _ { t + 1} ,A _ { t + 1} \right) | S _ { t } = s ,A _ { t } = a \right] \\
  &= \gamma \sum _ { s ^ { \prime } \in S } \mathcal { P } _ { S S ^ { \prime } } ^ { a } \sum _ { d ^ { \prime } \in A } \pi \left( a ^ { \prime } | s ^ { \prime } \right) q _ { \pi } \left( 5^ { \prime } ,a ^ { \prime } \right)
  \end{align}

Optimal Value Function
######################
The optimal state-value function :math:`V _ { * } ( s )` is the maximum state-value function over all policies

.. math::
  V _ { * } ( s ) = \max _ { \pi } v _ { \pi } ( s )

The optimal action-value function :math:`q_* ( s,a )` is the maximum action-value function over all policies

.. math::
  q_* ( s,a ) = \max _ { \pi } q _ { \pi } ( s,a )

An MDP is "solved" when we know the optimal value fn.

Define a partial ordering over policies

.. math::
  \pi \geq \pi ^ { \prime } \text{ if } v _ { \pi } ( s ) \geq v _ { \pi ^ { \prime } } ( s ) ,\forall _ { 5}

An optimal policy can be found by maximizing over :math:`q_* ( s,a )`,

.. math::
  \pi _ { * } ( a | s ) = \left\{ \begin{array} { l l } { 1} & { \text{ if } a = \operatorname{arg} \max q _ { x } ( 5,a ) } \\ { 0} & { \text{ otherwise } } \end{array} \right.

* There is always a deterministic optimal policy for any MDP.
* If we know :math:`q_* ( s,a )`, we immediately have the optimal policy.

Solving the Bellman Optimality Equation
#######################################

* Bellman optimality equation is non-linear
* No closed form solution in general
* Many iterative solution methods
  
  * Value iteration
  * Policy iteration
  * Q-learning
  * Sarsa








Q learning
==========
"Q" is for "quality" of a certain action in a given state.
We define a function :math:`Q \left( s ,a  \right)` representing the maximum discounted future reward when we perform action a in state s, and continue optimally from that point on.

.. math::
	Q \left( s _ { t } ,a _ { t } \right) = m a x R _ { t + 1}


Bellman equation represents the optimal Q-value of state *s* and action *a* in terms of the next state *s'* as following,

.. math::
	Q ^ { * } ( s ,a ) = r + \gamma m a x _ { a } Q ^ { * } \left( s ^ { \prime } ,a ^ { \prime } \right)

In words, it means the optimal future reward for this state and action is the immediate reward plu maximum future reward for the next state.


Explore-exploit dilemma
#######################
Should an agent exploit the known working strategy or explore possibly better unknown strategies?


.. rubric:: Reference

.. [RL_course_mdp] https://www.youtube.com/watch?v=lfHX2hHRMVQ


osascript -e 'set volume without output muted output volume 17 --100%'