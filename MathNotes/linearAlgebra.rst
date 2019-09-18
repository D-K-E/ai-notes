###############
Linear Algebra
###############

Notes from 
Lang, Serge, Introduction to Linear Algebra, Undergraduate Texts In Mathematics, Second Edition (New Haven, 1986)
Kuttler, Kenneth, Elementary Linear Algebra, Lecture Notes, 2016
and other various sources

Vectors
========

Points
-------

Let's say we have 1 dimensional space, we denote a point on that space with a
number, k.

Let's say we have 2 dimensional space, we denote a point on that space with
two numbers, :math:`k_1, k_2`.

Let's say we have n dimensional space, we denote a point on that space with n
numbers, :math:`k_1, k_2, ... , k_n`

These numbers are called **coordinates**.

The spaces are denoted by :math:`R^1` for 1 dimensional space, :math:`R^n` for
n dimensional space, n being a positive integer.

Operations on Points
*********************

Addition: 

Rule:

- let 
  :math:`A = (a_1, a_2, a_n)`  
  :math:`B = (b_1, b_2, b_n)`

- :math:`A + B = (a_1 + b_1, a_2 + b_2, a_n + b_n)`

Example:

- :math:`A = (7,5,3,8,12)` and :math:`B = (2,-3,-1,7,-10)`
- :math:`A + B = (7+2, 5+(-3), 3+(-1), 8+7, 12+(-10))`
- :math:`A + B = (9, 2, 2, 15, 2)`

Properties:

- :math:`(A+B) + C = A + (B+C)`
- :math:`A+B = B + A`
- if :math:`O = (0,0, ..., 0)` 
  then :math:`A + O = O + A = A`
  else :math:`A + O' = A'` for all :math:`A != O`
- if :math:`A = (a_1, a_2, ..., a_n) and -A = (-a_1, -a_2, ..., -a_n)`
  then :math:`A + (-A) = O`

Multiplication by scalar:

Scalar means simply that which scales. It increases or decreases the scale of
something. In this case it increase/decrease the scale/magnitude of the vector
that goes from origin, point O defined in the properties above, and the some
point in :math:`R^n`

Rule:

- Let :math:`A = (a_1, a_2, ..., a_n)` c is a number

- :math:`A \times c = (a_1 \times c, a_2 \times c, ..., a_n \times c)`


Example:

- Let :math:`A = (7,5,3,8,12)`
  :math:`c = 2`

- :math:`A \times c = (7 \times c, 5 \times c, 3 \times c, 8 \times c, 12 \times c)`
- :math:`A \times 2 = (7 \times 2, 5 \times 2, 3 \times 2, 8 \times 2, 12 \times 2)`
- :math:`A \times 2 = (14, 10, 9, 16, 24)`

Properties:

- :math:`(A + B) \times c = c \times A + c \times B`
- :math:`(c_1 + c_2) \times A = c_1 \times A + c_2 \times A` if :math:`c_1`
  and :math:`c_2` are numbers
- :math:`(c_1 \times c_2) \times A = c_1 \times (c_2 \times A)`

- let :math:`A \times c = B`
  - if :math:`c > 0`
    then A and B have the same direction with respect to origin
    else if :math:`c < 0`
    then A and B have the opposite direction with respect to origin
    else B is origin

Scalar/Dot Product:

Rule:

- let
  :math:`A = (a_1, a_2, ..., a_n)`
  :math:`B = (b_1, b_2, ..., b_n)`
  :math:`A \dot B = (a_1{\times}b_1 + a_2{\times}b_2 + ... + a_n{\times}b_n)`
  :math:`s` is a number
  :math:`A \dot B = s`

Properties:

1. :math:`A \dot B = B \dot A`

2. :math:`A \dot (B + C) = A \dot B + A \dot C = (B + C) \dot A`

3. let :math:`x` be a number
   - :math:`(x \times A) \dot B = x \times (A \dot B)`
   - :math:`A \dot (x \times B) = x \times (A \dot B)`

4. if :math:`A = (0, 0, ..., 0)`
   then :math:`A \dot A = 0`
   else :math:`A \dot A > 0`

5. if 
     :math:`A \not = (0, 0, ..., 0)` and 
     :math:`B \not = (0, 0, ..., 0)` and
     :math:`A \dot B = 0`
   then A is perpendicular/orthogonal to B


Located Vectors
---------------

A located vector is an ordered pair of points:

- let 
  :math:`A = (a_1, a_2, a_n)`  
  :math:`B = (b_1, b_2, b_n)`

- :math:`\vec{AB} = (A, B)`
  where:
    - A is the beginning point and
    - B is the end point
    - The vector is located at A.

Properties:

- let
  :math:`\vec{AB}`
  :math:`\vec{CD}`

- if :math:`B - A = D - C`
  then :math:`\vec{AB} = \vec{CD}`

It reads as follows if the difference between the end point and the beginning
point are the same for two located vectors, they are considered equal.

- Thus :math:`\forall \vec{AB}`, :math:`\exists \vec{O(B-A)} = \vec{AB}`

It reads as follows for all vectors, with beginning point A, and end point B
there exists an equivalent vector that has the beginning point as the origin 
and the end point as the difference between A and B.

Example:

- let
  :math:`J = ( 1,4,7,3,4)`
  :math:`L = ( 6,2,2,4,5)`
  :math:`D = ( 2,1,1,5,3)`
  :math:`T = ( 7,-1,-4,6,4)`

- :math:`L - J = T - D`
- :math:`(6-1, 2-4, 2-7, 4-3, 5-1) = (7-2, -1-1, -4-1, 6-5, 4-3)`
- :math:`(5, -2, -5, 1, 4) = (5, -2, -5, 1, 4)`
- :math:`\therefore`
- :math:`\vec{JL} = \vec{DT}`

- let
  :math:`\vec{AB}`
  :math:`\vec{DE}`
  :math:`s \not = 0`

- if :math:`B - A = s \times (E - D)`
  then :math:`\vec{AB}` and :math:`\vec{DE}` are *parallel*

- if :math:`s > 0` in :math:`B - A = s \times (E - D)`
  then :math:`\vec{AB}` and :math:`\vec{DE}` have the *same direction*

- if :math:`s < 0` in :math:`B - A = s \times (E - D)`
  then :math:`\vec{AB}` and :math:`\vec{DE}` have the *opposite direction*

Example:

- let 
  :math:`A = (3, 2)`
  :math:`B = (4, 8)`
  :math:`C = (6, 4)`
  :math:`D = (9, 22)`
  :math:`\vec{AB}`
  :math:`\vec{CD}`

- then
  :math:`B - A = (1, 6)`
  :math:`D - C = (3, 18)`
  :math:`D - C = 3 ( B - A )`
  :math:`\therefore`
  :math:`\vec{AB}` and :math:`\vec{CD}` are *parallel*
    and have the *same direction*

Operations on Located Vectors
******************************

Addition:

See section Addition for Points

Subtraction:

See section Subtraction for Points

Scalar/Dot Product

See section Scalar/Dot Product for Points

Rule:

- :math:`P = (p_1, p_2, ..., p_n)`
- :math:`Q = (q_1, q_2, ..., q_n)`
- :math:`C = (c_1, c_2, ..., c_n)`
- :math:`D = (d_1, d_2, ..., d_n)`
- :math:`O = (0, 0, ..., 0)`
- :math:`\vec{PQ} = (P, Q)`
- :math:`\vec{CD} = (C, D)`
- :math:`\vec{PQ} = \vec{(Q-P)O}`
- :math:`\vec{CD} = \vec{(D-C)O}`
- :math:`A = \vec{(Q-P)O} = ((Q-P), O)`
- :math:`B = \vec{(D-C)O} = ((D-C), O)`
- :math:`A = ((q_1 - p_1, q_2 - p_2, ..., q_n - p_n), (0, 0, ..., 0))`
- :math:`B = ((d_1 - c_1, d_2 - c_2, ..., d_n - c_n), (0, 0, ..., 0))`
- :math:`A = ((a_1, a_2, ..., a_n), (0, 0, ..., 0))`
- :math:`B = ((b_1, b_2, ..., b_n), (0, 0, ..., 0))`
- :math:`A \dot B = ((Q - P) \dot (D - C), 0 \dot 0)`
- :math:`A \dot B = (a_1{\times}b_1 + a_2{\times}b_2 + ... + a_n{\times}b_n, 0)`
- :math:`s` is a number
- :math:`A \dot B = (s, 0) = \vec{PQ} \dot \vec{CD}`


Norm of a Vector
----------------

Norm is the magnitude of a vector. It is denoted by :math:`||A||` for a vector
A. The operation is defined as :math:`||A|| = \sqrt{a^2 + b^2}` if :math:`A =
(a, b)`. If :math:`A = (a,b,c)` then :math:`\sqrt{a^2, b^2, c^2}`.
For a vector that is n dimensional, that is, :math:`A = (a_1, a_2, ..., a_n)`

For any vector :math:`||A|| = ||-A||`, since their direction does not relate
to their magnitude.

The distance between points A, B is defined as :math:`||A-B|| =
\sqrt{(A-B){\cdot}(A-B)}`. Naturally :math:`||A-B||=||B-A||`

We can also define circles and disks this way for 2d.

Let *a* be number that is :math:`a > 0`, and let *P* be a point in a plane
xy. The collection of points *S* is called

- **open disc** of radius *a* centered at *P* if :math:`||S-P|| < a`
- **closed disc** of radius *a* centered at *P* if :math:`||S-P|| ≤ a`
- **circle** of radius *a* centered at *P* if :math:`||S-P|| = a`

We can define balls and spheres using the same definitions for 3d:
Let *a* be number that is :math:`a > 0`, and let *P* be a point in a plane
xyz.

The collection of points *S* is called:

- **open ball** of radius *a* centered at *P* if :math:`||S-P|| < a`
- **close ball** of radius *a* centered at *P* if :math:`||S-P|| ≤ a`
- **sphere** of radius *a* centered at *P* if :math:`||S-P|| = a`

Notice that the sphere is the shell that contains the open ball. Their union
is the close ball.

From here we can derive the concept of **unit vector**, which is :math:`A / ||A||`
for a given vector. We simply divide the vector to its magnitude.
Again we can say that :math:`A` and :math:`B` have the same direction if
:math:`B = A \times k`, *k* being a scalar that satisfies :math:`k>0`

Having two perpendicular vectors, that is, :math:`A \cdot B = 0`, has
an interesting correlation, :math:`||A-B|| = ||A+B||`.
Why ?

.. math::
   
   ||A+B||^2 = ||A-B||^2
    A^2 + 2A \cdot B + B^2 = A^2 - 2A \cdot B + B^2
    4A \cdot B = 0
    A \cdot B = 0

Another thing that is interesting is that having a general Pythagoras theorem:

.. math::

    ||A+B||^2 = ||A||^2 + ||B||^2
    (A+B) \cdot (A+B) = ||A||^2 + ||B||^2
    A^2 + 2A \cdot B + B^2 = ||A||^2 + ||B||^2
    A \cdot B = 0
    \therefore
    A^2 + B^2 = ||A||^2 + ||B||^2


These properties of perpendicular vectors permit us to define the notion of
**projection** one vector to another.

Now let's say we have vectors *A* and *B* that intersect at point *K*. Suppose
that they are in the same direction. Imagine a vector that descends from the
tip of *A*, point *T*, to *B* that is perpendicular to B and that crosses *B*
at point *P*.
What we have is a *KTP* triangle. Since *P* lies on *B* we can substitute it
as :math:`cB = P`. Suppose we have another vector *G* that is perpendicular to
*B*, but passes through point *K*. The point :math:`T - P` would
necessarily lie on vector *G*, since *T* is at the tip of *A*, we can also
write :math:`A-P`.
Now the perpendicularity ensures the following :math:`(A - cB) \cdot B = 0`
This means that :math:`c = \frac{A \cdot B}{B \cdot B}`

*c* is called the **component** of *A* along *B*.
The **projection** of *A* along *B* is 
:math:`cB = \frac{A \cdot B}{B \cdot B} \cdot B`

Now our triangle *KTP* reveals other interesting properties. For example,
the cosinus of the angle between *KT* and *KP*, theta, is 
:math:`\cos{\theta} = \frac{KP}{KT}` in other words
:math:`\cos{\theta} = \frac{||cB||}{||A||}`

Now if we substitute the component formula to here:

- :math:`\cos{\theta} = \frac{A \cdot B}{B \cdot B} \times \frac{||B||}{||A||}`
- :math:`\cos{\theta} = \frac{A \cdot B}{||B||^2} \times \frac{||B||}{||A||}`
- :math:`\cos{\theta} = \frac{A \cdot B}{||B|| \times ||A||}`

And also :math:`\cos{\theta} \times ||B|| \times ||A|| = A \cdot B`

Parametric Lines
=================

They are lines which take their slope from a vector and their intercept from a
point.

- Let *A* be a vector that is not equal to origin

- Let *P* be a point that is not on *A*.

- The equation of the line passing through *P* in the direction of *A* is 
  :math:`X = P + t \times A` where *t* is all the available numbers in the
  field, and *X* is the output tuple for the points on line

Planes
=======

Planes are quite simple to describe once we acquire the notion of
perpendicularity.

- Let *P* be a point in 3d space.
- Let *X* be another point in 3d space
- Let *N* be a vector that is perpendicular to the segment :math:`X - P`

- Then a plane *T* is the collection of all points that satisfy the following
  equation :math:`(X-P) \cdot N = 0`, where *P* and *N* are known.

N is also called **normal**.

Two planes are perpendicular if their normals are perpendicular.
Two planes are parallel if their normals are parallel.
The angle between two planes are defined by the angle between their normal
vectors

The distance between a plane and an arbitrary point *Q* works out as the
following.
I have a point *Q*, a point *L* which is on the plane and a point *P* which is
another known point on plane. We suppose that the segment *QL* is
perpendicular to the plane. Thus we are looking for its norm.

This situation is very similar to where we observed components and projections
of vectors to one another. Here we have a *QLP* triangle. The direction of the
segment *QL* is the same as normal of the plane.  First we need to find the
component of *QP* along *QL*. The norm of the component times *QL* is the
value we are searching for.


Eigenvalues & Eigenvectors
===========================

Linear Transformations
=======================

Vectors: `Matrix with a one column/row. That which has a direction and a
magnitude.`

A matrix can transform the magnitude and the direction of a vector.

:math:`A = [[a,b],[c,d]]`

:math:`B = [[3,-2],[1,0]]`

The matrix B transforms the vector :math:`[1,0]^T` to :math:`[3,1]^T`
Why ? Let's multiply the vector with the matrix B

:math:`[[3x1 + -2x0],[ 1x1 + 0x0]]`
:math:`[[3+0],[1+0]]`
:math:`[3,1]^T`

So our first vector was changed both in terms of magnitude (it is longer now),
and in direction (it points to a different direction now)

Coordinate Isomorphism
-----------------------

Let *V* be a vector space over a field *R*, and 
:math:`B = \{b_1, b_2, ..., b_{n}  \}` is a basis of *V*.
If *v* is a vector in *V*, then 
:math:`v = b_1 {\cdot} x_1, b_2 {\cdot} x_2, ..., b_n {\cdot} x_{n}`.
:math:`X = \{ x_1, x_2, ..., x_{n} \}` is called the coordinate tuple of *v*
relative to *B*. 
Since :math:`X^{T} \in R^{n}`, let :math:`f: R^{n} \to V` 
where :math:`f(o_{i}) = b_i` for :math:`i = 1, ..., n`. The function *f* is
called a coordinate isomorphism for *V* and basis *B*. The :math:`o_i` denotes
the n tuple with element i equal to 1 and all the others equal to 0.


Change of Basis of a Vector using Linear Transformations
---------------------------------------------------------

The case is more or less that of a manifold.
Suppose :math:`A = \{a_1, a_2, ..., a_{n}  \}` and 
:math:`B = \{b_1, b_2, ..., b_{n}  \}` are basis of a vector space *V* over a
field *K*. Let :math:`f_{A}` and :math:`f_{B}` be two coordinate isomorphisms
that map coordinate tuples to vector v: :math:`f_{A}(X) = v` and
:math:`f_{B}(Y) = v` with *X* being coordinate tuple of *v* relative to *A*,
and *Y* being coordinate tuple of *v* relative to *B*.
The map :math:`f_{B}^{-1}(f_{A}(X))` maps a coordinate tuple relative to *A*
to a coordinate tuple relative to *B*.


Eigenvectors & Eigenvalues
===========================

Eigenvectors are special vectors that have the following property.
When we multiply an eigenvector with a linear transformation matrix, the
result is a scalar multiplication of the eigenvector itself.

Eigenvalue is simply the scalar multiple of the eigenvector when a linear
transformation is applied

For example:
:math:`A = [[3,1], [-2,0]]`
:math:`v = [2,1]^T`

The linear transformation, multiplication of v with matrix A, is equal to
:math:`[4,2]^T`.

Now the vector :math:`[4,2]` is simply v times 2.
2 is eigenvalue. So basically the transformation did not change the direction
of the vector but it changed the magnitude of the vector

When dealing with calculating eigenvectors and eigenvalues we start from
finding out an eigenvalue first.

Formally, eigenvectors are defined as follows:
:math:`A \dot v = \lambda \dot v`

- A is the transformation matrix
- v is the eigenvector
- lambda is the eigenvalue
