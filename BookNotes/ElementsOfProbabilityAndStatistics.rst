##############################################################
Elements of Probability and Statistics by Biagini & Campanino
##############################################################

Reference: 
Biagini, Francesca, and Massimo Campanino, Elements of Probability and Statistics: An Introduction to Probability with de Finetti’s Approach and to Bayesian Statistics (Switzerland, 2016)

Random Numbers
===============

Introduction
--------------

Numbers can be used to represent values. However the catch is that these
values are not necessarily known. For example, you toss a coin, you can
represent the result as number but you don't know the result yet, so its
content is not known yet.

However even if you don't know the exact value yet, you know *possible values*
that can be the result, that is you know the *set of possible values* for the
given element.

These are called random numbers and they are most of the time represented with
a capital letter. For example let the sides of a dice is A. We would write
:math:`I(A) = {1,2,3,4,5,6}`:

- :math:`I()` denotes the set of possible values

- :math:`I(A)` is *upper bounded* if :math:`I(A) < + \infty`
- :math:`I(A)` is *lower bounded* if :math:`I(A) > - \infty`
- :math:`I(A)` is *bounded* if :math:`I(A) > - \infty, I(A) < + \infty`

Two random numbers A and B are logically independent if

- :math:`I(A,B) = I(A) \times I(B)`: the product being a cartesian product.

Invalid Example:

- I have a pack of balls, containing 4 balls, with 4 different colors. I 
  pick a ball and pick another ball without putting back the first ball. 

Valid Example:

- I have a pack of balls, containing 4 balls, with 4 different colors. If I
  pick a ball and I put the ball back and then I pick another ball.

Problem with Invalid Example:

By not putting the ball back I effect the color of the second ball, since we 
know that the second ball can not be of the color of the first one.

Formally:

.. math::

    I(E1, E2) = {(i, j) | 1 \le i \le 4, 1 \le j \le 4, i \ne j}
    (i, j) \not\in I(E1, E2)
    \therefore
    I(E1, E2) \ne I(E1) \times I(E2)

Following operations are possible for random numbers:

1. :math:`A \lor B = max(A, B)`
2. :math:`A \land B = min(A, B)`
2. :math:`A \land B = AB`
3. :math:`\~{B} = 1 - B`

First two operations are distributive, associative and commutative. That is
they are:

- Distributive:
  :math:`A \lor (B \land C) = (A \lor B) \land (A \lor C)`
  :math:`A \land (B \lor C) = (A \land B) \lor (A \land C)`

- Associative:
  :math:`A \lor (B \lor C) = (A \lor B) \lor C`
  :math:`A \land (B \land C) = (A \land B) \land C`

- Commutative:
  :math:`A \lor B = B \lor A`
  :math:`B \land A = A \land B`

Plus they have the following properties:

- :math:`\sim \sim B = B`
- :math:`\~{(A \land B)} = \~{A} \lor \~{B}`
- :math:`\~{(A \lor B)} = \~{A} \land \~{B}`

Events
-------

Events are particular random numbers. They can have two values, thus they are
boolean in nature. :math:`I(E) = {0, 1}`. They have either happened, or they
have not happened.

- operation :math:`\land` is called *logical product*.
- Operation :math:`\lor` is called *logical sum*.

In the case of events:

- :math:`E1 \lor E2 = E1 + E2 - (E1 \land E2)`
- :math:`E1 \land E2 = E1 \times E2`

Notice that since events are random *numbers*, there is nothing weird about
adding or subtracting them. The fact that we don't know their value, does not
change the fact that they are addable or subtractable.

Complementary of an Event E is :math:`\~{E} = 1 - E`

Following are also true for events due to the fact that they are random
numbers:

.. math::
    
    (E1 \lor E2)^{\sim} = \sim E1 \land \sim E2
                        = (1 - E1) \times (1 - E2)
                        = 1 - E1 - E2 + (E1 \times E2)
                        = 1 - 1 ( E1 + E2 (E1 \times E2) )
                        = 1 - ( E1 \lor E2 )

As you can see this is coherent with the complementary rule that we have
provided above

Difference of events is the following:

- :math:`E1 \\ E2 = E1 - (E1 \times E2)`

Symmetric difference of events:

- :math:`E1 \triangle E2 = (E1 \\ E2) \lor (E2 \\ E1) = E1 + E2 \mod 2`

If the value of the random number, event, is equal to 1, we say that the event
happens, when it is equal to 0, we say that it does not happen.

The logical sum of two events happen (is true) if at least one of the event
happens.

The logical product of two events happen (is true) if both events happen
 
The complementary event of an event E happens, only if E does not happen.

The subset relation :math:`E1 \subset E2` implies that if E1 happens, so does
E2.

The operator :math:`\vdash` marks the truth value of the proposition as true.
It is thus a logical operator.

Following relations apply to events:

- *incompatiblity*: if :math:`E1 \subset E2 = 0`, meaning that if one event
  happens the other can not happen. Thus :math:`\vdash E1 \land E2 = 0`

- *exhaustivity*: if :math:`E_1, + E_2, + ..., + E_n \ge 1`.

- *partition* :math:`E_1 +, E_2, +..., E_n = 1` are said to be a partition if
  they are exhaustive and two by two incompatible).

page 6 given n events constituent
