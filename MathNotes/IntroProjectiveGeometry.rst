#################################
Introduction Projective Geometry
#################################

Projective geometry models the imaging process of the camera, that is
transformation of that which is in 3d to 2d. This is an important capacity
because several set of properties which appear in Euclidean geometry, does not
apply to projective geometry. For example, in projective geometry, parallel
lines do coincide due to the perspective.
Projective transformations conserve type (points stay points, lines stay lines,
etc.), incidence (whether a point lies on a line or not), and a measure called
cross ratio

Geometry Relations

+----------------------------+-----------+------------+--------+------------+
|                            | Euclidean | similarity | affine | projective |
+----------------------------+-----------+------------+--------+------------+
| Transformations            |                                              |
+----------------------------+-----------+------------+--------+------------+
| rotation                   |    X      |   X        |   X    |     X      |
+----------------------------+-----------+------------+--------+------------+
| translation                |    X      |   X        |   X    |     X      |
+----------------------------+-----------+------------+--------+------------+
| uniform scaling            |           |   X        |   X    |     X      |
+----------------------------+-----------+------------+--------+------------+
| nonuniform scaling         |           |            |   X    |     X      |
+----------------------------+-----------+------------+--------+------------+
| shear                      |           |            |   X    |     X      |
+----------------------------+-----------+------------+--------+------------+
| perspective projection     |           |            |        |     X      |
+----------------------------+-----------+------------+--------+------------+
| composition of projections |           |            |        |     X      |
+----------------------------+-----------+------------+--------+------------+
| Invariants                 |                                              |
+----------------------------+-----------+------------+--------+------------+
| length                     |    X      |            |        |            |
+----------------------------+-----------+------------+--------+------------+
| angle                      |    X      |    X       |        |            |
+----------------------------+-----------+------------+--------+------------+
| ratio of lengths           |    X      |    X       |        |            |
+----------------------------+-----------+------------+--------+------------+
| parallelism                |    X      |    X       |   X    |            |
+----------------------------+-----------+------------+--------+------------+
| incidence                  |    X      |    X       |   X    |     X      |
+----------------------------+-----------+------------+--------+------------+
| cross ratio                |    X      |    X       |   X    |     X      |
+----------------------------+-----------+------------+--------+------------+


Concepts
=========


Homogenous coordinates
----------------------

We have a point (x,y) in euclidean plane. To represent this point in projective
plane, we simply add a third coordinate of 1 at the end: (x,y,1).
Scaling is unimportant, so the point (x, y, 1), also called the augmented vector.
This means we can simply represent a point in projective plane as (ax, ay, a). 
Since scaling is unimportant these points are called homogenous points.
"a" is the scale factor that can not be 0. In order to find the eucledean points
from projective points we simply divide the projective points to its last
dimension:

- Euclidean : (x,y) -> Projective (Gx,Gy, G) -> Euclidean (Gx/G, Gy/G, G/G)

When the last element G is 0, the homogeneous point is called, a point at infinity,
or ideal points. They do not have an equivalent representation in euclidean plane


How do we represent a line in Projective space:

- We start with the simple formula ax + by + c = 0
  - Essentially this is elementwise multiplication of 2 vectors 
    :math:`v_1= [x, y, 1]` and :math:`v_2=[a, b, c]`
  - Notice that the :math:`v_1` is the augmented vector and
    :math:`v_2` is simply another homogeneous point
  - So a line in 2d projective space is essentialy a multiplication of
    an augmented vector with a homogeneous point

We can see if two lines intersect on 2d projective plane,
by using cross product of two homogenous points

Ex.
:math:`v_1 = [2,6,2]` and :math:`v_2 = [4,8,5]`
they intersect at the point :math:`\tilde{x}`.
The condition is that dot product of :math:`\overline{x}` with :math:`v_1`
or with :math:`v_2` needs to be equal to 0.
Notice the relation between the augmented vector and the homogenous point
:math:`\overline{x} = {\frac{\tilde{x}}{w}}`

This is also same as :math:`v_1 {\times} v_2 = \tilde{x}`
