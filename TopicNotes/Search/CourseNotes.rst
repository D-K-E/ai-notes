###################
Search
###################

AI is about what to do when you don't know what to do.
Regular programming is when you know what to do and make the machine do it by writting instructions.

=========================
Definition of a Problem
=========================

What is a problem:

- Initial State: :math:`S_0`
- Actions(S): a function that takes the state as the input an returns a set of possible actions that the agent can execute.
  * returns: :math:`\{a_1, a_2, a_3, {\dots} \}`
- Result: another function, which takes an action and a state as input, and gives another state as the output.
  * So :math:`R(s,a_i) {\to} {\overline{s}}`, 
- GoalTest: a boolean function, which says if the we are at the goal state or not.
- PathCost: another function which takes a sequence of state as an argument to calculate a cost value for the given sequence.
  * :math:`PC(s_i^a_{j} + s_{i+1}^{a_{j+1}}, {\dots}) {\to} n, n {\in} {\mathbb{R}}, i {\in} \{0,1,{\dots}\}, j {\in} \{1,2,{\dots}\}`
  * Most of the time, cost of a path is the sum of the cost of the individual steps, which are calculated with StepCost function.
- StepCost: another function takes the state, action, and the resulting state of the action as an input and calculates the step cost of that action as the output.
  * :math:`SC(s,a_i,{\overline{s}}) {\to} n`
- These last two are evaluation functions, the StepCost is the quick one, and PathCost is the heavy one.

---------------
States
---------------

At every state, we would want to separate out to three parts

- Frontier
- Explored
- Unexplored

Two type of graphs intersect with each other:

- State space graph
- Search state graph

State space graph is usually a given, search state graph is calculated.
During the calculation process:

- the states that are already calculated are explored states,
- the states that are being calculated are frontiers,
- the states that are not yet calculated are unexplored states.

This results in the following general search tree function.

.. code-block:: python

                def TreeSearch(problem):
                    frontier = {['initialState']}
                    # Frontier, is the path consisting only the initial state
                    while some condition:
                    if len(frontier) == 0:
                        # This means there can be no solution.
                        return False
                    path = choice(frontier)
                    # We make a choice among the paths that are calculated
                    # the chosen path is also removed from the frontier
                    # Choice function is a family of functions not a single
                    # algorithm
                    #
                    state = f(path)
                    # We find the state at the end of the path.
                    # f(): takes path as input returns its end.
                    if s == Goal:
                        return path
                        # Meaning if the state is the goal,
                        # then we found our path.
                    #
                    else:
                        # We are not at the goal state,
                        # Thus we should extend the path.
                        for a in actions(s):
                            frontier.add([path + a + Result(s,a)])
                            # We add the path, the action and the result of the action
                            # that is the new state resulting from the action.

Now let's see our options for the "choice" function:

- Breadth first search, or Shortest way Search
  * It chooses the shortest path that has not been considered.
    + The measure for the shortest path is the number of paths between the end state and the origin state.
    + For example, between s1 and s5 there are 2 paths, between s3 and s5 there are 102 paths, and between s2 and s5 there are 4 paths, breadth first search would evaluate, s1, then s2, then s3, before meeting a condition like, all the branches associated with the frontier state are in explored states list, etc.


.. code-block:: python

   def GraphSearch(problem):
       frontier = {['initial']} # Contains the Initial State with Its path.
       explored = {} # Contains the explored states with their path.
       if len(frontier) == 0:
          return False
       chosen_path = frontier.pop()# This should add this path to explored as well
       path = choice(chosen_path)
       state = end(path)
       explored.add(state)
       if state == goal:
           return path
       for action in ACTIONS(state):
           if Results(a, state) not in frontier or Results(a, state) not in explored:
                frontier.add([path,a,Results(a,state)])

.. code-block:: python

   # Breadth first search



- Uniform Cost search, or Cheapest First Search
  * Find the path with the **cheapest total cost**.
  * How it works:
    + We start in the start state
    + We pop the empthy path from frontier to explored
    + Add in the path out that state.
  * It is not a directed search, it is just an expansion, we should expect to cover half of the search space on average.

--------------------------
Use cases for Searches
--------------------------

In the case of a large binary tree:

Breadth first search and cheap first search:

- Has 2n space saving necessity

Depth first search:

- Has n space saving necessity.

Breadth first and cheap first always find their goal, because they evaluate the states at a horizontal level, whereas
Depth first search would go down the infinite path, because it evaluates the tree in vertical direction.

------------------
A* search
------------------

Better name would be Best Estimated Total Path Cost Search

To get faster results than the uniform cost search, we need to add more knowledge.
A good knowledge would be the estimate of distance from the start state to the goal.

A* search works as follows:

- :math:`min(f = g + h)`:
  * Minimum value for the function "f"
  * Function "f" is defined as the sum of the results of the function "g" and "h"
  * Function "g" is path cost, it takes the path as the argument.
  * Function "h" is equal to final state of the path, which is equal to the estimated distance to the goal
  * The value of the Function "h" should always be lower than the true cost of the state.

When A* ends, it returns a path, p with estimated cost, C.
And it turns out that the C is also the actual cost, because at the goal state, the estimated distance to the goal would be 0, thus C, that was suppose to represent the estimated cost, would represent the actual cost, since C = h(s) + g(p) and h(s) = 0.
All the other states in the frontier should have a bigger estimated cost than the C, because frontier is explored in cheapest first order.
The path p, then must have cost that is less than the true cost of any other path in the frontier.

======================================
`How to derive heuristics functions`_
======================================

Heuristics can be derived from formal description of the problems.
For example:

Description
    Block can move from A to B if A is adjenct to B, and B is blank

Derived Heuristic 1
    Block can move from A to B if A is adjenct to B. Heuristic: B - A == 1

Derived Heuristic 2
    Block can move from A to B. Heuristic: Misplaced Blocks.

This is called generating a relaxed problem.

============================================
`When Searches Work`_
============================================

1. Problem domain must be fully observable.
2. Problem domain must be Known: we have to know the set of available actions to us.
3. Problem domain must be discrete: we have to have a finite number of actions to choose from.
4. Problem domain must be deterministic: we have to know the result of taking an action.
5. Domain must be static, that is there must be nothing else that changes the domain besides our own actions.

====================================
`How to implement the algorithms`_
====================================

In the implementation we talk about nodes. A node has 4 fields:

* State at the End of the Path
* Action it took to get to the State at the End of the Path
* Total cost
* Parent, is the pointer to another node

This linking of nodes one to another creates a sequence, a "path".
The word path is used as an abstract idea, a node is the representation in the computer memory.
Nodes are implemented as sequence structures, they can be thus list.
Frontier, and explored list.

Frontier
----------

- Operations
  * Remove best item, add a new one.
  * Membership test
- Implementation
  * Priority queue
- Representation
  * Set
- Build
  * Hash Tables
  * Tree

Explored
------------

- Operations
  * Add new members
  * Check for membership
- Representation
  * Single set
- Build
  * Hash Tables
  * Tree
