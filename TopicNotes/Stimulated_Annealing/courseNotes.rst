#########################
`Stimulated Annealing`_
#########################

Main problem that examples this problem is n queens problem

You have n number of queens that you are trying to arrange so that they don't attack each other.

=============
Local Minima/Maxima
=============

In a graph of (state space,objective function) corresponding to (x,y) of the coordinate plane, a **local minimum** or **local maximum** can be described as the following scenario:

With a given heuristic, you reach a point during the search where any move seems like a back step from where you are.
That is the local minimum.
For example you are in board state where only 1 pair of queens are attacking to each other.
But you can not find a single move where the situation doesn't get worse.
Whereas you have to find a state where there are no queens attacking each other.
You've then reach a local minimum/maximum, depending on the way you formulate the quesiton.

====================================
`Solution to local Minima/Maxima`_
====================================

You have to add randomness to the agent.

If you are stuck at a local minimum/maximum, you have to start again, and see if you can come up with a better value,

This doesn't guarantee that you would absolutely get the global maximum, and it is inefficient, however there are improvements based on caching, and graph topology, so it is something we can work on.

Problems
----------

Step size is an issue when traversing the graphs. If it is too small, we can get stuck in flat areas, if it is too large we can skip the global minimum, maximum.
With big step size there is also a risk of going into an infinite loop, around the global maximum.

=====================
Simulated Annealing
=====================

In order to understand annealing, we need to understand the concept of energy minimisation.
When external conditions allow molecules to be mobile, and then the mobility of the molecule slowly reduces, the molecules then arrange themselves into the lowest energy consumption configuration, often these result in regular patterns, like in honeycombs, they try to optimise the storage space and minimze the building material for the structure they are building.
Another application is in sword making, you heat the edge of the blade, atoms start moving, and they form new structures with carbon and other atoms, then they cool the iron to preserve the lattice structures that the blade had created.
This idea of heating and cooling would help us get out of local minima and find the global minimum:

- High temperature is more randomness.
- Gradual cooling will decrease the randomness for converging on a solution.

.. code-block:: python

                t = 1

                while t:
                    T = schedule(t) # T is the simulated temperature at time t, which reduces from a high value at the beginning to near zero eventually.
                    if T == 0:
                        return current
                    next = random(current) # Randomly selected successor of current.
                    #
                    if ΔE > 0:
                        current = next
                    else:
                        current = next_with_probability_of_e_over_ΔE/T


Basically we iterate we simulated annealing.
We look for points close to our current position that might have an improved value.
However, we are going to select our next position randomly from the points in from the region near us, if the new position is better than our current position then we are going to take it. If it is not better we are still going to take it with probability of :math:`e^{\frac{{\delta}E}{T}}`

This method is **guarenteed** to converge on global maximum.

=====================
`Local Beam Search`_
=====================

The local beam search, instead of just using one position which we would call a particle, will keep track of k particles.
At each time frame, we would randomly generated neighbors of these particles, and k_best ones for the next iteration. If any of these particles, reach a goal, we terminate.


=======================
`Genetic Algorithms`_
=======================

For n-queens problem, we have 28 possibilities of total attacks. The heuristic function would be, 28 - number of attacks available to queens on board, which should be reduced to 0.

It uses breeding and mutation to find the optimal answer to a problem.
Suppose that 4 boards represents our gene pool:

- We trying to breed better boards by combining them in different patterns.
- We evaluate each board according to the heuristic function we just described. 
- From these scores we create proportional probabilities, for how likely each one is to breed.
  - Meaning we add the scores to each other and divide each one with the total number to have a percentage.
- We would select parents from four boards according to these percentages.
- Let's say, the percentages are:
  * 31
  * 29
  * 26
  * 14
- Selection process occurs the following way:
  * We roll a hundered sided die.
  * If the number that shows is between 1-31 then first board is the parent, if it is between 32-60, we select the second and so on.
    + That is 1-31 represents the first parent because its percentage is 31, 32-60 represents second parent because its percentage is 29, and since the first 31 numbers belong to first parent the 29 numbers that come afterwards belong to second parent.
  * From each set of parents we shall produce 2 children, why? No particular reason, just a convention.
  * We select a point along each pair of parent, this usually a position in parents, we pick this point randomly, and then create the child by taking the first part of the first parent which is anterior to selected position, and the second part from the second parent which is posterior to selected position, then we combine the two. Ex. First parent = 32152, selected position is the third position, second parent = 41254, then the first child is 32154, second child = 41252.

<<<<<<< HEAD

=======
=========================
`Notes from AIMA`_
=========================

General take on these two algorithms is that they are **local**, that is all that matters is the goal state, as opposed to the cost of the path that reaches to this goal state.

They are applied to environments that are not fully observable and deterministic.
Meaning that, the agent doesn't know what precept it would receive as the result of its action. Thus it has to adjust to the contingencies that can come up with the precepts.
Partial observability implies also that the agent needs to keep track of the states it had been to.

Local search algorithms and optimisation
--------------------------------------------

These are applied to problems in which the path to the goal state doesn't really matter, what usually matters is the goal state.

For example, a factory floor layout is not a problem in which you would need the order of the actions that you might want to execute. What you would want is to have a result state that satisfies the given constraints.

They have 2 main advantages:

- They use very little memory, usually a constant amount.
- They can find reasonable solutions in large or infinite (continous) state spaces.

Local search algorithms explore **state space landscape** which has two elements:

- Location: This is the current state of the agent.
- Elevation: This is a value defined by the value of the heuristic cost function or objective function

One dimensional version is mostly represented as a hill made of the values of the heuristic cost function, and the location representing the current state of the agent.

It has two main qualities:

- Complete: Finds the goal state if it exists
- Optimal: finds the global minimum or maximum.

Hill climbing algorithms mostly are incomplete and sub optimal

That is why we need stimulated annealing.

Stimulated annealing
------------------------

This algorithm is introduces randomness to hill climbing.
The idea is to gradually decrease the randomness during the selection of the next state.
If the next state does a better job, it is kept, at the cost of decreasing the randomness.
This algorithm is complete.
It can also handle continous environments.

.. code-block::

   function SIMULATED-ANNEALING(problem,schedule) returns a solution state
 inputs: problem, a problem
    schedule, a mapping from time to "temperature"

 current ← MAKE-NODE(problem.INITIAL-STATE)
 for t = 1 to ∞ do
   T ← schedule(t)
   if T = 0 then return current
   next ← a randomly selected successor of current
   ΔE ← next.VALUE - current.VALUE
   if ΔE > 0 then current ← next
   else current ← next only with probability e^{ΔE/T}


Genetic Algorithms
--------------------

Genetic algorithms are a variant of stochastic beam search.

We start with a k number of states called population.
Each state is called an individual.
Each state is evaulated by an objective function, called fitness function.

Pairs are created in order to reproduce new children.
Their coupling is done according to the value of the fitness function

For each pair we select a random cross over point for reproduction:
the offspring themselves are created by crossing over the parent strings at the
crossover point. For example, the first child of the first pair gets the first three digits from the
first parent and the remaining digits from the second parent, whereas the second child gets
the first three digits from the second parent and the rest from the first parent.

.. code-block::

 function GENETIC-ALGORITHM(population,FITNESS-FN) returns an individual
 inputs: population, a set of individuals
    FITNESS-FN, a function that measures the fitness of an individual

 repeat
   new_population ← empty set
   for i = 1 to SIZE(population) do
     x ← RANDOM-SELECTION(population,FITNESS-FN)
     y ← RANDOM-SELECTION(population,FITNESS-FN)
     child ← REPRODUCE(x,y)
     if (small random probability) then child ← MUTATE(child)
     add child to new_population
   population ← new_population
 until some individual is fit enough, or enough time has elapsed
 return the best individual in population, according to FITNESS-FN

function REPRODUCE(x, y) returns an individual
 inputs: x,y, parent individuals

 n ← LENGTH(x); c ← random number from 1 to n
 return APPEND(SUBSTRING(x, 1, c),SUBSTRING(y, c+1, n))



Searching with Non deterministic Actions
------------------------------------------

This is the case when we can't forsee the result of our actions.
Thus we **can't plan ahead**.
This means we would have to adjust our actions based on the result we observed after execution.

This implies that our solution is not a sequence of particular actions, it is a contingency plan, a strategy.
Thus it has the structure of a tree rather than a sequence, since they contain for the most part if else then statements.

The non deterministic or partially observable (or both) environments can be represented as and-or tree graphs.


and-or tree search graph
--------------------------

In a deterministic environment, branching is based on the agent's choices. These are called **or-nodes**.
in a non-deterministic environment, the environment gets to choose the result of an action as well. These are called **and-nodes**.

These properities create a different search tree, in which the solution is a subtree with several qualities:

- There is a goal node at every leaf
- There is an action in every or-node
- There is an outcome branch in every and-node.

This subtree is called a plan.

.. code-block::

   function AND -OR -G RAPH -S EARCH (problem) returns a conditional plan, or failure
       OR -SEARCH (problem.I NITIAL -S TATE , problem, [ ])

   function OR -SEARCH (state, problem, path) returns a conditional plan, or failure
       if problem.GOAL -TEST (state) then return the empty plan
       if state is on path then return failure
       for each action in problem.ACTIONS (state) do
           plan ← AND -SEARCH (RESULTS (state, action), problem, [state | path])
           if plan ~= failure then return [action | plan]
       return failure

   function AND-SEARCH (states, problem, path) returns a conditional plan, or failure
       for each s i in states do
           plan i ← OR -SEARCH (s i , problem, path)
           if plan i = failure then return failure

       return [if s 1 then plan 1 else if s 2 then plan 2 else . . . if s n−1 then plan n−1 else plan n ]

One key aspect of the algorithm is the way in which it deals with cycles, which often arise in nondeterministic problems (e.g., if an action sometimes has no effect or if an unintended effect can be corrected).
If the current state is identical to a state on the path from the root, then it returns with failure.
This doesn’t mean that there is no solution from the current state; it simply means that if there is a noncyclic solution, it must be reachable from the earlier incarnation of the current state, so the new incarnation can be discarded.
With this check, we ensure that the algorithm terminates in every finite state space, because every path must reach
a goal, a dead end, or a repeated state. Notice that the algorithm does not check whether the current state is a repetition of a state on some other path from the root, which is important for efficiency.
>>>>>>> newcurrent
