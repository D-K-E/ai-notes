########################
Bayes Nets
########################

Bayes Network Computation:

We have an internal variable A which we can not sense.
We have another variable called B, which we can sense and is assumed to related
to A.

Basically a bayes network is composed of at least two variables:

- Observed Variable
- Unobserved Variable.

We know the prior probability of A and the conditional A causes B, that is
we know the probability of B given A and B given not A.

We use diagnostic reasoning: P(A|B), P(A|~B)

We need at least 3 parameter to instantiate a simple Bayes Network:

- P(A)
- P(B|A)
- P(B|~A)

A Different Way of Computing Bayes Rule:
-------------------------------------------

Bayes Rule:

- :math:`P(A|B) = {\frac{P(B|A)·P(A)}{P(B)}}`

P(B) is independent of the variable A, so it is a little hard to compute.
Suppose we are also interested in the complementery/disjunctive event:

- :math:`P(~A|B) = {\frac{P(B|~A)·P(~A)}{P(B)}}`

Dividors are the same and since the events are disjunctive, that is

- :math:`P(A|B) + P(~A|B) = 1`

Thus we have the probability :math:`P'(A|B) = P(B|A) · P(A)` and the probability
:math:`P'(~A|B) = P(B|~A) · P(~A)`.
From these two probabilities we can obtain the original two probabilities by
multiplying the expressions with a constant called k, it is the normaliser.

- :math:`k·P'(A|B) = P(A|B)`, :math:`k·P'(~A|B) = P(~A|B)`.

If we add these expressions on both sides:

- :math:`P(A|B) + P(~A|B) = k · (P'(A|B) + P'(~A|B))`
- :math:`1 = k·(P(B|A) · P(A) + P(B|~A) · P(A))`
- :math:`k = (P(B|A) · P(A) + P(B|~A) · P(A))^{-1}`  

An example application:

Hidden Cause Different Measurement Networks
---------------------------------------------

A hidden cause causes two different measurements.

P(C) = 0.01
P(~C) = 0.99

P(+|C) = 0.9
P(-|C) = 0.1

P(-|~C) = 0.8
P(+|~C) = 0.2

P(C|-) = P(-|C) P(C) / P(-)
P(~C|-) = P(-|~C) P(C) / P(-)
P(C|-) = 0.2·0.9 / P(-)
P(~C|-) = 0.8 · 0.9 / P(-)
P(-) = 0.18 + 0.72
P(-) = 0.9

P(C|+,+) = ?

C: 0.001 -> +: 0.9 -> 0.9 multiplying all for obtaining P' = 0.0081
~C: 0.99 -> +:0.2 -> 0.2 multiplying all for obtaining P' = 0.0396

If we add these complimentary cases, we have: 0.0477 which would be equal to
P'(C|+,+) + P'(~C|+,+).
Since this value is used as a normaliser, that is, it covers the entire space in
which the events described can occur, In order to find the probability of
P(C|++), we divide the P'(C|++) to normaliser.

0.0477 / 0.0081 = ~0.1698

P(C|+,-) = ?

C: 0.01 -> +: 0.9 -> -:0.1 = 0.0009
~C: 0.99 -> +: 0.1 -> -:0.8 = 0,1584
                              0,1593

    C
   / \
 T1  T2

From the above structure we can deduce that T1 and T2 are conditonally
independent.

Conditional Independence
    Given the value of the given element, in the example C, the probability of
    other elements don't change with respect to each other. That is, if we knew
    what C is, the value of T1 would not effect the value of T2 and vice versa.
    It is represented as:

    given C:

    - T1 ⊥ T2

    or more succintly as:

    - T1 ⊥ T2 | C

If the C is not given they are conditonaly dependent, because it changes the
prior probabilities.

Here is an example:
Given the following probabilities:


P(C) = 0.01
P(+|C) = 0.9
P(-|~C) = 0.8

What is the probability of P(T1=+|T2=+) ?

The problem decomposes as the following:

- P(T1=+|T2=+, C)·P(C|T1=+) + P(T2=+|T1=+,~C)·P(~C|T1=+), why ?
  * Because:
  * From : `forums <https://discussions.udacity.com/t/lesson-8-where-did-that-total-probability-equation-come-from/240163/2>`_
    The total probability for a variable is given in terms of all possible
    values for the other variables. So if we wanted the total probability of B
    and the only other variable was A, we would have

    P(B) = P(B|+A)P(+A) + P(B|-A)P(-A)

    Ok, so far, so good. Now let's say we want the total probability of a
    positive test 2 result, +2:

    P(+2) = P(+2|+1, C)P(C|+1) + P(+2|-1, C)P(C|-1) + P(+2|+1, -C)P(-C|+1) + P(+2|-1, -C)P(-C|-1)

    Notice that I expressed each conditional probability in terms of whether or
    not we know the test result for test 1. I think that's because our original
    problem states that we want to condition the +2 probability on a +1 result.

    And speaking of that +1 result, that changes our total probability. The
    equation I wrote above takes into account all possible values for test 1
    (a true "total" probability), but the probability we are looking for is
    P(+2|+1). That means we only want the total probability conditional on a
    specific value for test 1, so we can get rid of any terms where test 1 is
    not positive. That gives us:

    P(+2) = P(+2|+1, C)P(C|+1) + P(+2|+1, -C)P(-C|+1)

    And this is the equation Sebastian starts with in the solution. He then
    removes more terms by pointing out the conditional independence of test 1
    and C and shows we can actually ignore the test results in cases where we
    know C already.

.. note:: Absoulute independence doesn't imply Conditional Independence nor vice
          versa.

Different Causes Same Observation Networks
--------------------------------------------

We have different causes but they get confounded in a single observation.

For example, I am happy, can be caused by having a good day, or a nice workout.
If we instantiate the network with these variables. It would be something like
this.
P(D) = 0.7, good day,
P(~D) = 0.3
P(~W) = 0.99
P(W) = 0.01, good workout,

P(H|D,W) = 1, probability of happy given good day and good workout.

P(H|~D,W) = 0.9, probability of happy given NOT good day and good workout.
P(~H|~D,W) = 0.1

P(H|D,~W) = 0.7, probability of happy given good day and NOT good workout.
P(~H|D,~W) = 0.3

P(H|~D,~W) = 0.1, probability of happy given NOT good day and NOT good workout.
P(~H|~D,~W) = 0.9

P(W|D) = ?
P(W|D) = 0.01

Why ? Because workout and day are independent if we ignore the happiness.
That is so long we are not happy, having a good workout or a sunny day don't
relate to each other.

Explaining Away
-------------------

Special instance of bayes net reasoning.

In our example, explain away manifests as the following:

If the event that we are happy has already happened:

- then the sunny wheather might explain away my happiness, thus it becomes less
  likely that I got a good workout.
- If the sunny wheather doesn't explain my happiness, it becomes more likely
  that I got a good workout.

With this information let's solve the question:

P(W|H,D) = ?

P(W|H,D) = P(W,H,D) / P(H,D) Why ?

Because:

P(W,H,D,E,...) = P(W|H,D,E...) P(H,D,E, ... )

Thus:

P(W,H,D) = P(W|H,D) P(H,D)

Therefore:

P(W|H,D) = P(W,H,D) / P(H,D) = P(H|W,D) P(W|D) P(D) / P(H,D)

W, D are conditonally independent, that is the causes of the events don't effect
each other. Hence their probabilities don't effect each other.

= P(H|W,D) P(W|D) P(D) / P(H|D) P(D)

= P(H|W,D) P(W|D) / P(H|D)

= P(H|W,D) P(W) / P(H|D)

P(H|D) = P(H|R,D) P(R) + P(H|~R,D) P(~R) Why ?

Because:
let's say P(H|D) = s

so for P(s) we would do:

P(s) = P(s,W) + P(s,~W)

P(s) = P(s|W) P(W) + P(s|~W) P(~W)

P(s) = P(H|D|W) P(W) + P(H|D|~W) P(~W)

since D and W are conditonally independent
we can write it as an "and" notation rather
than condition.

P(H|D) = P(H|D,W) P(W) + P(H|D,~W) P(~W)

So the final formula would be:

P(W|H,D) = P(H|W,D) P(W) / P(H|D,W) P(W) + P(H|D,~W) P(~W)

We then plug the numbers.

P(W|H,D) = 1 x 0.01 / 1 x 0.01 + 0.7 x 0.99

Another question:

P(W|H) = ?

P(W|H) = P(H,W) / P(H)

P(H) = P(H,W) + P(H,~W)

This should be expanded in order to recieve D,
because it is also a variable depending on H.

P(H) = P(H,W|D) + P(H,W|~D) + P(H,~W|D) + P(H,~W|~D)

P(H,W|D) = P(D|H,W) P(H,W) / P(D)

P(H,W|D) = P(D,H,W) / P(D)
P(H,W|D) = P(H|D,W) P(D,W) / P(D)
P(H,W|D) = P(H|D,W) P(D|W) P(D) / P(D)
P(H,W|D) = P(H|D,W) P(D|W)
P(H,W|D) = 

With same logic as above:

P(H,W|~D) = P(H|W,~D) P(~D|W)
P(H,W|~D) =

P(H, ~W|D) = P(H| ~W, D) P(~W|D)
P(H, ~W|D) =

P(H, ~W|D) = P(H| ~W, ~D) P(~W|~D)
P(H, ~W|D) =

P(H) = 0.5245

Then the equation becomes

P(W|H) = P(H|W) P(W) / 0.5245
P(W|H) = P(H|W) 0.01 / 0.5245

P(H|W) = s

P(s) = P(s,D) + P(s,~D)

P(s) = P(s|D) P(D) + P(s|~D) P(~D)
P(H|W) = P(H|W,D) P(D) + P(H|W,~D) P(~D)
P(H|W) = 0.7 + 0.3 x 0.9 = 0.97

We plug the number and the equation becomes:

P(W|H) = 0.97 x 0.01 / 0.5245

Explain away phenomenon can be summed up by
looking at the following values:

P(W|H,D) = 1 x 0.01 / 1 x 0.01 + 0.7 x 0.99 = 0.0142
P(W|H) = 0.97 x 0.01 / 0.5245 = 0.0185

The fact that good Day is not given increases the chance that the workout was
the cause of happiness.

final question on explain away:

P(W|H, ~D) = ?
P(W|H, ~D) = P(W, H,~D)  / P(H,~D)
           = P(H|W,~D) P(W,~D) / P(H,~D)
           = P(H|W,~D) P(W|~D) P(~D) / P(H,~D)
           = 0.9 x P(W)=0.01 x 0.3 / P(H,~D)

(
P(H, ~D) = P(H|~D)P(~D)

P(H| ~D) = s

P(H|~D) = P(s,W) + P(s,~W)

P(H| ~D) = P(s|W) P(W) + P(s|~W) P(~W)

P(H| ~D) = P(H|~D|W) P(W) + P(H|~D|~W) P(~W)

P(H| ~D) = P(H|~D,W) P(W) + P(H|~D,~W) P(~W)

P(H| ~D) = 0.9 x 0.01 + 0.1 x 0.99
P(H| ~D) = 0.009 + 0.099
P(H| ~D) = 0,108

P(H, ~D) = 0,0324
)
          = 0,9 x 0,01 x 0,3 / 0,0324
          = 0,0833

Observation on the Explain Away effect:
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

P(W|H,D) = 1 x 0.01 / 1 x 0.01 + 0.7 x 0.99 = 0.0142
P(W|H,~D) = 0,0833
P(W|H) = 0.97 x 0.01 / 0.5245 = 0.0185

By looking at these values we can see that, if happiness is related to both a
good day and a workout,
when we are only given that the event that we are happy has occured, the
probability of having done a workout, is more than being given that the event
that we are happy has occured and it is a good day.
However the most striking part is if we are given that the event that we are
happy has occured and that it is NOT a good day, the probability of having done
a workout increases considerably.

Simply put, when there is a knowledge of two joint causes for a node, and when
one is given as true, we tend to discredit the other, if it is given as false we
tend to credit the other.

Another interesting phenomenon is that the knowledge of the effect of the causes
renders the cause nodes to be dependent.


Bayes Networks
===================

Bayes networks define probability distributions over graphs of random variables.

Example network:

A     B
 \   /
   C
 /   \
E     F

Its probability distributions:

P(A)      P(B)
   P(C|A,B)
P(E|C)  P(F|C)

Joint probability represented by the bayes network:
P(A,B,C,E,F) that is, A, B, C, E, F are the variable names
that represent the probability distributions which are the
actual nodes of the graph.

Each incoming arc is a condition.
P(A,B,C,E,F) = P(A) * P(B) * P(C|A,B) * P(E|C) * P(F|C)

The advantage of Bayes networks that it represent a large data with less
variable.
That is for joint distribution on any five variables, we would need 2^5 -1
probability values.
Whereas for Bayes networks we need only 10, they decompose as the following:

P(A), needs one value
P(B), needs one value
P(C|A,B), needs four values
P(E|C), needs two values
P(F|C), needs two values

How do we know?

We count the arcs, and place it as the power of two. Ex P(A), 0 arc, 2^0 = 1,
P(C|A,B), 2 arcs, 2^2 = 4

3 + 6 + 8 + 12 + 16

D-Separation
---------------

Any two variables are independent if they are not linked by just an unknown
variable. Ex:

        A
       / \
      B   D
     /     \
    C       E

Unless we know what A is, C and D are related by virtue of B,A, but the moment
we know what A is,
D becomes independent, because D can not say anything more than what A does
concerning C.


