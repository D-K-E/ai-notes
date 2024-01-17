# N-Dimensional Geometry

Notes from Murty, K. (2001) Computational and Algorithmic Linear Algebra and
n-Dimensional Geometry. Ann Arbor. Chapter 3

let $S \subset R^n$ and $x \in R^n$, the translation of S to x is the set of
points: $ \{ x + y | y \in S \} $

Parametric representation of a point in $S \subset R^n$ is a vector of
functions, such as:
$x = (x_1, x_2, \dots, x_n)^T 
   = (f_1(a_1, \dots, a_r), \dots, f_n(a_1, \dots, a_r) ) $

With no constraints the points in S can take any real number. We can also
introduce constraints on functions to determine the points in S, so something
like the following is possible:

$S = \{
(f_1(a_1, \dots, a_r), \dots, f_n(a_1, \dots, a_r) )_j | 
    j \in \{1,\dots, n \},
    a \in C(a) \}$
where $C(a)$ represents a subset of $R^n$ that satisfies a set of constraints.

A line in N dimensions is:
$ x = a + \alpha b$ where $a, b \in R^n$ and $b \not = 0$ meaning that at least
one component of the vector b must be a non zero value.
Notice that it has single parameter $\alpha$ which can take all real values
from $R$, that is, it is a scalar value that gets multiplied with each
component of the vector b.

In order to check whether a point is in a line or not

```Python

def in_line(a: List[float], 
            b: List[float], 
            alpha: float,
            x: List[float]) -> bool:
    """
    Checks if point x is in line of the equation (a + b * alpha)
    """
    check = True
    for i in range(len(a)):
        if (a[i] + b[i] * alpha) != x[i]:
            check = False
    return check
```
Check whether there is a line between two points:

```Python

def have_a_line(a: List[float], c: List[float]) -> bool:
    """
    We check if a line might exist between these two points.
    and try to obtain the parametric representation (a + b * alpha = c)
    since b = c - a in alpha 1
    """
    if a == c:
        return False
    b = [c[i] - a[i] for i in range(len(a))]
    return any(b[i] > 0 for i in range(len(b)))
```

Two parametric representations of lines $U$ and $V$ in $R^n$ correspond to the
same line if the following conditions hold true:

- $U = \{ x = a + \alpha * b | \alpha \in R, a, b \in R^n, b \not = 0 \}$
- $V = \{ x = c + \beta * d | \beta \in R, c, d \in R^n, d \not = 0 \}$

- $d$ is a scalar multible of $b$
- $a - c$ is a scalar multiple of $b$

```Python

def check_parametric_same_line(a: List[float], alpha: float,
                               b: List[float],
                               c: List[float], beta: float, 
                               d: List[float]) -> bool:
    """
    Two parametric representations of lines $U$ and $V$ in $R^n$ correspond to
    the same line if the following conditions hold true:
    $d$ is a scalar multible of $b$
    $a - c$ is a scalar multiple of $b$
    """
    prev = None

    # check if 'd' is a scalar multiple of 'b'
    check = True
    for i in range(len(b)):
        if prev is None:
            prev = b[i] / d[i]
        else:
            check = b[i] / d[i] == prev
    if not check:
        return check

    # check if 'a - c' is a scalar multiple of 'b'
    for i in range(len(b)):
        if prev is None:
            prev = b[i] / (a[i] - c[i])
        else:
            check = b[i] / (a[i] - c[i]) == prev
    if not check:
        return check
    return check
```

Half lines and rays in N dimension:

The parameteric representation of a half line is 
$\{ x = a + \alpha b | \alpha \ge k \}$ where 

- $b \not = 0$, 
- $a,b \in R^n$
- k is a constant value.

Notice that this includes a set of points that is a subset of line's
representation which does not impose a condition on $\alpha$
In this context:

- $a$ is the starting point of half-line
- $b$ is the direction vector of the half-line

Using this representation a half line can also be characterized as a function,
such as, 
$ x = x(\alpha) 
    = \{ x_1(\alpha), x_2(\alpha), \dots, x_n(\alpha) | \alpha \ge 0 \} $
where $ x(\alpha) = a + \alpha b $
when we say a half line begins at $a$, we assume that $\alpha = 0$ and that
$b$ is the direction vector.


A ray is a half line that starts at 0, meaning that
The parametric representation of a ray is:

$\{ x = a + \alpha b | \alpha \ge k \}$ where 

- $b \not = 0$
- $b \in R^n$
- $a = \{ 0, 0, \dots, 0 \} \in R^n $
- $k$ is a constant value


For every point that is not equal to all zeros, that is 
$p = \{p_0, p_1, \dots, p_i, \dots \} \in R^n$ where $p_i \not = 0$
there is a unique ray which can be represented as:
$\{x = \alpha p : \alpha \ge 0 \}$ $p$ here is called the direction of the
ray. Notice that the same term corresponds to direction vector in the half
line equation.

Let's us consider the following half line X:
- $ \{ x = a + \alpha b | \alpha \ge k \} $

This half line begins at $a$ meaning that $\alpha = 0$

Let's us consider the following ray Y:
- $\{ x = a + \beta b | \beta \ge k \}$, where $a = \{0, 0, \dots, 0\} \in R^n$

In this case X is parallel to Y. The point $x$ on half line that begins in $a$
is translated by $\alpha / \beta$ amount in the direction of $b$.

Two half lines $L_1$ and $L_2$ are same if and only if:

- $L_1 = \{ x = a + \alpha b: \alpha \ge 0 \}$
- $L_2 = \{ x = c + \beta d: \beta \ge 0 \}$

- $a=c$, that is their starting point is the same
- $(b / d) > 0$ that is b and d are positive scalar multiplies of each other.

Meaning that when comparing half lines for equality the exact value of
direction does not matter so much as long as both directions are positive.

Line Segments in $R^n$
-----------------------

A line segment in $R^n$ is expressed as a condition on a ray in $R^n$. Simply
it is expressed by the following equation:

- $L = \{ x = a + \alpha b : \beta \le \alpha \le \theta \}$
- where $b \not = 0$ and $\beta < \theta$

We can express the line using interpolation of coefficients between the
$\alpha$ and $\theta$ as well. Let's remember the linear interpolation formula
is:
- $((x - min) / (max - min)) $

Adapted to our case this would mean:
- $ (x - \beta) / (\theta - \beta)$

if we define

- starting point as: $P_s = a + \beta b$
- ending point as: $P_e = a + \theta b$

we can define the points in between using the step length $ c = (\theta - \beta)$
with the direction being the same $b$.

Let's say the new direction vector is expressed using the step length $c$ as
in: $b' = c b$

The points of the line can be then expressed as:

- $L = \{ x = P_s + \delta b': 0 \le \delta \le 1 \}$

Let's see why:
- $L = \{ x = (a + \beta b) + \delta b': 0 \le \delta \le 1 \}$
- $  = \{ x = (a + \beta b) + \delta c b: 0 \le \delta \le 1 \}$
- $  = \{ x = a + \beta b + \delta c b: 0 \le \delta \le 1 \}$
- $  = \{ x = a + b (\beta + \delta c) : 0 \le \delta \le 1 \}$
- $  = \{ x = a + b (\beta + \delta (\theta - \beta)) : 0 \le \delta \le 1 \}$

Let's plug in the extreme values of $\delta$

$\delta = 0$ case:
- $  = \{ x = a + b (\beta + 0 (\theta - \beta)) : 0 \le \delta \le 1 \}$
- $\{ x = a + b \beta\} = P_s$

$\delta = 1$ case:
- $  = \{ x = a + b (\beta + 1 (\theta - \beta)) : 0 \le \delta \le 1 \}$
- $\{ x = a + b (\beta + \theta - \beta \}$
- $\{ x = a + b \theta\} = P_e$

We obtained the starting and ending points of the line, thus our
representation is correct, so another representation of the line would be

- $L = \{ x = P_s + \delta b': 0 \le \delta \le 1 \}$

Parallelogram law:

let: $p_1 = L_1(x) = a + x b$ and $p_2 = L_2(y) = c + y d$ 
where $L_1$ and $L_2$ are lines that start at origin and contain $p_1$ and
$p_2$ respectively. The addition of two points, that is $p_1 + p_2$ creates a
parallelogram whose corners are $O, p_1, p_2, p_1 + p_2$. The sides that are
parallel are O-$p_1$, $p_2$-$p_1 + p_2$ and $p_1$-$p_1 + p_2$, O-$p_2$ where O
represents the origin.

Linear Combinations and Subspaces
----------------------------------------

A subset $S \in R^n$ is a subspace of $R^n$ if it satisfies the following condition:

- All linear combinations of any two vectors $a, b \in S$ must be a member of S. 

Basically for all $a \alpha + b \beta = c$ where $\alpha, \beta$ are real values, $c \in S$.
Examples of subspaces are various coordinate planes of various dimensions whose origin is 0.

A linear combination such as $a_1 c_1 + a_2 c_2 + \dots + a_k c_k$ is composed of two parts:

- coefficients: $c_1, c_2, \dots c_k$ which can take any real number
- vectors: $a_1, a_2, \dots a_k$ which is a set of $k$ vectors in $R^n$.

The set of all combinations of $A = \{a_1, a_2, \dots, a_k\}$ is a linear hull, or the subspace spanned by $A$.
We can derive the set of all combinations of $A$ by using **all** the real numbers
as coefficients.

It is possible to check if a given a vector is in the linear hull of a vector using algorithms.

In order to represent a linear hull with fewer parameters, 
we need to define a dependency relation for linear combinations.

The idea is to define a vector as a linear combination of previous vectors.
For example, for the set $A = \{a_1, a_2, \dots, a_k \}$, if the vector
$a_k$ can be defined as the linear combination of vectors in $A - a_k$ 
then the linear hull of $A - a_k$ would be equal to linear hull of $A$.

A linear dependence relation is an expression. The expression must express a
zero vector. It must consist of a combination of vectors in a set. The vectors
must also have at least on nonzero coefficient: $-a_{1} - a_{2} + a_{3} = 0$
where $a_1 = [1, 0]$, $a_2 = [0, 1]$, $a_3 = [0, -1.2]$

When a vector can be expressed as a linear combination of the other vectors
in the same set, the set of vectors is said to be **linearly dependent**.
When this is not possible, the set of vectors is said to be **linearly independent**.

For a given a set of vector $\Gamma = \{g_1, g_2, \dots, g_m\} \subset R^n$ a subset $\Delta$
is **maximally linearly independent subset** of $\Gamma$ if it satisfies the following
conditions:

- $\Delta$ must be linearly indepedent
- including any other vector from $\Gamma - \Delta$ to $\Delta$ would make 
$\Delta$ linearly dependent.

All maximal linearly independent subsets of $\Gamma$ always contain
the same number of vectors in them (why ??), and this number is called
the **rank of $\Gamma$**.
This leads to the following curious fact, the linear hull of $\Gamma$ is equal to
the linear hull of any of its maximal linearly independent subsets, since all
the vectors that does not belong to the maximal linearly independent subset can 
be expressed as linear combinations of vectors inside the maximal linearly
independent subset.

## Representing the Linear Hull of a set of Vectors By a System of Homogeneous Linear Equations

Set of vector $\Gamma = \{g_1, g_2, \dots, g_m\} \subset R^n$ L = is all
combinations of vectors of $\Gamma$, meaning $\Gamma$'s linear hull. The most
compact representation of a linear hull can be achieved using a matrix. This
matrix, say B, has a specific order/size. This representation has the form $L
= {x: B \times x = 0}$. The order of the matrix B is SxN. S is the number of
linearly independent equations in $B \times x$. N is the number of dimensions
of the vector. S can also be found using the rank of $\Gamma$. $S = N -
\rank(\Gamma)$ that is S is equal to the difference between the number of
dimensions of the vector and the rank of $\Gamma$.

## Row space, Column Space and Null Space of a Matrix

Given matrix A of order MxN:
its set of row vectors is ${A_{1,:}, A_{2,:}, \dots, A_{M, :}}$
its set of column vectors is ${A_{:,1}, A_{:,2}, \dots, A_{:, N}}$

The row space of the matrix A is the linear hull of its row vectors.
The column space of the matrix A is the linear hull of its column vectors.

A point in row space can be represented using the notation we had seen
previously in the definition of N dimensional points, that is a general point
in the row space of A is ${a_1 A_{1, :}, a_2 A_{2, :}, \dots, a_M A_{M, :}}$
where ${a \in R^M}$, meaning that $a_1, a_2, \dots a_M$ can be any real
number.

A point in column space can be represented using the notation we had seen
previously in the definition of N dimensional points, that is a general point
in the column space of A is ${b_1 A_{:, 1}, b_2 A_{:, 2}, \dots, b_N A_{:,
N}}$ where ${b \in R^N}$, meaning that $b_1, b_2, \dots b_N$ can be any real
number. The column space is also called, range of A, or range space of A, or
image space of A. It can be denoted as R(A)

Null space of A is the set of all solutions of the homogeneous system of
linear equations where $A \times x = 0$. It is denoted by $N(A)$. In the
equation $A \times x = 0$ the x is $x \in R^N$, it thus denotes a column
vector since the order of A is MxN.

## Affine Combinations, Affine Hulls or spans, Affinespaces

For a given a set of vector $\Gamma = \{g_1, g_2, \dots, g_m\}$ 
where $g_i \in R^n$.

The affine combination of $\Gamma$ is a **vector** of the form $b_1 g_1 + b_2
g_2 + \dots + b_m g_m$ where $b_i \in R$ and $b_1 + b_2 + \dots + b_m = 1$

In the equation of the combination the $b = {b_1, \dots, b_m}$ is the set of
**coefficients**. The $b_1 + b_2 + \dots + b_m = 1$ is called the **affine
combination condition**.

Now let's take a closer look at the affine combination of $\Gamma$:

$b_1 g_1 + b_2 g_2 + \dots + b_m g_m 
    = (1 - b_2 - \dots - b_m)g_1 + b_2 g_2 + \dots + b_m g_m 
    = [1 g_1 - b_2 g_1 - \dots - b_m g_1] + b_2 g_2 + \dots + b_m g_m 
    = 1 g_1 - b_2 g_1 - \dots - b_m g_1 + b_2 g_2 + \dots + b_m g_m 
    = g_1 + b_2 g_2 - b_2 g_1  + \dots + b_m g_m - b_m g_1
    = g_1 + b_2 (g_2 - g_1)  + \dots + b_m (g_m - g_1)$

At this stage, there are multiple things to note:
The $[b_2(g_2 - g_1) + b_3(g_3 - g_1) + \dots + b_m (g_m - g_1)]$ in the
equation is a linear combination of $y = {(g_2 - g_1), (g_3 - g_1), \dots, (g_m -
g_1)}$. Why ? Because it satisfies two conditions of being a linear
combination:

- its coefficients: $[b_2, b_3, \dots, b_m]$ can take any real number.
- its vectors: ${(g_2 - g_1), (g_3 - g_1), \dots, (g_m - g_1)}$ are all in
  $R^n$

Thus the affine hull (also called affine space, or affine span) of $\Gamma$ is
simply the linear hull of $y$ translated by $g_1$, that is the affine hull of
$\Gamma$ is equal to $g_1 + linear hull of y$.

The straight line joining two points in $R^n$ is the affine hull of those two
points. Why ? 
The parametric representation of a line in $R^n$:
$ x = a + \alpha b$ where $a, b \in R^n$ and $b \not = 0$
A line joining two points in $R^n$ is $x = a + \alpha (b - a)$ where $b$ and
$a$ are the points that are being joined by the line and $\alpha \in R$.
real values. Let's expand that expression:
$$ a + \alpha (b - a) = a + \alpha b - \alpha a
                      = a - \alpha a + \alpha b
                      = a (1 - \alpha) + \alpha b$$

Notice that coefficients of this expression satisfies the affine combination
condition. Thus a straight line joining the two points $a$ and $b$ is an
affine hull, or affine space of the two points.

## How to check if a_{k+1} is in the affine hull of {a_1, \dots, a_k}

The vector $a_{k+1}$ is in the affine hull of vectors ${a_1, a_2, \dots, a_k}$
where $a_i \in R^n$ if and only if it satisfies the following conditions:

- $b_1 a_1 + b_2 a_2 + \dots + b_k a_k = a_{k+1}$ 
- $b_1 + b_2 + \dots + b_k = 1$

If this system of equations has a solution, then the vector $a_{k+1}$ is in
the affine hull of vectors ${a_1, a_2, \dots, a_k}$.

## Two ways of representing the affine hull of given set of vectors

Given a vector set $\Gamma = {a_1, a_2, \dots, a_k}$ where $a_i \in R^n$, let
F denote the affine hull of $\Gamma$.

There are two ways to represent F:

1. Using a linear hull.

We know that $F = a_1 + L$, that is, F is a linear hull L translated by vector
$a_1$. L must be of form $b_2 (a_2 - a_1)  + \dots + b_k (a_k - a_1)$ where
$b_i \in R$ and $(a_i - a_1) \in R^n$. This comes from the definition of being
a linear hull. Let $\Delta = {(a_2 - a_1), (a_3 - a_1), \dots, (a_k - a_1)}$.
Let $\Alpha$ denote a maximally linearly independent subset of $\Delta$. The
number of elements of $\Beta$ is the rank $r$ of $\Delta$.
Given all of these L can be defined as:
$L = {c_1 \beta_1 + c_2 \beta_2 + \dots + c_r \beta_r}$ where $\beta_i \in
\Beta$ and $c_i \in R$. 
$F = a_1 + {c_1 \beta_1 + c_2 \beta_2 + \dots + c_r \beta_r}$


1. Using a matrix.

We know that $F = a_1 + L$. L can be represented with a system of homogeneous
equations: $L = {x: A x = 0} where $A$ is a matrix. $A$ has the following
dimensions: MxN where N is the number of dimensions of the vector, and M is
the number of linearly independent equations in the system. The thing is if we
multiply the matrix A with any vector belonging to L, we would get 0, so if we
multiply the matrix A with a vector not belonging to L but belonging to the
vector space $\Gamma$, we should get a value that is not 0. There is a vector
that satisfies this condition $a_1$. Hence we can say $A a_1 = b$ where $b
\not = \{b_1 = 0, b_2 = 0, \dots b_n = 0\}$. Then we can represent 
$F = \{x: A x = b \}$.


## Convex Combinations and Convex Hulls

Let $a_1, a_2 \in R^n$. A **convex combination** of these points is: $b_1 a_1 +
b_2 a_2$ where $b_1, b_2$ satisfy **convex combination conditions**:

- $b_1 + b_2 = 1$
- $b_1, b_2 ≥ 0$

The **convex hull** of $a_1$ and $a_2$ is the set of all convex combinations of
them. The parametric representation of the convex hull of two points in $R^n$
has the form:

- $\{ x = b_1 a_1 + b_2 a_2: b_1 + b_2 = 1, b_1, b_2 ≥ 0\}$.

Notice that this corresponds to the line segment between $a_1$ and $a_2$.
Remember the parametric representation of the line segment was:

\{ x = a + \alpha b : \beta \le \alpha \le \theta \}

This can be seen, if we change the representation of the convex hull slightly:
$x = b_1 a_1 + b_2 a_2 = b_1 a_1 + (1 - b_1) a_2$ This last equation can be
seen as: $x = b_1 a_1 + (1 - b_1) a_2: 0 \le b_1 \le 1$. Notice that this is
same as the line segment equation where the $\beta = 0$ and $\theta = 1$ 

## Midpoint, and the Two Orientations of a Line Segment

Let $a_1, a_2 \in R^n$ the midpoint of the line segment joining $a_1$ and
$a_2$ can be expressed in the convex hull form as $\frac{1}{2}a_1 +
\frac{1}{2}a_2$ regardless of the orientation of the line segment. 
There are two possible orientations for a line segment joining two points (Why
? How do we define orientation ?).
Two orientations of the line segment can be specified by the convex hull as:

- $\{ a_1 + b (a_2 - a_1): 0 \le b \le 1 \}$
- $\{ a_2 + b (a_1 - a_2): 0 \le b \le 1 \}$

Notice that the first one indicates that the line segment starts from $a_1$
and includes points along the way to $a_2$.
The second one indicates that the line segment starts from $a_2$ and includes
points along the way to $a_1$.

## Convex Combinations of a General set of k Points

The convex combination condition of $k$ set of points in $R^n$ where k is
finite, is the same two points, that is:

- $b_1 + b_2 + \dots + b_k = 1$
- $\{b_1, b_2, \dots b_i, \dots b_k: b_i ≥ 0\}$

Convex hull of $k$ set of points is also known as **convex polytope** or a
**bounded convex polyhedron**.
The convex hull of $k$ set of points in $R^n$ is a subset of its affine hull.
The affine hull of $k$ set of points in $R^n$ is a subset of its linear hull.

Notice that these can be distinguished by the constraints on their
coefficients.

## How to Check if $a_{k+1} is in the Convex Hull of $\{a_1, \dots, a_k\}$ ?

Basically the convex hull of $\Kappa = \{a_1, a_2, \dots, a_k\}$ where $\Kappa
\subset R^n$ is ${b_1 a_1 + b_2 a_2 + \dots + b_k a_k = x: b_1 + \dots b_k = 1
and b_1, b_2, \dots b_i, \dots b_k ≥ 0}, so in order for $a_{k+1}$ to be in the
convex hull of $\Kappa$ it needs to satisfy the following system of equations:

- $b_1 a_1 + b_2 a_2 + \dots + b_k a_k = a_{k+1}$ 
- $b_1 + \dots b_k = 1$
- $b_1, b_2, \dots b_i, \dots b_k ≥ 0$

That is these equations need to have feasible solution with parameters $b_1,
b_2, \dots, b_k$. This can be solved with linear programming.

## Nonnegative Combinations, Pos Cones

It might be interesting to notice that changing the constraints on the vector
set results also in change of shape of the hull. For example, if we drop the
summation constraint on coefficients of the convex hull, we obtain a set of
nonnegative combinations or a Pos Cone which, as the name suggests, is a cone.
Formally a Pos Cone is defined as:

Let $\Kappa = \{a_1, a_2, \dots, a_k\}$ where $\Kappa \subset R^n$,
Pos Cone of $\Kappa$ is 
$Pos(\Kappa)=\{ b_1 a_1 + b_2 a_2 + \dots + b_k a_k = x : b_1, b_2, \dots b_k ≥ 0\}

Same as before this can be solved with linear programming techniques.

p. 335 Hyperplanes
