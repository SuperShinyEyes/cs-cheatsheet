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

:math:``
.. math::


Policies
========

In deterministic search problems, you want an optimal **plan**. In MDP, you want an optimal **policy(choice of action fo each state)** 

.. math::
  \pi*: S \Rightarrow A, a = \pi(s)

A policy :math:`\pi` gives an action for each state and hopefully maximizes expected utility(sum of discounted rewards). An explicit policy defines a reflex agent.

.. figure:: /images/ai/optimal_policies.png
  :scale: 20%
  :align: center
  :alt: alternate text
  :figclass: align-center

  < Optimal policies. The optimal policy is R(s) = -0.4. When the cost is very bad the agent will behave in a depressing manner. `Berkeley AI <https://youtu.be/wKx4MuLfe0M>`_ >

We can compute optimal policies by solving the mathematical optimization problem

.. math::
  \max_{\pi}G_t \text{ such that } s_{t+1} = \mathcal{T}(s_t,\pi(s_t))

Value Functions
===============
A value-function

.. math::
  \begin{align}
  v_{\pi}: \mathcal{S} \Rightarrow \mathbb{R}, &&(s \in \mathcal{S} \text{ with a single number } v_{\pi}(s) \in \mathbb{R})
  \end{align}

is a foundation of many efficient methods for computing optimal policies. 

We define the value :math:`v_{\pi}(s_t)` of the state :math:`s_t`as the resulting return by following the policy :math:`\pi` starting at time :math:`t` from state :math:`s_t`,

.. math::
  v_{\pi}(s_t) = G_t = \sum_{j=0}^{\infty} \gamma^j R(s_{t+j}, \pi(s_{t+j})) \text{ with } s_{t+1} = \mathcal{T}(s_t, \pi(s_t))

Once we fix the starting state :math:`s_t = s`, the sequence of actions  :math:`a_t` and states :math:`s_t` in the above equation is completely determined by the policy :math:`\pi` and transition model :math:`\mathcal{T}` since 

.. math::
 s_{t+1} = \mathcal{T}(s_t, a_t), \text{ and } a_t = \pi(s_t)

A value function obeys the recursive relation:

.. math::
  \begin{align}
  v_{\pi}(s) = R(s, \pi(s)) + \gamma v_{\pi}(\mathcal{T}(s, \pi(s))), && \text{aka Bellman equation}
  \end{align}
  :label: bellman

This recursive property could be exploited in order to accurately estimate the value function from previous experience. 

Given a policy :math:`\pi`, we define its **action-value function** :math:`q_{\pi}(s,a)` as the return obtained if the agent starts from state :math:`s_t =s`, takes action :math:`a` and then acts according to the policy :math:`\pi`, i.e.,


.. math::
 q_{\pi}(s,a) = \underbrace{R(s_t,a_t)}_\text{take action $a_t$} + \underbrace{\sum_{j=1}^{\infty} \gamma^j R(s_{t+j}, a_{t+j})}_\text{then follows $\pi$} \text{ with } s_{t+1} = \mathcal{T}(s_t, a_t), \text{ and } a_t = \pi(s_t)

Using value functions we can compare the quality of different policies. In particular, we say that policy :math:`\pi'` is at least as good as, or **dominates**, policy :math:`\pi` if

.. math::
  v_{\pi'}(s) \geq v_{\pi}(s) \text{ for all states } s \in \mathcal{S}

Note, for a given MDP there may be **more than one optimal policy**. However,  all optimal policies for a particular MDP share the same optimal value function :math:`v_∗(s)`. Thus, if :math:`\pi_*' is an optimal policy, its value function is given as

.. math::
  v_{\pi_*}(s) = v_{*}(s) := \max_{\pi} v_{\pi}(s),
  :label: value-function

where the maximization is over all possible policies :math:`\pi : \mathcal{S} \Rightarrow \mathcal{A}`. Similarily, all optimal policies share the same optimal action-value function

.. math::
  q_{\pi_*}(s,a) = q_{*}(s,a) := \max_{\pi} q_{\pi}(s,a).

If we insert the recursive relation :eq:`bellman` satisfied by any value function into :eq:`value-function`, we obtain an analogous recursive relation for the optimal value function, i.e.,

.. math::
  v_{*}(s) = \max_{a \in \mathcal{A}} \Big[R(s, a) + \gamma v_{*}(\mathcal{T}(s, a)) \Big]

This relation is also known as the **Bellman optimality equation**.


Stochastic Policies
===================
A stochastic policy is specified by the conditional probability 

.. math::
  P(a_t = a | s_t = s)

Similarly, a stochastic model for state transition
and rewards is obtained by specifying the conditional probability

.. math::
  P(s_{t+1} = s', R_{t+1} = r | a_t = a,s_t = s) \in [0,1]

of the new state at time :math:`t` being :math:`s′` and the reward obtained being :math:`r`, when the current state is :math:`s_t = s` and the agent takes action :math:`a_t = a`. Note that deterministic policies and transition models are special cases of the stochastic models.


Discounting
===========
Rewards' value gets discounted over time. Discounting happens because 

* sooner rewards probably have higher utility than later rewards
* helps our algorithm converge.

Discounting Example: discount of 0.5
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ 
* :math:`U([1,2,3]) = 1*1 + 0.5*2 + 0.25*3`
* :math:`U([1,2,3]) < U([3,2,1])`

Rewards & Returns(sum of rewards)
=================================
As a consequence of taking the action :math:`a_t`, the agent receives a numerical **reward**

.. math::
  R_{t+1} = R(s_t,a_t)
  
In many applications the overall goal is not to maximize the immediate reward :math:`R_{t+1}` but to maximize a long-term(cumulative) reward, i.e., the **expected discounted return**:

.. math::
  G_t \doteq R_{t+1} + \gamma R_{t+2} + \gamma^2 R_{t+3} + \cdots = \sum_{k=0}^{\infty} \gamma^k R_{t+k+1}

where :math:`\gamma` is a parameter :math:`0 \leq \gamma \leq 1`, is called the discount rate.

Returns at successive time steps are related to each other:

.. math::
  \begin{align}
  G_t &\doteq R_{t+1} + \gamma R_{t+2} + \gamma^2 R_{t+3} + \cdots \\
      &= R_{t+1} + \gamma(R_{t+2} + \gamma R_{t+3} + \gamma^2 R_{t+4} + \cdots )\\
      &= R_{t+1} + \gamma G_{t+1}
  \end{align}

Although the return :math:`G_t` is a sum of an infinite number of terms, it is still finite if the reward is nonzero and constant, if :math:`\gamma  < 1`. For example, if the reward is a constant +1, then the return is

.. math::
  G_t = \sum_{k=0}^{\infty} \gamma^k = \frac{1}{1-\gamma}




Recursive definition of value
=============================

.. math::
  \begin{align}
  Q^*(s,a) &= \sum_{s'}T(s,a,s') \big[ R(s,a,s') + \gamma V^*(s') \big] \\
  V^*(s) &= \max_a Q^*(s,a)\\
         &= \max_a \sum_{s'}T(s,a,s') \big[ R(s,a,s') + \gamma V^*(s') \big]
  \end{align}


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