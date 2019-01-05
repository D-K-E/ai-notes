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
