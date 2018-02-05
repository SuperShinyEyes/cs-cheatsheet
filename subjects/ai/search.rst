======
Search
======

.. note::
  This part of docs is mostly brought from Amit Patel's blog(https://www.redblobgames.com/pathfinding/a-star/).

Uninformed algorithms
=====================
DFS, BFS

Breadth First Search
####################

The following code demonstrates how to BFS work.

.. code-block:: python

  # Author: Amit Patel
  # https://www.redblobgames.com/pathfinding/a-star/introduction.html#breadth-first-search
  frontier = Queue()
  frontier.put(start)
  visited = {}
  visited[start] = True

  while not frontier.empty():
      current = frontier.get()

      # Following is for Early exit
      if current == goal:
        break    # End of the algorithm

      for next in graph.neighbors(current):
          if next not in visited:
              frontier.put(next)
              visited[next] = True


While search algorithms are mainly used to find shortest paths, it can be used to create procedural map generation.

.. code-block:: python

  # Author: Amit Patel
  # https://www.redblobgames.com/pathfinding/a-star/introduction.html#breadth-first-search

  frontier = Queue()
  frontier.put(start)
  came_from = {}
  came_from[start] = None

  while not frontier.empty():
    current = frontier.get()
    for next in graph.neighbors(current):
      if next not in came_from:
        frontier.put(next)
        came_from[next] = current

.. figure:: /images/ai/procedural_map_generation.png
 :align: center
 :alt: alternate text
 :figclass: align-center

 < Procedural map generation by BFS - `Amit Patel <https://en.wikipedia.org/wiki/Consistent_heuristic>`_ >

It's easy to get a path from a specific point back to the start.

.. code-block:: python

  # Author: Amit Patel
  # https://www.redblobgames.com/pathfinding/a-star/introduction.html#breadth-first-search
  current = goal
  path = []
  while current != start:
    path.append(current)
    current = came_from[current]
  path.append(start) # optional
  path.reverse() # optional

-------------------------------------------------------------------------------


Informed algorithms
===================

Dijkstra's algorithm
####################
AKA **Uniform Cost Search**. It prioritizes which path to explore. Instead of exploring all possible paths, it favors lower cost paths. We can assign lower costs to encourage moving on roads, higher costs to avoid forests, higher costs to discourage going near enemies, and more. When movement costs vary, we use this instead of Breadth First Search.

.. code-block:: python

  # Author: Amit Patel
  # https://www.redblobgames.com/pathfinding/a-star/introduction.html#dijkstra
  frontier = PriorityQueue()
  frontier.put(start, 0)
  came_from = {}
  cost_so_far = {}
  came_from[start] = None
  cost_so_far[start] = 0

  while not frontier.empty():
     current = frontier.get()

     if current == goal:
        break

     for next in graph.neighbors(current):
        new_cost = cost_so_far[current] + graph.cost(current, next)
        # YES, you may add a same frontier multiple times. That's
        # why we have costs and priorities here.
        if next not in cost_so_far or new_cost < cost_so_far[next]:
           cost_so_far[next] = new_cost
           priority = new_cost
           frontier.put(next, priority)
           came_from[next] = current


Heuristic Search
################
BFS and Dijkstra's algorithm expand in all directions but many times we have a direction. **Heuristic function** tells us how close we are to the goal. Here's a simple example.

.. code-block:: python

  # Author: Amit Patel
  # https://www.redblobgames.com/pathfinding/a-star/introduction.html#greedy-best-first
  def heuristic(a, b):
    # Manhattan distance on a square grid
    return abs(a.x - b.x) + abs(a.y - b.y)



Greedy Best First Search
^^^^^^^^^^^^^^^^^^^^^^^^
The queues of frontiers are now ordered by their priorities which tells you to which direction you should go.

.. code-block:: python

  # Author: Amit Patel
  # https://www.redblobgames.com/pathfinding/a-star/introduction.html#greedy-best-first
  frontier = PriorityQueue()
  frontier.put(start, 0)
  came_from = {}
  came_from[start] = None

  while not frontier.empty():
    current = frontier.get()

    if current == goal:
      break

    for next in graph.neighbors(current):
      if next not in came_from:
        priority = heuristic(goal, next)
        frontier.put(next, priority)
        came_from[next] = current

..


:math:`A^{*}` algorithm
^^^^^^^^^^^^^^^^^^^^^^^
:math:`A^{*}` is a modification of Dijkstra’s Algorithm that is optimized for a single destination. So you may think it as a mixture of Dijkstra and Greedy Best First Search.

.. code-block:: python

  # Author: Amit Patel
  # https://www.redblobgames.com/pathfinding/a-star/introduction.html#astar
  frontier = PriorityQueue()
  frontier.put(start, 0)
  came_from = {}
  cost_so_far = {}
  came_from[start] = None
  cost_so_far[start] = 0

  while not frontier.empty():
     current = frontier.get()

     if current == goal:
        break

     for next in graph.neighbors(current):
        new_cost = cost_so_far[current] + graph.cost(current, next)
        if next not in cost_so_far or new_cost < cost_so_far[next]:
           cost_so_far[next] = new_cost
           priority = new_cost + heuristic(goal, next)
           frontier.put(next, priority)
           came_from[next] = current

Choosing a Heuristic Function for :math:`A^{*}`
***********************************************

  From `A*’s Use of the Heuristic <http://theory.stanford.edu/~amitp/GameProgramming/Heuristics.html#a-stars-use-of-the-heuristic>`_

  * At one extreme, if :math:`h(n)` is 0, then only :math:`g(n)` plays a role, and :math:`A^*` turns into Dijkstra’s Algorithm, which is guaranteed to find a shortest path.
  * If :math:`h(n)` is always lower than (or equal to) the cost of moving from n to the goal, then :math:`A^*` is guaranteed to find a shortest path. The lower :math:`h(n)` is, the more node :math:`A^*` expands, making it slower.
  * If :math:`h(n)` is exactly equal to the cost of moving from n to the goal, then :math:`A^*` will only follow the best path and never expand anything else, making it very fast. Although you can’t make this happen in all cases, you can make it exact in some special cases. It’s nice to know that given perfect information, :math:`A^*` will behave perfectly.
  * If :math:`h(n)` is sometimes greater than the cost of moving from n to the goal, then :math:`A^*` is not guaranteed to find a shortest path, but it can run faster.
  * At the other extreme, if :math:`h(n)` is very high relative to :math:`g(n)`, then only :math:`h(n)` plays a role, and :math:`A^*` turns into Greedy Best-First-Search.


-------------------------------------------------------------------------------

Markov Decision Processes
=========================
An MDP is defined by:
  * A set of states :math:`s \in S`
  * A set of actions :math:`a \in A`
  * A transition_function/model/dynamics :math:`T(s,a,s')`

    * prob that :math:`a` from :math:`s` leads to :math:`s'`, i.e., :math:`P(s'|a,s)`

  * A reward(cost) function :math:`R(s,a,s')`

    * aka :math:`R(s')` or :math:`R(s)`
    * We want to maximize the reward/cost.

  * A start state
  * Maybe a terminal state

* MDPs are non-deterministic/stochastic search problems

What is Markov about MDPs?
##########################
"Markov" means that given the present state, the **future and the past are independent**. For MDP, "Markov" means **action outcomes depend only on the current state**.

.. math::
  P(S_{t+1} = s'| S_{t} = s_t, A_{t} = a_t, \cdots , S_{0} = s_0) \\
  = P(S_{t+1} = s'| S_{t} = s_t, A_{t} = a_t)

In deterministic search problems, you want an optimal **plan**. In MDP, you want an optimal **policy(choice of action fo each state)** :math:`\pi*: S \Rightarrow A`. A policy :math:`\pi` gives an action for each state and hopefully maximizes expected utility(sum of discounted rewards). An explicit policy defines a reflex agent.

.. figure:: /images/ai/optimal_policies.png
  :align: center
  :alt: alternate text
  :figclass: align-center

  < Optimal policies. The optimal policy is R(s) = -0.4. When the cost is very bad the agent will behave in a depressing manner. `Berkeley AI <https://youtu.be/wKx4MuLfe0M>`_ >

Discounting
###########
Rewards' value gets discounted over time. Discounting happens because 

* sooner rewards probably have higher utility than later rewards
* helps our algorithm converge.

Discounting Example: discount of 0.5
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ 
* :math:`U([1,2,3]) = 1*1 + 0.5*2 + 0.25*3`
* :math:`U([1,2,3]) < U([3,2,1])`


Recursive definition of value
#############################

.. math::
  \begin{align}
  Q^*(s,a) &= \sum_{s'}T(s,a,s') \big[ R(s,a,s') + \gamma V^*(s') \big] \\
  V^*(s) &= \max_a Q^*(s,a)\\
         &= \max_a \sum_{s'}T(s,a,s') \big[ R(s,a,s') + \gamma V^*(s') \big]
  \end{align}










--------------------


Glossaries
==========

Expansion
#########
The expansion of a node *n* means that each successor node *m* of *n* is checked: if *m* has not been visited, it is visited now and recorded for further (recursive) expansion.

Admissible heuristic
####################
  In computer science, specifically in algorithms related to pathfinding, a heuristic function is said to be admissible if it never overestimates the cost of reaching the goal, i.e. the cost it estimates to reach the goal is not higher than the lowest possible cost from the current point in the path. - `Wikipedia: Admissible heuristic <https://en.wikipedia.org/wiki/Admissible_heuristic>`_

Monotonic heuristic
###################

.. figure:: /images/ai/Heuristics_Comparison.png
  :align: center
  :alt: alternate text
  :figclass: align-center

  < Comparison of an admissible but inconsistent and a consistent heuristic evaluation function. - `Wikipedia: Consistent heuristic <https://en.wikipedia.org/wiki/Consistent_heuristic>`_ >



References
==========
.. [Introduction_to_A*] https://www.redblobgames.com/pathfinding/a-star/introduction.html
