################
Computer Vision 
################


What do we do in computer vision?
Extract information from the images

- Classify objects
  + You might want to know that a particular object is present in the image or not.
- 3d reconstruction
  + You might attempt to construct the 3d version of an object with lots of 2d images from different perspectives of an object.
- Motion analysis
  + How things move over time


Image Formation
====================

There are several ways that an image might form.
The simplest camera, or an image creator, is pinhole camera, in which light in the environment, goes through a very small hole to project into a camera chip that sits somewhere in the background.
The projected objects fall inversely to the camera chip, or the projection plane.
The math that governs all this is pretty simple:
- X is the physical height of the object,
- x is the height of the projection
- z is the distance of the physical object to camera.
- f is the focal distance between the pinhole and the camera chip/ projection plane.

The ratio between the X and z is equal to the ratio between b and f.
That is :math:`{\frac{X}{z}}={\frac{b}{f}}`

This comes from the resulting triangles and the angle.
The triangle which has one side as f and the other side as b, has the same angle in the corner facing towards the b, in the triangle with X and z and the angle facing towards the X.
The bigger triangle is just a scaled up version of the smaller triangle.
Which gives an interesting equation:
:math:`x=X{\times}{\frac{f}{z}}`

Ex.

James is 10 meters away from the camera,
He is 2 meters tall
and the focal length is 10mm.
what is the length of the projection ?

x = 2000 * 10 / 10000
x = 2 mm

This called central law of perspective projection, that is, in any camera, project of the size of any object scales with distance

Images are not 1 dimensional things, they are 2 dimensional so, we need to figure something for y as well.

Fortunately, the perspective law of projection goes for both ways.

x = X * f / z
y = Y * f / z

How lens work ?
Well, the fundamental problem of pinhole cameras is of the following:

      --------------|
      --------------|  |
object --- light ------| projection plane
      --------------|  |
      --------------|

The whole is missing a lot of the lights coming from the object.

What lens do is to bend the light to make it fall to the projection plane, so:

      --------------|
      --------------/\---- |
object --- light --|  |--- | projection plane
      --------------\/---- |
      --------------|

The illustration doesn't reflect well the the bending, but lens do that as well.
The equation that governs the relationship with lens is:
:math:`{\frac{1}{f}}={\frac{1}{Z}} - {\frac{1}{z}}`

Z is the extrinsic distance, distance between lens and the object.
z is the intrinsic distance, distance between lens and the projection plane
f is the focal distance of the lens.
Before the focal distance was a constant due to the inflexible nature of the pinhole,
now it is calculated.



Object Recognition
--------------------

The key concept for the tasks of object recognition is invariance:
- Which means there are natural variations of the image that don't effect the nature of the object itself, you wish to be invariant to those natural variations

Here are the possible invariances:

- Scale

  + Object is bigger, closer to camera, thus looks larger, it doesn't change,
    what it is, so we should be oblivious to this change

- Illumination

  + The light source illuminating the object might switch places, so the shadows
    might change, we should be obliviuos to that either

- Rotation

  + Object might turn, who cares.

- Deformation

  + Object might change its shape, for example a box might be opened, or closed,
    it is still a box, so we should be oblivous to those changes.

- Occlusion

  + Object might be blocked by another object for example, i might place a chair in
    front of a table, so we should be oblivious to this natural variation.

- View Point/Vantage point

  + This one of the hardest natural variations to process, because depending on the vantage points objects can appear really differently.


Grayscale images
-------------------

In computer vision, we mostly use grayscale images, because it is more robust to illimunation variance:
It can be described mathematically as, a matrix of several hundered rows and several hundered columns, containing pixel values, that is intensity of light values. 
highest value 255, white
lowest value 0, black.

Color images would contain, 3 values as an element of the matrix, red value, green value, and the blue value.

Extracting Features
---------------------

How to for example detect a horizontal edge feature. That is a horizontal line in the given image.

The most obvious feature extractor, would be something like the following, we run a 2 value matrix across the entire image.
This 1x2 matrix gives us the direction for applying addition and subtraction.
For example if we have [+|-] this means we subtract the second term from the first term, or if we have [-|+] , this means that we subtract the first term from the second term.

Example:

Let's say we have the following:

.. math::

   image = \left[
   \begin{array}{ccccc}
   255 & 212 & 7 & 1 & 3 \\
   211 & 231 & 3 & 9 & 0 \\
   218 & 240 & 8 & 12 & 2 \\
   241 & 241 & 5 & 4 & 0 \\

   \end{array}
   \right]

   mask = [+|-]

   result = \left[
   \begin{array}{cccc}
   255 - 212 = 43 & 212 - 7 = 205 & 7 - 1 = 6 & 1 - 3 = -2 \\
   211 - 231 = -20 & 231 - 3 = 228 & 3 - 9 = -6 & 9 - 0 = 9 \\
   218 - 240 = -22 & 240 - 8 = 232 & 8 - 12 = -4 & 12 - 2 = 10 \\
   241 - 241 = 0 & 241 - 5 = 236 & 5 - 4 = 1 & 4 - 0 = 4 \\
   \end{array}
   \right]

In the resulting matrix, the second column, that is :math:`result_{:,2}` stands out with its high pixel values **compared** to adjacent columns, we have probably a horizontal edge over there.

What we did was to apply a linear filtering to the image. The formula for linear filtering is, :math:`I'(x,y) = {\sum_{u,v} I(x - u, y - v) · g(u,v)}`
It reads as follows:

- The pixel accorded by x, and y, in the new image I', is obtained by summing over all layers in the kernel, u and v of the original image shifted by u and v, times the kernel itself.
- u and v are indexes that indicate how the kernel maps to the original image.
  The function does what we just did. Let's analyse it further:

Our kernel was 1x2 matrix with +1 and -1 as values in the row.
So u = [1], and v = [1, 2]
Let's say, in our image we are at the second column of the second row, so x=2, y=2.
Assuming that the edges are covered with zero value pixels.
That would mean in our new image:

the pixel I'(2,2) would be equal to I(2-1=1,2-1=1)·g(1,1) + I(2-1=1,2-2=0)·g(1,2)
255 · 1 + 0 · -1 = 255 + 0 = 255

let's say x=2, y=3.

the pixel I'(2,3) would be equal to I(2-1=1,3-2=1)·g(1,1) + I(2-1=1,3-2=1)·g(1,2)
212·1  + 255· -1 = -255 + 212 = -43

For horizontal edges we would need the transposition of our kernel, so 2x1.

Since a horizontal edge is characterized by the consistency of the difference
between the superiour and the inferiour pixel across the rows.
So for an image like:

____________________
|                  |
|1 2 1 4  2 2  1 2 |
-------------------|
|9 8 7 13 8 10 9 8 |
|                  |
°°°°°°°°°°°°°°°°°°°°

As you can see the horizontal edge "-" is revealed by the difference between the
superiour and the inferiour pixel values.

########################################
Horizontal filter finds vertical edges##
Vertical filter finds horizontal edges##


Gradient image
----------------

The following are two gradient images resulting from our filtering process.

I_x = I ⊗ [+1,-1]
I_y = I ⊗ [+1,-1]^T

These two gives us vertical and horizontal edges respectively. 

A good way to find edges in any direction is the following formula:
:math:`{\sqrt{{I_x}^2 + {I_y}^2}}`

This response over here tells us how strong the gradient is in any directions.

This is a good example of a simple feature detector. A detector that detecs
edges in the image.

Another edge detector would be Canny Edge detector which finds the local maxima
and connects that so that they will create a single edge, if there are multiple
edges, the detector leaves a hole.

Other masks/kernels that we can use are:
Sobel masks
Prewitt masks
Kirsh masks

You can come up with your kernel, for detecting certain features.


Gaussian Kernels
-------------------

Gaussian Kernels are characterized by their specific structure.
Their value is maximum at the center and they fall exponentially as we go to the
side of the matrix.

The resulting gradient image is a blurred image, now the question:
Why would we want a blurred imaged ?

2 major reasons:

- Down sampling the image
  + High resolution images sometimes give weird results while iterating over the
    pixels
- Noise reduction
  + You respond to pixel noise that may otherwise make it hard to compute image
    gradients.

We can combine gaussian kernels with other gradient kernels, so that we can make
edge detection and smoothing.

Harris Corner Detector
------------------------

The idea behind the Harris corner detection is simple,

In the following vertical and horizontal detectors
I_x = I ⊗ [+1,-1]
I_y = I ⊗ [+1,-1]^T

We know that, if I_x has a large value we likely have vertical edge
if I_y has a large value we likely have horizontal edge,
if both I_x and I_y have large values we likely have a corner.
Because a corner is where a vertical edge meets with a horizontal edge.

Harris Corner Detector generalizes to the images, by rotating the coordinate
plane. It does so with the following eigendecomposition of the preceeding edge
detection formulas.

.. math::

   \left(
   \begin{array}{cc}

   {\sum{I_x}^2} & {\sum{I_x}{I_y}} \\
   {\sum{I_x}{I_y}} & {\sum{I_y}^2} \\

   \end{array}
   \right)

When we apply eigen value decomposition to this matrix, we obtain 2 eigen
values. The local maxima for these eigen values give us the corners, that is
when both are large.

Two most common methods for feature extraction are:

- HOG: Histogram of Oriented Gradients
- SIFT: Scale Invariant Feature Transform.

3D Vision
=================

Can we recover depth from a 2d image ?

- Sometimes:
  + If we know the size of the object in the picture, we can recover the depth
  of the image as well
  + If we don't know the size we can't.

Stereo
---------

This is what humans use for recovering the depth of a scene.

We have two cameras/eyes with identical focal lengths.
Their displacement, that is the fact that one is slightly above or near to
another is reflected on the projection based on the stereo rig, that is the
imaginary base line perpendicular to the projection plane which goes through
the pinhole of the camera.

This helps us to recover the depth for most vertical shapes, but for horizontal
object of interest that goes beyond the cadrage of the image, there is something
called **aparture effect** where the angle from which you look at the object
doesn't really change what you see in the object.

How to find the baseline, ie, depth ?

The formula for finding the depth in a stereo setup is the following:

p: projected object
f: focal distance
z: depth
B: baseline
x_1: the position of the projection of p
x_2: the position of the projection of p

:math:`{\frac{x_2 - x_1}{f}}={\frac{B}{z}}`

Basically, we look at the relative displacement of the two camera images,
x_2 - x_1, we see that depth is inversely proportional but scales linearly with
focal distance,f, and baseline,B.
focal distance and baseline are constant, the x_2 - x_1 is calculated.
x_2 - x_1 is sometimes called delta_x

Example:
z = 100 m
delta_x = 1 mm
baseline = 0.5 m
f = ?

500 / 100000 = 1 / f

1/200 = 1/f
f = 200mm

Correspondance
-----------------

You found an interesting point in left image, where do you search for it in the
right image:

- Along the same line

How can we determine the correspondance ?
Two main ways:

- Compare small image patches
- Compare features

Minimizing SSDs
-----------------

Minimizing the SSDs (sum of squared differences) error of two
images would mean approaching to equal images in the same axis.

This is basically comparing small image patches.

Algorithm for SSD minimization:

- We take two patches, one from the left image L, and another from the right image
R, then

- We normalise so that the average brightness is 0

- We then take the normalised image and take the difference.

- Then we square the difference.

- The we sum up all the pixels which gives us a single value, for SSD.

And smaller the value closer you are to the image in question.
Common and useful technique for comparing for what's called, image templates,
your left image is the template and you are trying to find the optimal template
in the right image

Alignment
-------------

Alignment of entire lines are done with cost functions:

For example while we are trying to match two lines, we say
a bad match would cost 20, a good match would cost 0,
an occlusion would cost 10.
In the case of following two series:

R - R - B - B - B - G - G - G

R - B - B - B - R - G - G - G

Two possible matches can be allowed:


Match 1
R - R - B - B - B - G - G - G
|     /   /   /     |   |   |
R - B - B - B - R - G - G - G

Match 2
R - R - B - B - B - G - G - G
|   |   |   |   |   |   |   |
R - B - B - B - R - G - G - G

The match 1 would have the cost 20 because of two occluded pixels in the serie,
1 at the bottom and 1 at the top serie.

The match 2 would have the cost of 40 because of two wrong matched pixels in the
serie.

The problem can be solved using the n^2 algorithm,
n, being the number of pixels in the scan line.

What we do is to create a matrix from two series:

R - R - B - B - B - G - G - G
°
B

B

B

R

G

G

G                           °

The idea is that any path between two "°" signs are a valid path.
We have three possible movement in this path, diagonal, that is 0 cost movement,
down or right, which can mean occlusion or wrong match.
The process is the exact procedure in the HMM's trellis.

The algorithm is the following:

The optimum value in the path W, is designated here as W_{i,j is the maximum
of the match cost, plus the point before 

I don't quite get the algorithm, but here it is:

:math:`W(i,j)=max( match(ij) + V(i-1,j-1) + occul+V(j-1,i) +occul+V(j,i-1) )`


Stereo vision can be immensily improved by emitting structured light, that is
light that creates stripes in different widths on the object, facilitating the
discerntion of the object under it.


Structure from Motion
------------------------

These are models that gives us a 3d representation from pictures of the object.

SFM math is really really involved

A good indicator of a well posed, solveable computer vision problem, requires
that:
m, camera poses, or motion
n, points, structures

2 × m × n, number of constraints

number of unknowns: 6m + 3n
7 unknowns can not be recovered because absolute positioning of the entire
system for each points in x and y coordinates can not be recovered, and the
scale of the images can not be recovered.


Image Processing
=================

Computer Vision Pipeline:

- Input: Image, Image Frames, Image Stacks
- Pre-Processing: Noise Reduction, Color Correction, Scaling, etc.
- Selecting Areas of Interest: Face Reduction, Image Cropping, etc.
- Feature Extraction: Finding Facial Markers (Mouth, Eyes, ... ), etc.
- Prediction/Recognition: Facial Expression Recognition, Emotion Prediction, etc
- Action/Output

Image Preprocessing
--------------------

Process of making images easier to analyze and process computationaly.
It has 2 steps:

- Correct images and eliminate unwanted traits
- Enchance the important parts of an image

What is important in an image is its intensity. That is a measure of lightness
and darkness

The patterns in lightness and darkness often define the shape and the
distinguishing characteristics of an object in an image

When color IS important
------------------------

In general if objects or traits are easier to identify in color for us humans,
it is better to provide color images to algorithms. For example, it is easier
for us to understand the stopping light in traffic, which is red, by using
color as a parameter.
Common example is computer aided diognastic tools for medical images. Color can
be a good indicator for a tumor or any other sickness for a doctor. Thus, we
should be using color images.

What are digital images
------------------------

They are functions of space, represented as 2d arrays. 

Color Spaces and Transformations
----------------------------------

Blue screen color detection depends on consistent color and even lighting. 
This can not be the case for a lot of images.
We need a method to detect objects under varying light conditions.

A colorful object is just a representation in a 3 axis coordinate space with a
certain area.
For example RGB is a color spaces whose axis are Red, Green, Blue.
There are also other color spaces like HSV whose axis are Hue, Saturation, Value
or HLS, for Hue, Lightness, Saturation

We are going to workout an example in using HSV color space. Why ?
Because Value of HSV is the most varying component under different lighting
conditions. The H channel stays fairly consistent under shadow or excessive
brightness. If we rely on this channel and disgard the information on V channel
we should be able to detect colored objects more reliably than RGB color space.

Hue: 0 - 180, since it is a measurement of degrees

See Colorspaces notebook for an example

Geometric Transformations
--------------------------

Geometric transformation of an image is essentially moving around the pixels
using a mathematical formula. By using these formulas we can change the
perspective of an image.
For example you can take an image of a text from an angle, and you can transform
it so that it is as if you are looking at it straight on.

Mathematically we can characterize perspective by saying in real world that is
3d coordinate space, z coordinate, which is the distance from the camera,
determines the scale of an image. That is greater the distance from the camera,
or greater the magnitude of z coordinate, the smaller the image would be.

Transforming Text
------------------

Document scanning is all about scanning and aligning documents for better
readability.
The first step for this is to make the input scans consistent and readable

For example a general app about reading business card would be something like:

- Take a picture of a business card
- Straighten and align the image
- Use text recognition to automatically read in contact information

We shall concern ourselves with the second step, that is straighten and align
and image.

In order to change the perspective of an image, you need 4 points of a plane in
the original image. These can be two things:

- Corners of your region of interest
- Corners of your image 

You need to provide the coordinates of those points, AND the coordinates of the
warped image, that is you need to give the coordinates of the original points,
and coordinates in which you expect the image to appear

Filters
--------

In addition to geometric and color space transformations, we can also use
patterns of intensity in an image to detect features and regions of interest.
For example edges, that is the difference between a pixel and a neighbouring
pixel help us to detect boundries

Edge detection filters are also known as high pass filters, that is they detect
big changes in intensity or color and produce an output that shows these edges.

There are also low pass filters for preprocessing image by reducing noise or
unwanted traits in an image

Frequency in images
--------------------

We have an intuition of what frequency means when it comes to sound.
High-frequency is a high pitched noise, like a bird chirp or violin. And low
frequency sounds are low pitch, like a deep voice or a bass drum. For sound,
frequency actually refers to how fast a sound wave is moving to make a certain
pitch. Faster sound waves, make higher pitches and are called high-frequency
waves.

High and low frequency
-----------------------

Similarly, frequency in images is a rate of change. But, what does it means for
an image to change? Well, images change in space, and a high frequency image is
one where the intensity changes a lot. And the level of brightness changes
quickly from one pixel to the next. A low frequency image may be one that is
relatively uniform in brightness or changes very slowly. This is easiest to see
in an example.
Most images have both high-frequency and low-frequency components. In the image
above, on the scarf and striped shirt, we have a high-frequency image pattern;
this part changes very rapidly from one brightness to another. Higher up in this
same image, we see parts of the sky and background that change very gradually,
which is considered a smooth, low-frequency pattern.

High pass filters
------------------

Edges are areas in image where the intensity changes rapidly. These often
indicate object boundries

Convolution kernels is a matrix of numbers that modify the image.


For example here is a 3x3 matrix whose elements sum up to 0:

+----+----+----+
| 0  | -1 | 0  |
+----+----+----+
| -1 | 4  | -1 |
+----+----+----+
| 0  | -1 | 0  |
+----+----+----+

Here what happens is that we subtract values of the surrounding pixels from the
center pixel. 

Now the reason behind summing up to 0 is the fact that this kernel tries to be
weightless, that is it does not try to increase or decrease the contrast in the
image. If the elements don't add up to zero, then the image would either
brighten or darken with respect to positive or negative outcome.

The process of applying this kernel to the image is called convolving. We say
the image is convolved with the kernel.
Convolution is essentially a suite of frobenius inner product, where the image
is represented as a vector of vectors which contain pixels with their
neighbours.

For example, here is an image matrix

+-----+----+-----+
| 100 | 51 | 100 |
+-----+----+-----+
| 180 | 56 | 90  |
+-----+----+-----+
| 130 | 98 | 200 |
+-----+----+-----+
| 180 | 56 | 90  |
+-----+----+-----+
| 130 | 98 | 200 |
+-----+----+-----+
| 100 | 51 | 100 |
+-----+----+-----+

In the case of convolving the image with the kernel, we simply take our source
image as a vector of neighbouring pixels vectors

Same image in different form:

[
    [0,100,51],[100,51,100],[51,100,0]
    # this would correspond to the first row in the image
    [0,180,56],[180,56,90],[56,90,0]
    # this would correspond to the second row in the image
    .
    .
    .
    [0,100,51],[100,51,100],[51,100,0]
    # this would correspond to the last row in the image
]

We then make a entrywise product, that is each entry of the kernel is multiplied
with their corresponding entry in the neighbouring pixels vector

For example for a kernel 1x3

+---+-----+---+
| 2 | -3  | 1 |
+---+-----+---+

Entrywise product with the first row would give something like

[0x2, 100x-3, 51x1],[100x2, 51x-3, 100x1], [51x2, 100x-3, 0x1]

Then in accordance with the frobenius inner product, we sum up each element
to come up with a value for the pixel position. We should remember that only
absolute values are allowed in the final image

[249, 147, 198] # new first row for the convolved image

Gradients

Gradients are a measure of intensity change in an image, and they generally mark
object boundaries and changing area of light and dark. If we think back to
treating images as functions, F(x, y), we can think of the gradient as a
derivative operation F ’ (x, y). Where the derivative is a measurement of
intensity change.

Sobel filters
---------------

The Sobel filter is very commonly used in edge detection and in finding patterns
in intensity in an image. Applying a Sobel filter to an image is a way of taking
(an approximation) of the derivative of the image in the xxx or yyy direction.
The operators for SobelxSobel_xSobelx​ and SobelySobel_ySobely​, respectively,
look like this:

Sobel filters

Next, let's see an example of these two filters applied to an image of the
brain.

Sobel x and y filters (left and right) applied to an image of a brain
xxx vs. yyy

In the above images, you can see that the gradients taken in both the xxx and
the yyy directions detect the edges of the brain and pick up other edges. Taking
the gradient in the xxx direction emphasizes edges closer to vertical.
Alternatively, taking the gradient in the yyy direction emphasizes edges closer
to horizontal.

Magnitude

Sobel also detects which edges are strongest. This is encapsulated by the
magnitude of the gradient; the greater the magnitude, the stronger the edge is.
The magnitude, or absolute value, of the gradient is just the square root of the
squares of the individual x and y gradients. For a gradient in both the xxx and
yyy directions, the magnitude is the square root of the sum of the squares.

:math:`abs_sobelx=(sobelx)2 = \sqrt{(sobel_x)^2}=(sobelx​)2`
​

:math:`abs_sobely=(sobely)2 = \sqrt{(sobel_y)^2}=(sobely​)2`
​

:math:`abs_sobelxy=(sobelx)2+(sobely)2 = \sqrt{(sobel_x)^2+(sobel_y)^2}=(sobelx​)2+(sobely​)2`
​
Direction
----------

In many cases, it will be useful to look for edges in a particular orientation.
For example, we may want to find lines that only angle upwards or point left. By
calculating the direction of the image gradient in the x and y directions
separately, we can determine the direction of that gradient!

The direction of the gradient is simply the inverse tangent (arctangent) of the
yyy gradient divided by the xxx gradient:

:math:`tan−1(sobely/sobelx)tan^{-1}{(sobel_y/sobel_x)}tan−1(sobely​/sobelx​)`

Later, we'll see exactly how the magnitude and direction of an image gradient
can be used in current applications. Let's keep learning!

Low Pass Filters
-----------------

They help us to reduce the noise in an image. Basically they act like and edge
detecter, only they detect low intensity areas. That is areas where the gradient
, the rate of change from one pixel to another is rather small.

You can either blur the image or block high frequency parts.

A simple kernel that does this is the following:

+---+---+---+
| 1 | 1 | 1 |
+---+---+---+
| 1 | 1 | 1 |
+---+---+---+
| 1 | 1 | 1 |
+---+---+---+

This simple kernel simply averages the central pixel over the neighbouring
pixels. Low pass filters typically take an average and not a difference as high
filters do, so their components should all add up to 1, and not 0.

However the above kernel adds up to 9.
If it stays that way it would also brighten the entire image. We should then
normalize the values by dividing it with the total sum, which is 9 in this case

+-----+-----+-----+
| 1/9 | 1/9 | 1/9 |
+-----+-----+-----+
| 1/9 | 1/9 | 1/9 |
+-----+-----+-----+
| 1/9 | 1/9 | 1/9 |
+-----+-----+-----+

Then again we convolve this kernel with the image.

For the sake of computation, we convolve with the first kernel provided above,
then we multiply it by 1/9. We do this because both proceedures are
mathematically equivalent, and it is less expensive to multiply multiple
integers with integers, instead of multiplying multiple floats with integers

Gaussian Blur
--------------

The most popular of low pass images. This is pretty interesting because it tends
to preserve the edges

Canny Edge Detector
---------------------

- What level of intensity change constitutes an edge ?
- How can we consistently detect both thick and thin edges ?

Canny edge detector is a response to all these

1. Filters out the noise by using a gaussian blur
2. Applies sobel filters in order to find magnitude and direction
   of the edges
3. Applies non maximum suppression to the output of the filters, in order to
   determine strongest edges and thin them to one pixel wide line
4. Applies hysterisis thresholding to isolate the best edges

Hysterisis thresholding works as follows:

1. Identify high intensity edges
2. Identify weak intensity edges
3. Check whether weak edge is connected to a strong edge
4. If it is connected to a high intensity edge, keep the weak edge, else
   we can disgard it.

Image Segmentation
-------------------

It is the process of dividing an image into segments or unique areas of interest.
It is mainly done in two ways:

- Area detection
- Form/Edge detection

Image Contours
---------------

Contours in an image are continous curves that follow the edges along a boundry

They provide information about the shape of the object.
In opencv contours are best detected when there is a white object against a
black background

From the contours, I can get the following properties:

- contour area
- center
- perimeter
- bounding rectangle

These are called contour features


Contour Features
-----------------

Below is an image of two hands, making a thumbs up and down. If I perform
contour detection on this image, I should get two distinct contours that define
the outline of the hands and arms.

Your task will be to determine some information about the shape of these
contours.

Two hands showing a thumbs up and thumbs down
Select a single contour

Your first step in getting information from a list of multiple contours, is to
select one at a time for analysis.

Assuming you've detected all the contours in an image, to select a single
contour, simply select one by index from the list of all contours. Below, the
first contour in a list is stored as selected_contour.

# Select the contour at index= 0 from a list
selected_contour = contours[0]

Then, to display this selected contour, you can use the same drawContours
function that you've seen but instead of a -1 passed in as a parameter, you need
to pass in the specific contour and an index value of 0.

# Draw the first contour (index = 0)
contour_image = cv2.drawContours(contour_image, [selected_contour], 0,
                                (0,255,0), 3)

# Draw all contours
contour image = cv2.drawContours(contour_image, contours, -1,  (0,255,0), 3)

Contour Features
-----------------

Every contour has a number of features that you can calculate, including the
area of the contour, it's orientation (the direction that most of the contour is
pointing in), it's perimeter, and many other properties outlined in OpenCV
documentation, here.

In this quiz, you'll be asked to identify the orientations of both the left and
right hand contours. The orientation should give you an idea of which hand has
its thumb up and which one has its thumb down!

Orientation
-----------

The orientation of an object is the angle at which an object is directed. To
find the angle of a contour, you should first find an ellipse that fits this
contour and then extract the angle from that shape.

# Fit an ellipse to a contour and extract the angle from that ellipse
(x,y), (MA,ma), angle = cv2.fitEllipse(selected_contour)

Orientation values
------------------

These orientation values are in degrees measured from the x-axis. A value of
zero means a flat line, and a value of 90 means that a contour is pointing
straight up!

So, the orientation angles that you calculated for each contour should be able
to tell us something about the general position of the hand. The hand with it's
thumb up, should have a higher (closer to 90 degrees) orientation than the hand
with it's thumb down.

Using this knowledge, your next task will be to focus only on the left hand.
Keep scrolling!

Bounding Rectangle
--------------------

In the next quiz, you'll be asked to find the bounding rectangle around the left
hand contour, which has its thumb up, and use a bounding rectangle to crop the
image and better focus on that one hand!

# Find the bounding rectangle of a selected contour
x,y,w,h = cv2.boundingRect(selected_contour)

# Draw the bounding rectangle as a purple box
box_image = cv2.rectangle(contour_image, (x,y), (x+w,y+h), (200,0,200),2)

And to crop the image, select the correct width and height of the image to
include.

# Crop using the dimensions of the bounding rectangle (x, y, w, h)
cropped_image = image[y: y + h, x: x + w]

Hough Transform
-----------------

Line detection is the simplest form of detection you can have. Its equation is
y=mx+b. The Hough transform takes this line, and maps it to a point in Hough
space also called parameter space.
A line is represented in Hough space by the virtue of its slope, m, and its
intercept b. That is in Hough space, one axis is for slopes, the other is for
intercepts. And the line is represented as a point in the Hough space
Multiple lines can be represented as multiple points
This is particularly interesting if we think of the patterns we can have.
Let's say we have multiple lines crossing through a point or is at around in
Hough space. That means there is a line with small discontinuities.
Our strategy then to find continous lines is to look at intersection points in
Hough space.

This idea is good but, the line that is straight up gives an interesting problem
because it has infinite slope, so a better way to represent the line is to
transform the Hough space into polar coordinates.

Instead of then m, and b, we have rho, which is the distance of the line from
the origin and theta which is the angle from the horizontal axis. Now a
fragmented line or points that fall in a line can be identified in Hough space
as the intersection of sinusoids

Line Detection and Navigation
------------------------------

Now it's your turn to apply what you know about the Hough transform on a
different image. The Hough transform is often used in simple navigation
techniques to find and align vehicles with lane lines or other well-defined
paths.

So, in this example, you'll use the Hough transform to detect lane lines in an
image of a road.

K-means
-----------

K-means is a simple ml algorithm that helps to cluster similar data points.
This is applicapble to images, because we can simply plot the images in 3d
coordinate system

Since any pixel is a point in 3d vector space, we can group pixels into clusters
based on their associated position in the 3d vector space

K-means cluster functions as follows:

1. Assign k number of centers to data set
2. Assign every data point to a cluster based on its nearest center point
3. Take the mean of all the values in the cluster
   1. Update the center points value, as the calculated mean value
4. Repeats step 2 and step 3 until convergence.
   The convergence is defined by us, it can be defined as how much the centers
   move after each update. If they move really really less, like 1 pixel, then
   we can consider them as converged, or it is an np hard problem


Image Segmentation with Deep Learning
--------------------------------------

Convolutional neural networks are generally associated with image classification
tasks where these networks learn to recognize objects given lots of labeled data.
Given many images of dogs, CNN's can be used to accurately identify many
different breeds and sizes of dogs better than many humans. However, humans
often don't just look for one object at a time to recognize and identify; they
often look at the many pieces and objects that make up a scene -- background,
foreground, perceived movement, and arrangement of objects -- and we form a
larger understanding of what is actually happening in an image. This is much
more than classification; it's interpretation and breaking an image into many
important pieces.

The first step in this is accurate segmentation, and many CNN architectures have
been developed to help with this task.
From classification to segmentation

One of the most basic use of CNN's in image segmentation is actually an extended
classification algorithm. Instead of recognizing single objects; CNN's can be
trained to classify every pixel in an image so that the image is broken up into
these different segments. This can be a very slow learning process, so other,
faster algorithms are still being researched and developed. Some networks learn
from smaller maps of image features, which we'll learn more about soon. You're
encouraged to read about the latest CNN segmentation techniques, here.


Feature Extraction and Object Recognition
-------------------------------------------

Features are distinct and measurable information in an image. We can use a
specific structure such as a line, an edge or specific color structure.

A good feature should be consistent across different scales, lighting condition,
and viewing angles. Ideally, they would be also visible in noisy images, and
where only part of an object is visible.

Typically features are quite small and a set of them should help you identify
larger objects. Most important quality of the feature is its repeatibility, that
is can I capture again in another image of the same object

Types Of Features
-------------------

There are three categories:

- Edges: areas in image where the intensity changes rapidly
- Corners: Where 2 edges meet
- Blobs: region-based features, areas of extreme brightness, or
  unique texture, etc

The most interesting one of these is corners, because they are more repeatable,
that is we can capture them easily across different images of the same object. A
corner represents a point where two edges change abruptly.

Corners can be formalized as high intensity in all directions, this is a
difference from edges which has high intensity in two directions

Each of these gradient measurements has an associated magnitude which is the
strength of the gradient and the direction of the change of intensity.
Both of these values can be calculated by using Sobel operators

Corner Detectors
-----------------

Image gradients help us to recognize corners.
Corners are intersection of edges.
We can detect them by taking an area from an image, usually a square frame,
and look whether the gradient is high in all directions
Each of these gradient measurements has an associated magnitude, a strength of
the gradient, that is the change, and a direction of the change.
The gradient in the X direction and the gradient in the Y direction can be
calculated using sobel operators. However we need to get the direction and the
magnitude of the total gradients from these operators. In order to do that,
we need to convert these values, that is gradient in X, and gradient in Y,
to polar coordinates.
We detect corners before applying binarisation.
Magnitude is rho and direction theta.
Gradient x, and gradient y is transformed into lengths as the two sides of the
gradient triangle, and the total gradient length is the hypothenus. Gradient x
is the length of the bottom side, and gradient y is the length of the right side

- magnitude is :math:`magnitude=\sqrt{G_x^2 + G_y^2}`
- direction is :math:`{\theta}=tan^{-1}{\frac{G_y}{G_x}}`

Inverse tangent is arctangent. How does arc tangent function ?
Simple for a given angle theta, arctangent means
arctangent(y) = theta, and tanget(theta) = y.
tangent(theta) = gradient_y / gradient_x = y
arctangent(y) = y - {\frac{y^3}{3}} + {\frac{y^5}{5}} - {\frac{y^7}{7}} + ...

We approximate the power series with 

Basically a corner detection is done by making a frame move around an area,
when there is a big variation in the direction and the magnitude of the
calculated gradients and this large variation identifies a corner

Dilation and Erosion
---------------------

Dilation and erosion are known as morphological operations. They are often
performed on binary images, similar to contour detection. Dilation enlarges
bright, white areas in an image by adding pixels to the perceived boundaries of
objects in that image. Erosion does the opposite: it removes pixels along object
boundaries and shrinks the size of objects.

Often these two operations are performed in sequence to enhance important object
traits!

Dilation
---------

To dilate an image in OpenCV, you can use the dilate function and three inputs:
an original binary image, a kernel that determines the size of the dilation
(None will result in a default size), and a number of iterations to perform the
dilation (typically = 1). In the below example, we have a 5x5 kernel of ones,
which move over an image, like a filter, and turn a pixel white if any of its
surrounding pixels are white in a 5x5 window! We’ll use a simple image of the
cursive letter “j” as an example.

Opening
--------

As mentioned, above, these operations are often combined for desired results!
One such combination is called opening, which is erosion followed by dilation.
This is useful in noise reduction in which erosion first gets rid of noise (and
shrinks the object) then dilation enlarges the object again, but the noise will
have disappeared from the previous erosion!

To implement this in OpenCV, we use the function morphologyEx with our original
image, the operation we want to perform, and our kernel passed in.

Closing
-------

Closing is the reverse combination of opening; it’s dilation followed by
erosion, which is useful in closing small holes or dark areas within an object.

Closing is reverse of Opening, Dilation followed by Erosion. It is useful in
closing small holes inside the foreground objects, or small black points on the
object.

Feature Vectors
----------------

If we want to recognize objects we need to look at the distinct set of features.
We call these feature vectors.

For example if we have a take the image of a trapezoid and divide it grid of cells
and look at the direction of the gradient in each cell with respect to center of
the trapezoid, we can flatten this data an create a 1d array. This would be a feature
vector for the trapezoid, in this case it is a vector of gradient directions.
These vectors simply represent the object in a different manner, however ideally,
they should admit some flexibility to be invariant to lightening and perspective
changes, but remain distinctive enough in order to recognize the different shapes

Histogram of Oriented Gradients
---------------------------------

Histogram is a graphical representation of the distribution of data.
It looks like a bar graph, where the height of each bar represents the amount
of the data that falls under a range also called bins
HOG simply produces a histogram of oriented gradient directions in an image

Here is how HOG functions:

1. Calculate the magnitude and direction of gradient at each pixel
2. Group these pixels into square cells, usually 8x8
3. Counts how many gradients in each cell fall in a certain range of orientations

Histogram of Oriented Gradients

HOG is also referred to as a type of feature descriptor, which is a simplified
representation of an image that is made up of extracted features (that highlight
important parts in an image) and that discards extraneous information. In this
case the features represent the image gradient -- it's magnitude and directions,
which describe the shapes and patterns of intensity that make up the image.

Implementing HOG
-----------------

There are a number of steps to create a HOG feature vector, and we'll go through
them here. It's also important to note that many image sets require pre-
processing as a first step to ensure consistency in size and color, but we will
gloss over that here to better focus on HOG.

For this example, we'll be looking at a small dataset of car images.

Note: The below code is to emphasize HOG steps and not for copy-and-paste usage.

1. Gradient Magnitude and Direction

First, HOG relies on calculation of the image gradient at each pixel; it's
magnitude and direction. And we already know how to calculate these values: with
Sobel filters! In the below code, I am using OpenCV's Sobel function instead of
creating my own filter.

Haar Cascades
---------------

Haar cascades work as following:

1. It detects haar features, which are gradient measurements that look at a
   rectangular regions around a certain pixel area and somewhat subtract these
   areas to calculate a pixel difference:
   - Edges
   - Lines
   - Rectangular shapes

Face detection uses lines and rectangles a lot because patterns of alternating
light and dark areas define a lot of features on a face. Ex. our pupils are a
dark area in a face where as our cheeks and chins define high gradient outline
for a face.

2. Detect non-face region
   - Basically we look at an area and say this area does not stimulate enough
     response in terms of detected features to be a face/object we are trying to
     detect, thus let's discard this information. It then spits out a an image
     reduced in size and forwards it to the next classifier

Haar cascades are fast enough that you can process a live video stream on a
laptop computer

Motion Detection
-----------------

When processing videos the only thing that is substantially different than
working with a static image is the motion. We detect motion by selecting some
features and detecting how they change over frames of image

A method for capturing this is called Optical Flow:

The constants are the following:

- Pixel intensities stay consistent between frames
- Neighbouring pixels have similar motion

It is used for tracking objects:

- eyes in virtual reality games
- cars in traffic
- running person from walking person
