======
Search
======

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


:math:`A^{*}` algorithm
#######################
:math:`A^{*}` is a modification of Dijkstra’s Algorithm that is optimized for a single destination. Dijkstra’s Algorithm can find paths to all locations; A* finds paths to one location. It prioritizes paths that seem to be leading closer to the goal.

-------------------------------------------------------------------------------


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
