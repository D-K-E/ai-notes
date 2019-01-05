##############################
Planning Under Uncertainity
##############################

We have seen different types of environments:

+------------+---------------+------------+
|            | Deterministic | Stochastic |
+============+===============+============+
| Fully      | A* star, DFS, |MDP         |
| Observable | BFS,UniformCS |            |
+------------+---------------+------------+
| Partially  |               |   POMDP    |
| Observable |               |            |
+------------+---------------+------------+

Remember:
Stochastic: We don't know for sure the result of the actions

Deterministic: The outcome of the action is always the same and predictable

Fully Observable: Every state is visible from the current state, so you would
need only momentary sensory input for making a decision.

Partially Observable: You would need a memory for making a decision

MDP (Markov Decision Process)s
================================

Markov Decision Process are composed of the following:

- States: {s_1, s_2, s_3, ..., s_n}
- Actions: {a_1, a_2, a_3, ..., a_n}
- State Transition Matrix: T(s,a,s') = Probability(s'|a,s):
  The matrix gives the transition probabilities of moving to another state
  given the action and current state.
- Reward function associated to states for designating a goal
- Policy: An action  that is associated to each state to reach to the goal.
- The planning problem is about finding the good and efficient policy

The cost function in MDPs can be defined as follows:

Reward(s) = {Goal Reward (a large positive value),
             Avoid rewards (large negative value),
             Step Cost (Small negative value)}

The objective of MDP can be defined as follows:
:math:`objective = max(Expactation[{\sum_{t=0}^{infinity}({\gamma}^t)R_t}])`

- t: time value
- gamma: discount factor, value between 0 and 1: 0 < gamma < 1
- R_t: cost function result at the time t.

The objective is to maximise the expectation from the sum of cost functions.
The discount factor decays the future reward relative to more immediate rewards
and it's kind of an alternative way to specify step cost, it is basically there
to give an incentive to reach the goal as fast as possible.
The good mathematical thing about the discount factor is that it keeps the
sigma expression bounded.
By specifying the discount factor it is easy to show the following bound:

:math:`{\sum_{t=0}^{infinity}({\gamma}^t)R_t}{\le}{\frac{1}{1-{\gamma}}}{\times}R_max`

The R_max is in most cases Goal Reward

Value Iteration
----------------

Once we define a cost function, which in turn helps us to define an objective,
we are ready to designate a value for each state.

Value in this case would be:

:math:`V^{\pi}(s) = E_{\pi}[{\sum_{t=0}^{infinity}({\gamma}^t)R_t}|s_0=s]`

The expression looks complex but it is actually simple to decompose:
It says if we execute the policy pi, the expectation for the sum of the reward,
given that the initial state of the policy is the current state, is the value of
the state.
So basically: :math:`V^{\pi}`, says we are operating under the policy pi, same
thing with E.

Value function is a potential function that leads from the goal location all the
way into the state space, so that the hill climbing in this potential function
leads you on the shortest path to the goal.

Here is an example of its application:

+----+---+---+---+------+
|    | 1 | 2 | 3 | 4    |
+====+===+===+===+======+
| a  | 0 | 0 | 0 | +100 |
+----+---+---+---+------+
| b  | 0 | X | 0 | -100 |
+----+---+---+---+------+
| c  | 0 | 0 | 0 | 0    |
+----+---+---+---+------+

Now we ask the question:

Is 0 is a good value for field a3 ?


The answer is no, beca can compute a better one and the reason is evident in the
function.

For a state space in which:

Reward(s) = {Goal Reward: +100
             Avoid rewards: -100
             Step Cost: -3}

With the transition matrix of:

Going East 0.8 chance
Going south 0.1 chance
Staying in the same state 0.1 chance


V(a3,E) = 0.8 * 100 -3 = 77

E: east, denoting going east.

We see thus that for the current state, 0 is not a good value, 77 is a better
value. If we do this until the convergence of the value function algorithm, we
would have the following matrix:

+----+----+----+----+------+
|    | 1  | 2  | 3  | 4    |
+====+====+====+====+======+
| a  | 85 | 89 | 93 | +100 |
+----+----+----+----+------+
| b  | 81 | X  | 68 | -100 |
+----+----+----+----+------+
| c  | 77 | 73 | 70 | 47   |
+----+----+----+----+------+

The algorithm is called the value iteration:
From AIMA-python

.. code-block:: python

   def value_iteration_instru(mdp, iterations=20):
    U_over_time = [] # List for updated state value dictionary
    U1 = {s: 0 for s in mdp.states} # state value dictionary
    R, T, gamma = mdp.R, mdp.T, mdp.gamma # R for reward, T, transition matrix
    # in the form of P(not_current_state|current_state, current_action)
    for _ in range(iterations):
        U = U1.copy()
        for s in mdp.states:
            U1[s] = R(s) + gamma * max([sum([p * U[s1] for (p, s1) in T(s, a)])
                                        for a in mdp.actions(s)]) # by maxing
                                        # over every action of s we guarantee to
                                        # choose the best policy
        U_over_time.append(U)
    return U_over_time


POMDP (Partialy Observable Markov Decision Process)
===================================================

POMDPs address the problem of optimal exploration versus exploitation, there
would be a information gathering actions, and goal driven actions.

