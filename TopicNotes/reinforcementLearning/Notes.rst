##########################
Reinforcement Learning
##########################

Forms of Learning:

- Supervised Learning: (x_1, y_1), (x_2, y_2): y = f(x)
  Ex: Speech recognition
- Unsupervised Learning: x_1, x_2, x_3, ... P(X=x_1), we search for a probability distribution, or a cluster
  Ex: Clustering, light emmissions coming from the space.
- Reinforcement Learning: state, action, state, action, ... . There are some rewards associated with some of these states. Rewards are just scalar numbers, positive, negative etc. We search for an optimal policy, that gives us what to do in any given state.
  Ex: Elevator controller

MDP Review - Markov Decision Process
=====================================

MDPs consist of a set of states, a set of actions available for a state, transition function which gives a result state, and a reward function.

Formally, :math:`s {\in} S`,
:math:`a {\in} Actions(s)`,
:math:`P(s'|s,a)`,
also an initial state :math:`s_0`,
sometimes reward function that is general over the triplet
:math:`R(s,a,s')` or sometime we only talk about
the result state :math:`R(s')`

To solve an mdp, we try to find an policy, that can maximise discounted total reward

Reinforcement Learning
========================

Reinforcement Learning comes into play when we don't know the reward function or even the transition modal of the world.
When we don't know these we can't solve the MDP, however with reinforcement learning,
we can find these, either by interacting with the world, or you can learn substitutes that would tell you as much as you
know, so that you never have to actually compute the reward or the transition model.

Several choices exist for modaling:

+---------------------+--------+-----------+---------+
| agent               | know   | learn     | Utility |
+=====================+========+===========+=========+
| utility based agent | P      | R ? -> U | U        |
+---------------------+--------+-----------+---------+
| Q Learning          | ?      | Q(s,a)    | Q       |
+---------------------+--------+-----------+---------+
| reflex agent        | ?      | Policy(s) | Policy  |
+---------------------+--------+-----------+---------+

Q(s,a): is like a utility function evaluating state action pairs. We don't know transition modal and reward function.
Reflex agent: is just about policy, it is called reflex agent because it is pure stimulus response.

Passive Reinforcement Learning:
    Passive means agent has fixed policy and executes that. During that process it learns about R and/or P, transition modal.

Active Reinforcement Learning
    Active implies that we change the policy as we learn about the world.

Passive Temporal Difference Learning
-------------------------------------

This needs a table of utilities for each state, we also keep track of how many times we visited each state.
Table of utilities should start out as blank, and the table of number of visits should start out with zeros
