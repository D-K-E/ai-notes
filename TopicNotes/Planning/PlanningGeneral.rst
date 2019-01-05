######################
`Planning`_
######################

Planning and Execution
=========================

Planning and execution should be intervened in real world applications of AI.

Why ? Because some feedback is needed from the states during the execution phase.

Why ? Because the states change during the real world, so they don't stay the same as they were during the planning phase.

Ex., let's say the car should turn to left at the intersection, during the execution of this process, the car needs to stop as per indicated by traffic lights, so that it follows the rules of traffic.

This means that the interweaving of the planning and execution is a necessity that comes from the environment.

If the envrionment is:

- Stochastic: we don't know for sure what an action is going to do.
- Multiagent: there are other cars in the road, so we have to plan the distance between cars.
- Partial Observability: we don't know some of the states before execution process starts, you don't know if the traffic in Boğaz Köprüsü is heavy unless you are there, if there is heavy traffic, you can leave the car, and take the ferry instead. 
- Unknown areas.
- Hiearachical

Most of the problems can be changes by shifting our point of view, that is instead of planning in world states, we plan on belief states.


Plans in Partially observable Environments
--------------------------------------------


Sensorless planning in a deterministic world
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

In partially observable environments, in which we don't know where we are in the given state space, we act and depending on the result, we deduce something about the state space. Ex.

We are at the point A, but we don't know it. We go left. Bump into a wall, we then know that we are at the leftmost area of the state space. Let's call that area B. From B we start going right. We bump into a wall, we then know that we are at the rightmost area of the state space. Let's call it C. Thus we know 3 states, point A, B, C. We can also say that B is at the left of A, and C is at the right of B.

Partially observable planning in a deterministic world
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

We have local sensing, that is we can see:

- What location it is in
- What is going on in the current location
- But it can't see whether there is any dirt in any other locations.

Act-percept cycle, we act and observe its consequences, depending on the consequence we can split the world into two.


Stochastic Environments
-------------------------

Let's say we have an agent, that has some defect, we don't know the consequence of our action, meaning we have multiple outcomes.

So in a stochastic partially observable environment, actions increase uncertainty and thus the state space, the observations would decrease the uncertainty and thus the state space.

Any plan that would work in deterministic world might workout okay, in the stochastic world, but no finite plan is guaranteed to work.

In order to achieve the goal state we need to have access to infinite number of actions available to us in our notation.

This is introduced by the notion of condition, and tree.

We execute a series of actions then we observe and if we are at a desired state we continue if not we loop back to a previous state.

If the stochasticity is independent, that is if sometimes agent's action works sometimes it doesn't, then with probability of 1 in the limit, this plan will in fact achieve the goal.

mathematically we can express them as the following.

:math:`A,S,F Result(Result(A, A -> s), S -> F)`

We apply the action going A to S to the state A, then to the resulting state, we apply the action of going S to F to the previous resulting state which the state S, then we check if we are at the goal state.

New stochastic partially observable worlds, the equations are a little more complex.


Normally what we have is a state like s1, a resulting state which is the result of the Result function. Result function maps the action object to the state object. Something like:

- s1 = Result(state, action)

In stochastic partially observable environments, there is a b1, which is a belief state. b1, is a result of the Update function.
The Update function takes two arguments, a resulting state and an observation.
The resulting state, which is in the argument, is the result of the Predicate function.
The Predicate function takes two arguments, a state, and an action.
Basically what happens is, we take a belief state apply an action on it, then update it with respect to the observation.

Classical Planning
====================

Representation language for dealing with states, actions and plans. It is an approach for dealing with the problem of complexity by factoring the world into variables.

Classical planning assumes that a state space is a k-Boolean meaning there are :math:`2^{k}` states in that space.

For example, in the vacuum world,
the state space would be:

- Dirt is at the location A
- Dirt is at the location B
- Vacuum is at the location A

With these three variables we can have 8 possible states. And all the states can be represented with these three varaibles.

World state would be a complete assignment of boolean values to each of the variables.

Belief state can be a complete assignment, a partial assignment, and an arbitrary formula.

These are all the states we can have.

There is also an action schema, that means, a list of possible actions.
An action would look like the following in a scheme:

- Action(Fly(p,x,y)) which would read as the action, flying in which p flies from point x to y.
  * Precond: the conditions that are necessary in order to *execute*
    + Plane(p) ^ Airport(x) ^ Airport(y) ^ At(p,x),
      - Plane(p): that is p should be a plane, as opposed to a truck or submarine.
      - Airport(x): that is x should be an airport, as opposed to a highway.
      - Airport(y): that is y should be an airport as well.
      - At(p,x): that is p should be at airport x in order to take off from there, as opposed to some other airport.

  * Effects: the result of the actions commited by the agent.
    + ~At(p,x) ^ At(p,y)
      - Meaning that the effect of the action would be a state in which x is not at p but in y


Progression search
--------------------

Start from the initial state and advance as in the problem solving mechanism
Initial state, execute action, check for the goal state, branch out, execute another action etc.



Regression search
------------------

We start from the goal state and go backwards.
Regression search makes sense when the branching factor is high at the beginning.
Ex: Buying a book

- Action: buy(b)
  * Precond: ISBN(b)
  * Effect: Own(b)
- Goal: Own(0136042597)

If we start from an initial state, that is we take the books with isbn numbers.
We would match them against the goal state's isbn number in order to execute the buy action.
That would mean we would have to match against 10 million numbers before finding the right one.

If we approach the question from backwards.
If we say, what action would result in owning the book, 0136042597, we can respond I should buy the book, 0136042597.
The problem then becomes a look up problem rather than a matching problem.


Plan State Search
-------------------

We know the start state and end state. We try to come up plans that fills the between.

Ex. We have a sliding puzzle of 8x8.

The action schema of this puzzle would be:

- Action(Slide(t, a, b)): slide the tile t from a to b.
  * Precondition: On(t,a) ^ Tile(t) ^ Blank(b) ^ Adj(a,b)
    + On(t,a): tile t is on location a.
    + Tile(t): tile t is an instance of Tile object
    + Blank(b): b is a blank location
    + Adj(a,b): the location a is adjacent to the location b.

  * Effect: On(t,b) ^ Blank(a) ^ ~On(t,a) ^ ~Blank(b)
    + On(t,b): tile t is on location b.
    + Blank(a): location a is blank.
    + ~On(t,a): tile t is not on location a.
    + ~Blank(b): location b is not blank

An advantage of writing this formal representation is that with problem relaxing a program can come up with good heuristics as well by for example canceling out some conditions on precondition, or canceling out the negative effects in the effects section of the schema.

Situation Calculus
--------------------

Goal: move all cargo from airport A to airport B.
Basically we are using First order logic to represent a planning.

- Actions: are objects meaning that they are functions in first order logic. like Fly(p,x,y)

- Situations: are objects that correspond to the past of the actions, if you arrive to the states by using two different set of actions those would be considered as two different situations.
  * Most of the time we have an initial situation often called :math:`s_0`, we call a function on situation, called the result so that the result of the situation object and an action object is another situation:
    + :math:`s_1 = Result(s_0, a)`, "a" being the action.

- Actions that are possible in a given state is done by using a predicate.
  * Possible(a, s): Predicate Possible of "a", a possible action in state "s"
    + These possible actions are most of the time are described with some sort of precondition on s, like Precondition(s) -> Possible(a,s)

- The example predicate for the possible of action in state "s", also called the possibility axiom, is something like the following:
  * Plane(p,s) ^ Airport(x,s) ^ Airport(y,s) ^ At(p,x,s) -> Possibility(Fly(p,x,y),s)
    + Plane(p,s): Plane "p" is at state "s".
    + Airport(x,s): Airport "x" is at state "s".
    + Airport(y,s): Airport "y" is at state "s".
    + At(p,x,s): Plane "p" is at the airport x in the state "s".
    + Possibility(Fly(p,x,y),s): There is a possiblity of the action Fly which joins together the plane "p", the airport "x", and the airport "y" at state "s".

Now there is another notation for predicates like At, which can vary from one situation to another.
These predicates are called *fluents*, to express that they are in a constant state of change.

Fluents refer to a specific situation and we put that situation as the last argument.

The trickiest part of situation calculus is to describe what changes and what doesn't change as a result of an action.

We write an axiom for what changes, that is we write an axiom for *fluents*.

These axioms are called **Successor-state axioms**. These are used to described what happens in the state, that's the successor of executing an action.

They generally have the form saying, forall actions and states if it's possible to execute action "a" in state "s", then the fluent is true if and only if action made it true, or action didn't undo it. Ex:

At(p,x,s)
    forall action, state: Possible(action, state) -> fluent:true <--> action make it true v action didn't undo

In(p,x,s)
    forall action, state, cargo, plane, x: Possible(action, state) -> In(cargo, plane, Result(state, action) <--> action = Load(cargo, plane, x) or (In(cargo, plane, situation) and action != Unload(c,p,x))
    * This reads as the following: forall the action, state, cargo, airport x
      + If
      + the action is possible for the state,
      + then
      + In predicate which takes the cargo, plane and the situation which is the result of the state and the possible action as argument would hold True,
      + If and only If
      + the action is Load() action involving the cargo, plane, and the airport x
      + or
      + the action is not Unload() action involving the cargo, plane and the airport x

We can make an initial state by coming up with some assertions on various types of predicates involving the domain.

Initial state: S_0
    At(Plane_1, SabihaGökçen, S_0)

Goal state: Thereexists a state S_n in which forall cargo exist Charles de Gaul in state S_n

The advantage of the Situation calculus is as follows:

Once we describe the problem in plain first order logic:

- We don't need any special programs, since we already have theorem provers for first order logic, we can just state this as a problem, and apply a normal theorem prover that we already had for other uses and it can come up with an answer in the form of a path that satisfied this goal

- That is a situation, which corresponds to a path given the initial state and given the description of the actions.

Dealing with planning: Summary
----------------------------------

- Deterministic, fully observable environments.
- Stochastic and Partially observable environments.

Weakness

- We can't distinguish probable and improbable solutions.



Search Problem as a Graph
----------------------------

Think of a search problem as a graph where the nodes are states and the edges are
actions. The problem is to find a path connecting the initial state to a goal state. There are
two ways we can relax this problem to make it easier: by adding more edges to the graph,
making it strictly easier to find a path, or by grouping multiple nodes together, forming an
abstraction of the state space that has fewer states, and thus is easier to search.


Heuristics for planning:
-------------------------------

For 
