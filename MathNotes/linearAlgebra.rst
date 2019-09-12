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

- let 
  :math:`P = (p_1, p_2, ..., p_n)`
  :math:`Q = (q_1, q_2, ..., q_n)`
  :math:`C = (c_1, c_2, ..., c_n)`
  :math:`D = (d_1, d_2, ..., d_n)`
  :math:`O = (0, 0, ..., 0)`
  :math:`\vec{PQ} = (P, Q)`
  :math:`\vec{CD} = (C, D)`
  :math:`\vec{PQ} = \vec{(Q-P)O}`
  :math:`\vec{CD} = \vec{(D-C)O}`
  :math:`A = \vec{(Q-P)O} = ((Q-P), O)`
  :math:`B = \vec{(D-C)O} = ((D-C), O)`
  :math:`A = ((q_1 - p_1, q_2 - p_2, ..., q_n - p_n), (0, 0, ..., 0))`
  :math:`B = ((d_1 - c_1, d_2 - c_2, ..., d_n - c_n), (0, 0, ..., 0))`
  :math:`A = ((a_1, a_2, ..., a_n), (0, 0, ..., 0))`
  :math:`B = ((b_1, b_2, ..., b_n), (0, 0, ..., 0))`
  :math:`A \dot B = ((Q - P) \dot (D - C), 0 \dot 0)`
  :math:`A \dot B = (a_1{\times}b_1 + a_2{\times}b_2 + ... + a_n{\times}b_n, 0)`
  :math:`s` is a number
  :math:`A \dot B = (s, 0) = \vec{PQ} \dot \vec{CD}`


Norm of a Vector
----------------

Norm is the magnitude of a vector. It is denoted by :math:`||A||` for a vector
A. The operation is defined as :math:`||A|| = \sqrt{a^2 + b^2}` if :math:`A =
(a, b)`. If :math:`A = (a,b,c)` then :math:`\sqrt{a^2, b^2, c^2}`.
For a vector that is n dimensional, that is, :math:`A = (a_1, a_2, ..., a_n)`

For any vector :math:`||A|| = ||-A||`, since their direction does not relate
to their magnitude.

The distance between points A, B is defined as :math:`||A-B|| =
\sqrt{(A-B){\dot}(A-B)}`. Naturally :math:`||A-B||=||B-A||`

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

From here we can derive the concept of unit vector, which is :math:`A / ||A||`
for a given vector. We simply divide the vector to its magnitude.
Again we can say that :math:`A` and :math:`B` have the same direction if
:math:`B = A \times k`, *k* being a scalar that satisfies :math:`k>0`


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
