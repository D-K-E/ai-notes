#####################
Differential Calculus
#####################

Limits
=======

What are limits ?
-------------------

The limit of a function f(x) at some value a,
is simply what the value of the function approaches
when x gets close to a.
Most of the time x is equal to a.

However there are some functions whose limits are *messy*:

- Functions who are not defined at a. There is no value at a to talk of.
  (e.g. Rational functions with the denominator as zero at the point x=a.)
- Rational functions whose numerator or denominator appears to march off to
  infinity. Infinity allows no limit.
- Functions who vibrate violently around the value a. 
- Functions who approach different values when x gets close to a from the left
  side and the right side.


Differentials
==============

Slope of the line.
What is the difference between the current instant and the instant before


Formal definition:

- :math:`f^{(1)}(p) = \lim_{h \to 0} \frac{f(p+h) - f(p)}{h}`

Simple rules:

Power rule:
-----------
take the exponent, move it down,
multiply it with the coefficient—it’s the new coefficient.
Now take one from the exponent.
This is the new exponent.
(This is for integer coefficient polynomials, as you can see, this is painfully limited)
 2x^7, we have the derivative 14x^6.
Simple enough? Another exercise: x^2.
The derivative is 2x.

Addition and Subtraction
-------------------------

The derivative of f(x)+g(x) is just f'(x)+g'(x). That is we sum the derivatives.
Changing +g(x) to -g(x) gives us the subtraction rule.


Product Rule
-------------

The derivative of f(x)*G(x) is f'(x)*G(x)+f(x)*G'(x).
That is derivative of f(x) times the G(x) plus
f(x) times the derivative of G(x).

Here is its proof:

.. math::

    p(x) = f(x) \times g(x)
    p'(x) = \lim_{h \to 0} \frac{p(x+h) - p(x)}{h}
    v = f(x) \times g(x+h)
    p'(x) = \lim_{h \to 0} \frac{p(x+h) - p(x) + v - v}{h}
    p'(x) = \lim_{h \to 0} \frac{(f(x+h) \times g(x+h)) - (f(x) \times g(x)) + v - v}{h}
    p'(x) = \lim_{h \to 0} \frac{(f(x+h) \times g(x+h)) - v + v - (f(x) \times g(x))}{h}
    p'(x) = \lim_{h \to 0} \frac{(f(x+h) \times g(x+h)) - (f(x) \times g(x+h))  + v - (f(x) \times g(x))}{h}
    p'(x) = \lim_{h \to 0} \frac{ ((f(x+h) - f(x)) \times g(x+h)) + v - (f(x) \times g(x))}{h}
    p'(x) = \lim_{h \to 0} \frac{ ((f(x+h) - f(x)) \times g(x+h)) + f(x) \times (g(x+h) - g(x))}{h}
    p'(x) = f'(x) \times \lim_{h \to 0} g(x+h)) + f(x) \times g'(x)
    p'(x) = f'(x) \times g(x) + f(x) \times g'(x)

We replace :math:`\lim_{h \to 0} g(x+h)` with :math:`g(x)` since *h* is very
very close to 0, and we know that since *g(x)* is a differentiable function it
must be continuous.

Division Rule
--------------

The derivative of f(x)/g(x) is [f'(x)g(x)-f(x)g'(x)]/g(x)^2.
That is the derivative of f(x) times the g(x) minus
the f(x) times the derivative of g(x) divided by the g(x) squared

Chain Rule
-----------

The derivative of f(g(x)) is
f'(g(x))*g'(x).

Or, it is equal to the derivative of the outer function
evaluated at the inner functions times the derivative of the inner function.


Integrals
==========

The area under the curve.

Riemanns sum and Approximating a Definite Integral
---------------------------------------------------

The formula
:math:`Sum={{\sum}^{n}_{i=1} f(x^{*}_i)(x_i - x_{i-1}}`

Essentially it means, I am trying to sum infinitely small bits

Here is the catch:
I am trying to approximate the given area by using n number of rectangles.
Let's try to approximate the area under the function f(t) by using 5 rectangles

:math:`{\int}f(t)dt {\approx} height_1 {\times}width +h_2w +h_3w+h_4w+h_5w`

Instead of using 5 rectangles we can use n rectangles to have a better/worse
approximation

:math:`{\int}f(t)dt {\approx} height_1 {\times}width +h_2w +h_3w+h_4w+...+h_{n}w`

Let's factor out the width since its constant

$\int_i^a$

:math:`{\int}f(t)dt {\approx} w{\times}(h_1 +h_2 +h_3+h_4+...+h_{n})`

We can express the sum in the parantheses with the sum notation as well

:math:`{\int}f(t)dt {\approx} w{\times}{\sum^{n}_{i=1}}(h_i)`

The value of the width of the rectangle is simply the difference between range
of the function distributed to the range of the approximation.
So if the integral is taken over the interval [a,b] as in :math:`{\int}_{a}^{b}`
Then the width for the n number of rectangles would be:
:math:`width={\frac{b-a}{n}}`

$\sum_i$

The height is simply the according y value of the x with respect to f(x).
That is the meaning behind the notation :math:`f(x_{i}^{*}`

So final form of the equation is the following

.. math::

   `{\int}_{a}^{b}f(t)dt{\approx}{\frac{b-a}{n}}{\times}{\sum^{n}_{i=1}}f(x_{i}^{*}`

Now the part f(t) of the integral side should be rather obvious,
the height of the rectangle, and we have seen that the a and b are
related to the range of the function, then

what's up with dx ?
Well simply put dx is what happens when delta(x), that is x_i - x_{i-1}, approaches
to the 0. So dx is the difference when the difference between any point in x axes,
in the range of f(x) becomes very very very very very very very close to 0

Now, when you have a quantity whose value is virtually zero, there's not much
you can do with it. 2+dx is pretty much, well, 2. Or to take another example,
2/dx blows up to infinity.
But there are two circumstances under which terms involving dx can yield a
finite number. One is when you divide two differentials; for instance, 2dx/dx=2,
and dy/dx can be just about anything.


Line Integrals
---------------

Sum of infinitely small areas under the curve within the range of f(x,y) 

This is multivariate calclulus and it is a slight generalization of what we had
seen above in the definite integrals

Now a normal integral is:

- :math:`{\int}_{a}^{b}f(t)dt` where

  - :math:`\int` means sum
  - a is the lower range
  - b is the upper range
  - dt is the difference between t_i and t_{i-} when it is infinitely small

A line integral is:

- :math:`{\int}_{a}^{b}f(x,y)ds`

Now let's see how we arrive to this:

We have a function k(x) which is defined on a coordinate plane xy.
The function maps the value of x to a value of y in the coordinate plane

Now f(x,y) does the exact same thing in form. It takes the value of x and y
and maps it to another value in third dimension let's say z for example.
f(x,y) = z
This means that we have now a third dimension z, to which our function f(x,y)
maps to, so our plane now has three axis xyz

What about ds ? It is actually the same as saying dz, that is the difference
between z_i and z_{i-1} as it approaches to zero

How does all this relate to our k(x) ?

This is the tricky part

Now let's say c(x) = y and g(y) = x
then when x=t, y=c(t), and y=t, x=g(t)

So given that a <= t <= b
f(x,y) can be written as f(g(t), c(t))

So we can rewrite our line integral as follows:

- :math:`{\int}_{a}^{b}f(g(t),c(t))ds`

Now ds can actually be expressed in forms of dy and dx.
Because simply put infinitely small change in the curve k(x) is going to result from
infinitely small change in x direction and infinitely small change in y direction.
Notice that all three measures are distance measures.
Let's break it down this way:
dx = x_i - x_q
dy = k(x_i) - k(x_q)
ds = (x_i, k(x_i)) - (x_q, k(x_q))

Now the distance between two points are calculated with pythagoras theorem
:math:`\sqrt{a^2 + b^2}`

We plug in our points to pythagoras theorem

:math:`\sqrt{(x_i - x_q)^2 + (k(x_i) - k(x_q))^2}`

Based on the above mentioned equivalency this simply transforms to

:math:`\sqrt{(dx)^2 + (dy)^2}`
      
Then we can rewrite our line integral as follows

- :math:`{\int}_{a}^{b}f(g(t),c(t)){\times}{\sqrt{(dx)^2 + (dy)^2}}`

Now the problem is our point functions are all defined in t but our ds is expressed
in dx and dy, how do we transform it

Well let's suppose we multiplied the ds with dt/dt which 1, since we divide to equal
quantities, so:

- :math:`ds={\sqrt{(dx)^2 + (dy)^2}}{\times}{\frac{dt}{dt}}`

If we reformulate the expression a bit

- :math:`ds={\sqrt{ {\frac{1}{(dt)^2}} {\times} ((dx)^2 + (dy)^2) } }{\times}{{dt}}`

  - We simply put the 1/dt of the dt/dt expression inside the square root

I continue this line of progress and distribute the 1/dt over the variables

- :math:`ds={\sqrt{ {\frac{1}{(dt)^2}}{\times}(dx)^2 + {\frac{1}{(dt)^2}}{\times}(dy)^2) } }{\times}{{dt}}`

The expression inside can be simplified as the following:

- :math:`ds={\sqrt{ {\frac{(dx)^2}{(dt)^2}} + {\frac{(dy)^2}{(dt)^2}} } }{\times}{{dt}}`

And by using simple properties of multiplication on fractions we can have the following:

- :math:`ds={\sqrt{ ({\frac{dx}{dt}})^2 + ({\frac{dy}{dt}})^2 } }{\times}{{dt}}`

Now notice that dy/dt and dx/dt are actually derivatives of c(t) and g(t) respectively
that is they are c'(t)=dy/dt and g'(t)=dx/dt
So the final form of our equation would be:

- :math:`ds={\sqrt{ (g'(t))^2 + (c'(t))^2 } }{\times}{{dt}}`

