#########################
Inference in Bayes Nets
#########################

The point of this unit is to be able to answer probabilistic questions using
bayes nets.

Instead of input and output, in probabilistic inference we call the givens,
evidence, and the results, query.
The variables we know the values of are the evidence, and the variables we want
to find out are the query.
Anything that is neither evidence, nor query is a hidden variable.

The output is not a number, but rather a probabilistic distribution.
So the answer is going to be a complete joint probability distribution over
query variables.
It is called the posterior distribution given the evidence.
It is expressed as the following:

P(Q_1, Q_2, ... | E_1=e_1, E_2=e_2, ...), this is the computation we want to
come up with.

Another reasonable question would be to ask which combination of values would
yield highest probability, so something like this:

argmax_q P(Q_1=q_1, Q_2=q_2, ... | E_1=e_1, E_2=e_2, ... )

Which q values are maxiable given the evidence values is question we can ask.

The direction of computation is irrelevant, that is we can have e_1, e_2 and
compute q_1, etc. or have q_1 and compute all the rest, etc.
This is to notice that the direction of computation in ordinary programming
languages are in one way, from input to output, this is not the case for bayes
nets inference.

Exact Inference
=================

Enumeration
----------------

Enumeration is a method to calculate the probabilities in a given bayes net, it
simply enumerates all the possibilities and adds them up and thus comes up with
an answer.

Here is how it works:

Example bayes net graph:

 B    E
 .\ ./
   A
 ./ .\
 J    M

The problem:
P(+b|+j,+m) = ?
It reads as if b is true, j is true, m is true, given that the events j, and m
are already happened, what is the probability of the event b would occur.

Enumeration works as follows:

First we express the conditional probabilities as unconditional probabilities.
So,

P(+b|+j,+m) = P(+b,+j,+m)/P(+j,+m)

Then we enumerate the atomic probabilities and calculate the sum of their
products.
P(+b|+j,+m) the probability of these terms can be determined by enumerating all
possible values of the hidden variables. In this case there are two E and A.
So we shall sum over those variables for all values of E and A.

Formally:

.. math::

   {\sum_{e}}{\sum_{a}} P(+b,+j,+m,e,a)

"a" and "e" represent here the possible value that E, and A can have in given
net.

We transform this equation to a product of 5 variables by expressing each of the
nodes in relation to their parent nodes in the net.

that is: :math:`{\sum_{e}}{\sum_{a}} P(+b) P(+j|a) P(+m|a) P(e) P(a|+b,e)`

The product section can be named as a function, f(e,a)
We look up the values from the table of the bayes net, which is created during
the creation of the net.

This enumeration technique works for small networks, bigger networks would
increase the number of rows to lookup.

So we need to find a way to speed up the enumeration.

Pull out terms
----------------

We can pull terms out from the enumeration, for example

:math:`{\sum_{e}}{\sum_{a}} P(+b) P(+j|a) P(+m|a) P(e) P(a|+b,e)`

In the above equation the P(+b) is the same throughout the summation
thus it doesn't need to be added to the summation everytime, we can
put it in front of the summation. P(e) is also independent of "a",
so it can also come before the summation of the terms related to a.

But this won't decrease the amount that much.

Maximize independence
-----------------------

This is a technique for efficient inference.
The structure of the bayes net determines how efficient it is to do
inference on it.


Causal Direction
---------------------

The important thing is that Bayes nets tend to be the most compact,
and thus the easier to do inference on when they are written
in the causal direction, that is, when the networks flow from the causes to
directions.

Variable Elimination
-----------------------

This is a technique with several steps.
We try to reduce the network to the smaller networks.

First step is joining factors.

Joining Factors:

- A factor is a multi dimensional matrix, it is a table of properties for a
  given node.
- We choose two or more of the factors
- We will combine them, and it will represent the joint probability of all the
  variables in that factor.

For example if we have the tables for P(r) and P(t|r), by multiplying the values
in the tables, we can have
P(r,t)

After this we do the second step, elimination or summing out, or marginalisation.

Elimination:

- We choose a variable that is **absent** in the next table.
- We sum out the choosed variable based on the variable that is **present** in
  the next table.

Example operation:


+--------+-------------+
| P(T,L) |             |
+========+====+========+
| T      | L  | Values |
+--------+----+--------+
| +t     | +l | 0.051  |
+========+====+========+
| +t     | -l | 0.119  |
+--------+----+--------+
| -t     | +l | 0.083  |
+========+====+========+
| -t     | -l | 0.747  |
+--------+----+--------+

P(+l) = 0,051 + 0,083 = 0,134
P(-l) = 0,119 + 0,747 = 0,866

Approximate Inference
=======================

We use sampling.
That is we stimulate the events in the nets.
The more samples we have, the closer we approach to exact inference situation.

The sampling have the following advantages:

- We know a procedure for coming up with an at least approximate value for the
  joint probability distribution
- If we don't know what the conditional probability tables are, but we could
  simulate the process, we could still proceed with the sampling
We couldn't with exact inference.

Sampling method is *consistent*, that is if we have infinite number of samples
we can calculate the true joint probability.

For calculating conditional probabilities, we look at the samples that interest
us.
For example,
If we have sample like P(+C,-S,+r,-l), and if we are looking for P(-C|+r), since
our sampling is based on P(+C), we reject at,
and keep the ones with P(-C) in it. This proceedure is called
*rejection sampling*.

The problem with rejection sampling is that if the evidence is unlikely, you end
up rejecting a lot of samples.

To fix that we use a technique called, *likelihood weighting*

Likelihood Weighting
----------------------

This is a technique in which we fix the samples.
For example, let's say when the burglary occurs, alarm goes of, so we have
structure like B -> A.
For a question like what is the probability of burglary occuring in the event
that alarm goes off, P(B|+a) ?
Following would happen.
Since the burglaries are infrequent, during the sampling process, we would
generate mostly P(-b, -a), and reject them
because we are looking for P(B|+a)
So we say, during the sampling process, that the value of "a" is positive from
the start.
This way we get to keep every sample, but this method is **inconsistent**, that
is by having infinitely many samples obtained this way, we can not obtain the
true joint probabilities.

However that can be fixed by adjusting the weighting of the samples on the
outcome.

Example Calculation:

Let's say we are trying to calculate P(R|+s,+c)

for the following network

     Cloudy
    /      \
Sprinkler  Rain
    \      /
     Wet Grass

We have 4 tables:

+------+-----+------+
|P(S|C)|            |
+======+=====+======+
| +c   | +s  | 0,1  |
+------+-----+------+
|      | -s  | 0,9  |
+------+-----+------+
| -c   | +s  | 0,5  |
+------+-----+------+
|      | -s  | 0,5  |
+------+-----+------+


+------+-----+------+
|P(R|C)|            |
+======+=====+======+
| +c   | +r  | 0,8  |
+------+-----+------+
|      | -r  | 0,2  |
+------+-----+------+
| -c   | +r  | 0,2  |
+------+-----+------+
|      | -r  | 0,8  |
+------+-----+------+

+------+-----+
| P(C) |     |
+======+=====+
| +c   | 0,5 |
+------+-----+
| -c   | 0,5 |
+------+-----+


+------+-----+------+------+
|P(W|R,S)                  |    
+======+=====+======+======+
| +s   | +r  | +w   | 0,99 |
+------+-----+------+------+
|      |     | -w   | 0,01 |
+------+-----+------+------+
|      | -r  | +w   | 0,90 |
+------+-----+------+------+
|      |     | -w   | 0,10 |
+------+-----+------+------+
| -s   | +r  | +w   | 0,90 |
+------+-----+------+------+
|      |     | -w   | 0,10 |
+------+-----+------+------+
|      | -r  | +w   | 0,01 |
+------+-----+------+------+
|      |     | -w   | 0,99 |
+------+-----+------+------+

P(R|+s,+w)

Let's say we have the following sample:

P(+c, +s, +r, +w) what is its weight ?

Since the "s" and the "w" are constrained by the problem,
we apply the product of their probabilities as weights,
(P(+s|+r)=0,1 * P(+w|+r,+s)=0,99) = 0,099


Gibbs Sampling
------------------
This technique uses a method called Markov Chain Monte Carlo (MCMC):
 we resample one variable at a time conditioned on all the other.
The variables are dependent, and the technique is consistent.


Monty hall problem
------------------

Three doors, one with goat, empty, car.
These are boolean.
Goat is given as +
What is the
There is one selected, and the goat is given thus, my selection is not goat.

P(+c)|P(-c), P(+e)|P(-e), P(+g)|P(-g)
1/3  | 2/3,  1/3  | 2/3   1/3  | 2/3


P(C,E,G) = 1/27
P(+c,+e,+g) = 1/27
P(+c,-e,+g) = 2/27
P(-c,+e,+g) = 2/27
P(-c,-e,+g) = 4/27
P(+c,-e,-g) = 4/27
P(+c,+e,-g) = 2/27
P(-c,+e,-g) = 4/27
P(+c,-e,-g) = 4/27
P(-c,-e,-g) = 8/27

P(+g), + P(C,E|-g), + P(C) = 1
given, empty door, selection
           2/3         1/3 

