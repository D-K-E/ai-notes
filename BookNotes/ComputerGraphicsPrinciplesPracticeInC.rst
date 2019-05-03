############################################
Computer Graphics: Principles and Practice
############################################

Notes from:

Foley, James, Andries van Dam, Steven Feiner, and John Hughes, Computer Graphics: Principles and Practice, 2nd edn (Paris, 1996)

and from:

Foley, James, Andries van Dam, Steven Feiner, and John Hughes, Computer Graphics: Principles and Practice, 2nd edn (Paris, 1996)

Chapter 7: Essential Mathematics
================================

**v**: vector v
**M**: matrice M
except:

**R**: is the set of Real numbers
**C**: is the set of complex numbers
:math:`\mathbf{R}^{+}`: is the set of positive real numbers
:math:`\mathbf{R}^{+}_0`: is the set of non negative real numbers
*i*: is the indexing variable as in :math:`x_i`

Cartesian product of two sets: A and B:

:math:`A x B = {(a,b) : a \in A, b \in B}`

.. code:: python
    
    def cartProd(A, B):
        "Cartesian product of two lists"
        result = []
        for a in A:
            for b in B:
                result.append((a,b))

:math:`[a, b] = {x : a ≤ x ≤ b}`

.. code:: python
    
    def checkClosedInt(x, lower_limit, upper_limit):
        "Check given number x is between lower and upper limit"
        return bool(x >= lower_limit and x <= upper_limit)

:math:`[a, b) = {x : a ≤ x < b}`
:math:`(a, b] = {x : a < x ≤ b}`

.. code:: python
    
    def checkHalfOpenIntUpper(x, lower_limit, upper_limit):
        "Check given number x is between lower and upper limit"
        return bool(x >= lower_limit and x < upper_limit)

    def checkHalfOpenIntLower(x, lower_limit, upper_limit):
        "Check given number x is between lower and upper limit"
        return bool(x > lower_limit and x <= upper_limit)

:math:`[a, ∞) = {x : a ≤ x}`
:math:`(−∞, b] = {x : x ≤ b}`

.. code:: python
    
    def checkLowerLimitWithInf(x, lower_limit):
        "Check given number x is between lower and upper limit"
        return bool(x >= lower_limit)

    def checkUpperLimitWithInf(x, upper_limit):
        "Check given number x is between lower and upper limit"
        return bool(x <= upper_limit)

Function:

:math:`f: \mathbf{R} \to \mathbf{R}: x \to x^2`. reads as:

.. code:: python
    
    def main(x: float) -> float:
        return x**2
        

Function f which maps a value x from **domain** R 
to a value :math:`x^2` from **codomain** R, 

Surjective function: a function that can map to an entire codomain.
Ex:

:math:`g: \mathbf{R} \to \mathbf{R}^{+}_0: x \to x^2`

The x is mapped to values that covers the entire codomain

g is surjective whereas f is not.

Injective function: a function which operates between a domain and a codomain
in which no two values of the domain can map to the same value of the
codomain, meaning that if h(a) = h(b) then a=b.
Ex:

:math:`h: \mathbf{R}^{+}_0 \to \mathbf{R}^{+}_0: x \to x^2`

An function that is both injective and surjective like h can have an inverse 
function h' which maps the codomain of h to domain of h. That is it simply 
undoes what h does.

A function that is both injective and sujective is called bijective.

.. code:: python
    
    def checkFuncAndInverse(fn, fnInv, domain: set, codomain: set):
        "Check if a given func and inverse is has injective properties"
        result = []
        for x in domain:
            res = fnInv(fn(x)) == x
            result.append(res)

        for y in codomain:
            res = fn(fnInv(y)) == y
            result.append(res)

        return all(result)


Coordinates
------------
Coordinates are real numbers that are associated to geometric entities, like
points, lines, etc.

Geometric properties do not change, whereas numerical properties can change
for example, a point lies on a line is a geometric property true irrespective
of the coordinate system.

Chapter 16: Illumination Models from Ed. 2
==========================================

Ambient Light
--------------

Simple illumination equation:
:math:`I = k_i`
where I is the resulting intensity, pixel value for example, and k is the 
coefficient of the intrinsic intensity of the object. Notice no direction of 
light source is included in this equation

Let's suppose another model, where there is an ambient light illuminating all
the objects at a constant intensity. This ambient light is a result of diffuse
where multiple surfaces reflect light, creating a non directional source of
light. This would transform our equation into:
:math:`I = I_a k_a`

where I is the resulting intensity, :math:`I_a` is the intensity of the
ambient light, and :math:`k_a` is the coefficient of ambient reflection, a
material property of the surface that reflects the ambient light

Diffuse reflection
--------------------

Ambient light considers that the object is illuminated uniformly from all
angles now let us consider illuminating objects from a point light source.
In the case of the latter, the object would be bright in one side but not
necessarily in another side. Its brightness would change according to the
position of the point light source.

Lambertian reflection
++++++++++++++++++++++

Diffuse reflection is a property where an object, with dull, matte surface,
like chalk is equally bright from all angles because they reflect the
light equally in all directions

A surface's brightness depends on the angle between two variables:
- L(ight source) direction. From where the light source is situated.
- N(ormal of the surface), a vector that is perpendicular to the surface.

This works as the following:

Let us suppose that the incoming ray of light has a very very small area such
as :math:`{\delta}A`.

If the incoming beam came from a direction that is perpendicular to the
surface, the area of the illumination would have been equal to 
:math:`{\delta}A`.

If we change the angle of approach of the incoming beam to an angle between 0
and 90. Than :math:`{\delta}A` would be a side of a right angled triangle
whose hypothenuse is the surface of the object. The angle :math:`{\theta}`
would then be the angle between the hypothenuse and the :math:`{\delta}A`

The cosine of the angle theta is directly proportional to the amount of light
reflected to the viewer, which is quite logical if you think. If you position
yourself right behind the light source, the brightest spot of the object would
right opposite to you.

The equation that models this relationship is:

:math:`I = I_p k_d \cos{\theta}`

where I_p is the light source intensity, k_d is the diffuse reflection
coefficient of the material between 0-1. The angle theta must be between 0 -
90.

This also means that we are treating the surfaces as self-occulding, that is
light cast from behind does not illuminate the front.
When we need to light objects that are not self-occulding, we use 
abs(cos theta) to invert their surface normals. Thus we treat them as if they
are being illuminated from both sides

If both N and L is normalized, we can rewrite the :math:`\cos{\theta}` as a
dot product of N and L.

The equation :math:`I = I_p {\times} k_d {\times} (N \cdot L)}`
tend to make objects look harsh.
So to obtain something more realistic, we add an ambient light:
:math:`I = I_a {\times} k_a + I_p {\times} k_d {\times} (N \cdot L)`

Light-source attenuation
-------------------------

The equation above won't distinguish where identical materials overlapping in
an image. In order to do that we need to add another factor to the equation.
The factor is called light source attenuation factor :math:`f_att`.
The equation thus becomes:
:math:`I = I_a {\times} k_a + f_{att} {\times} I_p {\times} k_d {\times} (N \cdot L)`

A useful example of :math:`f_att` is:
:math:`f_att = min(\frac{1}{c_1 + c_2{\times}d_L + c_3{\times}d^2_{L}} , 1)`

Where c1, c2, c3 are constants that are provided by the user, and the
:math:`d_L` is the distance between the light source and the illuminated
surface.

Colored light and surfaces
---------------------------

The colored light is treated by channel, that is we write an equation for each
component of the color model. We represent an object's *diffuse color* by
:math:`O_d`, O for object, d for diffuse. In order to mark the component
that is represented we add the its initial. Ex: :math:`O_{dR}` for red, G for
green, B, for blue in RGB color space. The above equation for red would be:
:math:`I_R = I_{aR}k_{a}O_{dR} + f_{att}I_{pR}k_{d}O_{dR} (N \cdot L)`

Rather than indicating the channel name in the equation for all the channels,
we simply use the variable :math:`\lambda` in order to replace the channel
initial, thus the equation becomes:
:math:`I_{\lambda} = I_{a\lambda}k_{a}O_{d\lambda} +
f_{att}I_{p\lambda}k_{d}O_{d\lambda} (N \cdot L)`

Specular Reflection
--------------------

Specular reflection is easy to observe. Illuminate an apple, you would see
that a particular part of the apple is highlighted with the color of the light
source. This is specular reflection, you change your viewpoint you should
notice that the highlighted area would also change.

Phong illumination model is explaining that:

.. math::
    
    I_{\lambda} = I_{a\lambda} k_a O_{d\lambda} 
                  + f_{att} I_{p\lambda} [k_{d}O_{d\lambda} (N \cdot L)
                                          + k_{s}O_{s\lambda} (R \cdot V)^n]

Where O_s is object's specular color, k_s and n are user provided.
I_{p\lambda} is the primary component of the light source
An alternative approach is to use the halfway vector H where:
:math:`H = ( L + V ) / (|L + V|)`. Computationally this is cheaper.
Because once we postulate that the viewer and the light source are at
infinity.
The H becomes a constant 1.
But the specular component n, does not produce the same result as before due
to the change in angles
