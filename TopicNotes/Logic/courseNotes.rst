#######################
`Logic`_
#######################

Expert systems:

- Supply


Future of planning algorithms ?

- Learning from examples
- Transfer learning
  - We learn in one area and apply it to another.
- Interactive planning:
  - Human - machine teams solves problems together:
    * What is the best interface ?


Understand the following algorithms:

Resolution algorithm:

It is an elegant way of infering new knowledge from a knowledge base

Graphplan

Value iteration for markov decision processing.

Propositional logic
---------------------

Propositional symbols such as
B, E, A, M, J

corresponding to:

B
    Burglary Occuring

E
    Earth Quake Occuring

A
    Alarm Occuring

M
    Mary Calling

J
    John Calling

These can be either True or False, or unknown

We can make logical sentences by combining them with logical operators. Ex:

We can say that:

- Alarm is True when:
  * Burglary occuring true
  * Earthquake occuring true

Equivalent in propositional logic would be :math:`E {\lor} B {\Rightarrow} A`

We can say that when the alarm occurs, Jonh and Mary would call.
:math:`A {\Rightarrow} (J {\land} M)`

We can say John calls if and only if Mary calls.
:math:`J {\Leftrightarrow} M`

We can say that John is not Mary.
:math:`J {\sim} M`

A propositional logic is either true or false with respect to a model of the world.

A model is just a set of true or false values, for all the propositional symbols.
Ex.
model = { B:True, A:False, J:True, ... }

We can define the truth of a sentence in terms of the truth of the symbols with respect to the models using truth tables.

Truth tables
----------------

Truth tables list all the possibilities for the propositional symbols:

| p     | q     | not p | p ^ q   | p v q  | p --> q | p <--> q |

|-------|-------|-------|---------|--------|---------|----------|

| false | false | true  | false   | false  | true    | true     |

| false | true  | true  | false   | true   | true    | false    |

| true  | false | false | false   | true   | false   | false    |

| true  | true  | false | true    | true   | true    | true     |


**Valid sentence** is **true in every possible model** for every possible combination of values of the propositional symbols.
**Satisfaiable sentence** is one that is true in some model.
**Unsatisfaiable sentence** is false in every possible model.

Limitations of propositional logic
-----------------------------------

- It can handle only truth or false values:
  - No capability of handling uncertainty as in probability theory.
- No possibility to model object with properties that are continous like, size, colour etc.
- No shortcuts, that is to give a statement about a space you need to invoke every element of that space:
  - To say that all the rooms in the house are clean, you would need to invoke all the rooms in the house and relate them to each other with respect to their current state.


First Order Logic
=====================

We shall compare three approaches that model our world, namely:

- Propositional Logic
- First order Logic
- Probability theory

We shall compare their ontological commitment with respect to our world. We would also compare the believes the agents might have within these approaches, that is their epistemological commitment.

Ontological commitment of the first order logic is that there are:

- Relations between things in the world
- Objects
- Functions on those objects

in the world.

Epistemological commitment of the first order logic is that agents can believe:

- True
- False
- Unknown

states of those that are in the world.

Ontological commitment of the propositional logic is that there are:

- Facts

in the world.

Epistemological commitment of the propositional logic is that agents can believe:

- True
- False
- Unknown

  states of those that are in the world.

Ontological commitment of the probability theory is that there are:

- Facts

in the world.

Epistemological commitment of the probability theory is that agents can believe:

- :math:`B = \{ 0,{\dots}, 1 \}`

states of those that are in the world.

Problem solving used **atomic representation** of the world, that is each state is indivisible, on which you can only check if it is the same or not with another state, or if it is the goal state or not.

In propositional logic, and probability theory, we use **factored representation** of the world that is, we divide the world into a set of facts that are true or false, this is called a factored representation.

The most complex type of representation is called **structured representation**, in which the individual state is not just a set of values for variables, but it can include relationships between objects, a branching structure, etc. What we see in traditional programming languages, in a database, this comes with first order logic.


First Order Logic
====================

We have a set of:

- objects
- constants that can refer to objects
- function mapping from object to objects.
- relations

Syntax in first order logic
-----------------------------

We have sentences that describe facts that are true or false.

Atomic sentences are the predicates corresponding to relations:

- ABOVE(A,B)
- Vowel(A,B)
- A = B

We can also combine operators from propositional logic in the sentences of First order logic.

There are also quantifiers:

- :math:`{\forall}x` means for all x.
- :math:`{\exists}y` there exists a y.



