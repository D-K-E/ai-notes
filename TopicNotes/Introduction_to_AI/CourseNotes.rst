Introduction to ai
#####################

What makes an ai is the capacity to react to changes.

Edges embody the rules of action, whereas the nodes embody the states for a game playing agent.

Intelligence should be defined within the context of a task.

An agent interracts with the environment by sensing its properties.
Actions change the state of environment.


Types of Ai problems:
*********************

AI problems are classified according to the properties of the environment states.

Environment States:

- It can be fully observable like a chess board.
- It can be partially observable like "yazlıkçı okeyi" where you can see the boards of the oponents.
- It can be deterministic
  - You know for sure the results of each action.
- It can be stochastic:
  - You don't know for sure the results of each action.
- It can be discrete:
  - There is a finite number of states the environment can be in.
- It can be continous:
  - The number of possible states of the environment is infinite.
- It can be benign:
  - The agent is the only one that is taking actions that intentionally effects its goal.
- It can be adverserial:
  - There can be other agents that can take actions to defeat its goal.


Intelligent agent:

It is one that takes actions in order to maximise expected utilty given a desired goal.
This is difficult to measure, so we use bounded optimality.



Alpha-beta pruning can provide performance improvements when applied to expected max game trees that have finite limits on the values the evaluation function returns.

Isolation game:
You move on the board, diagonaly or columnwise or row-wise, you can't move into occupied or previously occupied squares, the goal is to be the last player moving.

Mini-max Algorithm:
We assume that the opponent tries to minimze our scores, and we always try to maximise our scores.
Triangles pointing up, is the turns where we try to maximise our score.
Triangle pointing down, marks the turns where the oppononet is trying to minimse our scores.

The algorithm is as follows:

- For each max node, pick the maximum value among its child nodes, if there is at least one plus one child, the computer can always pick that to win. Otherwise it can never win. At each mid node we chose the minimum value to represent the opponent.


Supplemented Reading:
Norvig, Peter, and Stuart Russell, Artificial Intelligence: A Modern Approach, 3rd edn (New York, 2010), sections 5.1-2.

Summary:

Games are for the most part competitive environments, that is agent's interests are in conflict with each other.
This gives rise to adversarial search problems.
The difference between the mathematical game theory and its application in AI:

For mathematical game theory, every multi agent environment in which the agents act upon each other is *significant* is considered as a game environment, they are labeled as economies.
In AI, the most common games are zero-sum games of perfect information (chess, go, etc), that is: "this means deterministic, fully observable environments in which two agents act alternately and in which the utility values at the end of the game are always equal and opposite."

The formal definition of a game:

:math:`S_0`
    The initial state, which specifies how the game is set up at the start.

PLAYER (s)
    Defines which player has the move in a state.

ACTIONS(s)
    Returns the set of legal moves in a state.

RESULT(s, a)
    The transition model, which defines the result of a move.

TERMINAL - TEST (s)
    A terminal test, which is true when the game is over and false
otherwise. States where the game has ended are called terminal states.

UTILITY (s, p):
    A utility function (also called an objective function or payoff function), defines the final numeric value for a game that ends in terminal state s for a player p. In chess, the outcome is a win, loss, or draw, with values +1, 0, or 12 . Some games have a wider variety of possible outcomes; the payoffs in backgammon range from 0 to +192.

A **zero-sum** game is (confusingly) defined as one where the total payoff to all players is the same for every instance of the game.
Chess is zero-sum because every game has payoff of either 0 + 1, 1 + 0 or 12 + 12 .
*Constant-sum* would have been a better term, but zero-sum is traditional and makes sense if you imagine each player is charged an entry fee of :math:`{\frac{1}{2}}`


The relationship between the iterative deepening and the visited tree nodes is the following:

The number of nodes visited during the iterative deepening is the cumulative sum of all the visited nodes up to that level.

Visited tree node: 1
Iterative tree node: 1
Visited tree node: 4
Iterative tree node: 5
Visited tree node: 13
Iterative tree node: 18
Visited tree node: 40
Iterative tree node: 58
Visited tree node: 121
Iterative tree node: 179

The general formula for the iterative deepening is:

.. math::

   b = K

   n = {\frac{K^{d+1} - 1}{K - 1}}

b
    The branching factor. Gives how much the tree would branch out at each level.

n
    The number of nodes in the tree.

d
    The level indicator. Indicates the level of the tree.


Evaluation function in the case of isolation game:

With an evaluation function like simply counting the number of moves our computer player
has available at a given node, the player would select branches in the mini-max tree that
lead to spaces where player has most options.

Good evaluation function for isolation is
number_of_my_moves - number_of_opponent_moves

It promotes the boards in which i have more moves than my opponents, and penalises the boards in which the opponent has more moves.

.. code-block:: python

   # Here is a pseudo-code for
   # Minimax algorithm with alphabeta pruning
   minimax(root) = max(min(3, 12, 8), min(2, x, y), min(14, 5, 2))
   minimax(root) = max(3, min(2,x,y), 2)
   z = min(2,x,y)
   # Then z <= 2
   minimax(root) = max(3,z,2)
   minimax(root) = 3


Expectimax alpha-beta pruning
*****************************

This expectimax algorithm works in the case where we don't really know what the outcome will be.
That is we have several different outcomes with different probabilities.
Expectimax uses the minimax tree which represents all the possible outcomes in the game.
Each branch has a probability, and to propagate a value to a superior node, we take the values and multiply them with the probabilities
assigned to their branches, then depending on the type of branch, that is min or max branch

An Example Tree:

Max:
node 1: ; node 2: ; node 3:

Min:

Node1B1 Probability: 0.1
Node1B2 Probability: 0.5
Node1B3 Probability: 0.4

Node2B1 Probability: 0.5
Node2B2 Probability: 0.5

Node3B1 Probability: 0.5
Node3B2 Probability: 0.1
Node3B3 Probability: 0.4

Node1B1Leaf1: -4
Node1B1Leaf2: -5
Node1B2Leaf1: 5
Node1B2Leaf2: 6
Node1B3Leaf1: 8
Node1B3Leaf1: 10

Node2B1Leaf1: 0
Node2B1Leaf2: 10
Node2B2Leaf1: 10
Node2B2Leaf2: 5

Node3B1Leaf1: -7
Node3B1Leaf2: -3
Node3B2Leaf1: 9
Node3B2Leaf2: 10
Node3B3Leaf1: 2
Node3B3Leaf1: 5

An Example Calculation without pruning:

Min:

Node1: :math:`-5{\times}0.1 + 0.5{\times}5 + 8{\times}0.4 =5.2`

Node2: :math:`0{\times}0.5 + 5{\times}0.5 = 2.5`

Node3: :math:`-7{\times}0.5 + 9{\times}0.1 + 2{\times}0.4 = 1.8`


Max:

Node1: 5.2

An Example Calculation with Pruning:

Min:

Node1:  :math:`-5{\times}0.1 + 0.5{\times}5 + 8{\times}0.4 =5.2`

Node2: if Node2B1Leaf1 is 0, then we can prune all the other branches in the middle branch.
Why ?

Because we know that the values range from -10 to 10, and that the top branch is 5.2, meaning that we assume
Node2 is either equal to or less than 5.2, and the probability distribution is 0.5. This means that even if all
of the rest of the branches yield 10, the final outcome for the branch would be less than or equal to 5. Thus
they would not change the value of the top branch.

Then
Node2: <= 5

Node3: if Node3B1Leaf1 is -7 then we can safely ignore everything else. Because as in the above branch, even if
every branch would yield 10, their sum would not be greater than 5.2.

Then
Node3: <= 1.5

1.5 comes from the fact that we evaluate as if the other branches yielded 10, and multiply it with the probabilities associated with the
branches and then sum everything up.








Terms:
******

Heuristic
    Some additional piece of information - a rule, constraint, function - that informs an otherwise brute-force algorithm to act in a more optimal manner

Pruning
    Pruning the search tree is to evaluate certain branches of the search tree by easy to check conditions. It allows us to ignore portions of the search tree that make no difference to the final choice

Mini-Max Algorithm
    You are trying to maximize your chances of winning on your turn, and your opponent is trying to minimize your chances of winning on his turn. What we do is we map every possible move in the game, and map them to min or max states. Then depending on the type of the state we either take the maximum value indicated in the child nodes or the minimum value indicated in the child notes.

Depth-Limited Search
    Going as far as we can to safely meet our deadline.

Horizon Effect
    When it is obvious to human player that the game will be decided in the next move, but the depth of search doesn't let the computer player to see this happening. This means that some critical moves are not noticed by the machine. These critical moves occur because of the shape of the environment, that is because of a property of environment. Our agent nodes how to prioritise the nodes, but that doesn't mean that it is aware of how the nodes are situated in relation to each other.

Evaluation Function
    It evaluates the possible moves so that the minimax driven agent can choose a move.

Alpha-Beta Prunning
    Alpha-beta pruning works as follows. If a tree is on the max turn, upon viewing the first branch, we know that if a value is to be propagated to the superior branch other than the first one, it has to be bigger than the value of the first branch, thus we can ignore the branches that comes up with a lower or equal value. If a tree is on the min turn, upon viewing the first branch, we know that if a value is to be propagated to the superior branch other than the first branch value, then it has to be smaller than the value of the first branch, thus we can ignore the branches that comes up with a superior or equal value.
    α
        the value of the best (i.e., highest-value) choice we have found so far at any choice point along the path for MAX.
    β
        the value of the best (i.e., lowest-value) choice we have found so far at any choice point along the path for MIN.

Immediate Pruning
    Immediate pruning happens when a branch fulfills the necessay conditions to be designated as the node's value immediately. For example, let's say the condition is that the sum of the numbers inside a node has to be equal to 10. If the first branch is 10 we don't need to look at the other values.

