##############################
Convolutional Neural Networks
##############################

CNNs
=====

In natural language processing:

- They are used for sentiment analysis.

Computer Vision

- Image classification tasks
  - Given the image the cnn would assign a label which it believes that
    it summarises the content of the image

- They help drones to navigate unfamiliar territory

CNN structure
---------------

Just like multilayer perceptron models, convolutional neural networks have the
input_layer + hidden_layers + output_layer structure

CNNs however differ from the multilayer perceptrons by using different hidden
layers:

- Convolution layers
- Pooling layers
- Fully connected layers

Miniproject: Training MLP in Mnist
-----------------------------------

Take note of the validation loss and test accuracy for the model resulting from
each of the below changes. (Try out each amendment in a separate model.)
Task List

- Increase (or decrease) the number of nodes in each of the hidden layers.
  Do you notice evidence of overfitting (or underfitting)?

- Increase (or decrease) the number of hidden layers.  Do you notice evidence of
  overfitting (or underfitting)?

- Remove the dropout layers in the network.  Do you notice evidence of
  overfitting? 

- Remove the ReLU activation functions.  Does the test accuracy decrease?
  - ReLU -> tanh: Test accuracy:

- Remove the image pre-processing step with dividing every pixel by 255.  Does
  the accuracy decrease?

- Try a different optimizer, such as stochastic gradient descent.
  - rmsprop -> sgd: Test accuracy: 97.7400, validation loss: 0,0863

- Increase/decrease batch size
  - 128 -> 150: Test accuracy: 97.6800%, validation loss: 0,0856

MLPs and CNN
--------------

MLPs
~~~~~

- Only use fully connected layers, as known as dense layers, that is layer is
  connected to every node
- Only use vectors as input, thus loosing pixel positions with respect
  to each other, so we loose locations

CNNs
~~~~~

- Also use sparsely connected layers
- Also accept matrices as input

By expanding the number of hidden layers we can discover more complex patterns
in our data

Convolutional Layer
--------------------

This is a special type of hidden layer that is found in convolutional neural
networks.

Basically it is constructed by a convolutional window/frame which is
like a mask that passes through the image horizontally and vertically. Each
passage defines a node, and the resulting nodes are grouped in a rayer called
convolutional layer.

The value for the convolutional layer node is created as in any linear
regression, that is we multiply the value of the pixel with the weights and sum
everything up including a bias term. Then we apply an activation function like
relu.

Weights are usually represented inside the convolutional window. Sometimes it is
called filter.

In order to detect each feature we need to have a convolutional layer for each.
Each layer has different weights

Normally what we do is to have 2d matrix for convolutional frame, if the image
is in color, then we use a 3d tensor for it

The convolutional frame is also called convolutional filters, and they are
randomly generated at first.
We won't be given to the cnn what kind of patterns or filters it needs to detect
, this will be learned from the data

Parameters of Convolutional Filters
-------------------------------------

- Height
- Width
- Stride: that is how much the filter slides on the original image
  - If we have a stride of 1, then the convolutional layer is about the
    height and width of the image
  - If we have a stride of 2, then the convolutional layer is about the
    half of the height and width of the image
    
- Padding: Sometimes the filter extends outside of the image, in that case
  we can do 2 things, get rid of the filter in the convolutional layer, or
  add some 0 as padding where the filter extends outside of the image.

Convolutional Layers in Keras
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

To create a convolutional layer in Keras, you must first import the necessary
module:

:code:`from keras.layers import Conv2D`

Then, you can create a convolutional layer by using the following format:

:code:`Conv2D(filters, kernel_size, strides, padding, activation='relu', input_shape)`

Arguments

You must pass the following arguments:

    - :code:`filters` - The number of filters.
    - :code:`kernel_size` - Number specifying both the height and width of the
      (square) convolution window.

There are some additional, optional arguments that you might like to tune:

    - :code:`strides` - The stride of the convolution. If you don't specify
      anything, strides is set to 1.
    - :code:`padding` - One of 'valid' or 'same'. If you don't specify anything,
      padding is set to 'valid'.
    - :code:`activation` - Typically 'relu'. If you don't specify anything, no
      activation is applied.
      **You are strongly encouraged to add a ReLU activation function to every
      convolutional layer in your networks.**

NOTE: It is possible to represent both kernel_size and strides as either a
number or a tuple.

When using your convolutional layer as the first layer (appearing after the
input layer) in a model, you must provide an additional input_shape argument:

    - :code:`input_shape` - Tuple specifying the height, width, and depth (in
      that order) of the input.

NOTE: Do not include the input_shape argument if the convolutional layer is not
the first layer in your network.

There are many other tunable arguments that you can set to change the behavior
of your convolutional layers. To read more about these, we recommend perusing
the official documentation.

Example #1

Say I'm constructing a CNN, and my input layer accepts grayscale images that are
200 by 200 pixels
(corresponding to a 3D array with height 200, width 200, and depth 1).
Then, say I'd like the next layer to be a convolutional layer with 16 filters,
each with a width and height of 2. When performing the convolution,
I'd like the filter to jump two pixels at a time.
I also don't want the filter to extend outside of the image boundaries; in other
words,
I don't want to pad the image with zeros.
Then, to construct this convolutional layer, I would use the following line of
code:

:code:`Conv2D(filters=16, kernel_size=2, strides=2, activation='relu', input_shape=(200, 200, 1))`

Example #2

Say I'd like the next layer in my CNN to be a convolutional layer that takes the
layer constructed in Example 1 as input.
Say I'd like my new layer to have 32 filters, each with a height and width of 3.
When performing the convolution, I'd like the filter to jump 1 pixel at a time.
I want the convolutional layer to see all regions of the previous layer, and so
I don't mind if the filter hangs over the edge of the previous layer when
it's performing the convolution. Then, to construct this convolutional layer,
I would use the following line of code:

:code:`Conv2D(filters=32, kernel_size=3, padding='same', activation='relu')`



if you look up code online, it is also common to see convolutional layers in
Keras in this format:

:code:`Conv2D(64, (2,2), activation='relu')`

In this case,
- there are 64 filters,
- each with a size of 2x2,
- and the layer has a ReLU activation function.

The other arguments in the layer use the default values,
so the convolution uses a
- stride of 1, and the
- padding has been set to 'valid'.

Number of Parameters in a Convolutional Layer
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The number of parameters in a convolutional layer depends on the supplied values
of filters,
kernel_size, and input_shape.
Let's define a few variables:

    - :code:`K` - the number of filters in the convolutional layer
    - :code:`F` - the height and width of the convolutional filters
    - :code:`D_in` - the depth of the previous layer

Notice that :code:`K = filters`, and :code:`F = kernel_size`.
Likewise, :code:`D_in` is the **last value in the input_shape tuple**.

Since there are :code:`F*F*D_in` weights per filter, and the convolutional layer
is composed of :code:`K filters`,
the total number of weights in the convolutional layer is :code:`K*F*F*D_in`.
Since there is one bias term per filter, the convolutional layer has
:code:`K biases`.
Thus, the number of parameters in the convolutional layer is given by
:code:`K*F*F*D_in + K`.

Shape of a Convolutional Layer
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The shape of a convolutional layer depends on the supplied values of kernel_size,
input_shape, padding, and stride. Let's define a few variables:

    - :code:`K` - the number of filters in the convolutional layer
    - :code:`F` - the height and width of the convolutional filters
    - :code:`S` - the stride of the convolution
    - :code:`H_in` - the height of the previous layer
    - :code:`W_in` - the width of the previous layer

Notice that :code:`K = filters`, :code:`F = kernel_size`, and :code:`S = stride`.
Likewise, :code:`H_in` and :code:`W_in` are the first and second value of the
input_shape tuple, respectively.

The depth of the convolutional layer will always equal the number of filters K.

If :code:`padding = 'same'`, then the spatial dimensions of the convolutional
layer are the following:

    - :code:`height = ceil(float(H_in) / float(S))`
    - :code:`width = ceil(float(W_in) / float(S))`

If :code:`padding = 'valid'`, then the spatial dimensions of the convolutional
layer are the following:

    - :code:`height = ceil(float(H_in - F + 1) / float(S))`
    - :code:`width = ceil(float(W_in - F + 1) / float(S))`


Pooling Layers
---------------

Pooling layers take convolutional layers as input.
A complex dataset with many different object categories would require a large
number of filters.
When the number of parameters are high, it might lead to overfitting.
So we need to reduce this dimensionality.

Max Pooling Layers
~~~~~~~~~~~~~~~~~~~

This takes a stack of feature maps as input. We determine the height and the
width of the
max pooling frame. We also indicate the stride.
This frame/window/filter simply passes through the convolutional filters of the
convolutional layer,
and finds the maximum value within its frame, then outputs the maximum value. So
with each passage
within the convolutional filter, a maximum value is generated by the max pooling
frame. This
creates the same number of filters but with reduced height and width.

Global average pooling layer
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

This is more extreme type of dimensionality reduction. It takes the stack of
feature maps as input,
and computes the average value of the nodes for each map in the stack.
As before we work with each feature map separately. We sum up all the values of
the feature map, then
divide it to the total number of values contained in the feature map.
So the global average pooling filter simply returns a scalar.
It takes a 3d input array, and returns a vector.

Max Pooling Layers in Keras
~~~~~~~~~~~~~~~~~~~~~~~~~~~

To create a max pooling layer in Keras, you must first import the necessary
module:

:code:`from keras.layers import MaxPooling2D`

Then, you can create a convolutional layer by using the following format:

:code:`MaxPooling2D(pool_size, strides, padding)`

Arguments

You must include the following argument:

    - :code:`pool_size` - Number specifying the height and width of the pooling
      window.

There are some additional, optional arguments that you might like to tune:

    - :code:`strides` - The vertical and horizontal stride. If you don't specify
      anything, strides will default to pool_size.
    - :code:`padding` - One of 'valid' or 'same'. If you don't specify anything,
      padding is set to 'valid'.

NOTE: It is possible to represent both pool_size and strides as either a number
or a tuple.

You are also encouraged to read the official documentation.

Example

Say I'm constructing a CNN, and I'd like to reduce the dimensionality of a
convolutional layer by
following it with a max pooling layer.
Say the convolutional layer has size (100, 100, 15), and
I'd like the max pooling layer to have size (50, 50, 15).
I can do this by using a 2x2 window in my max pooling layer,
with a stride of 2, which could be constructed in the following line of code:

    :code:`MaxPooling2D(pool_size=2, strides=2)`

If you'd instead like to use a stride of 1, but still keep the size of the
window at 2x2, then you'd use:

    :code:`MaxPooling2D(pool_size=2, strides=1)`

Checking the Dimensionality of Max Pooling Layers

Copy and paste the following code into a Python executable named pool-dims.py:

.. code:: python3

   from keras.models import Sequential
   from keras.layers import MaxPooling2D

   model = Sequential()
   model.add(MaxPooling2D(pool_size=2, strides=2, input_shape=(100, 100, 15)))
   model.summary()

Summary
--------

Convolutional Layers:

- Detect regional patterns in an image.

Max Pooling Layers:

- Come after convolutional layers to reduce their dimensionality


Image Size Problem
-------------------

Images online come in different sizes, whereas CNNs require same size input.
It is common to resize every image to a square with the spatial dimensions equal
to a power of 2.

Convolutional Neural Network Architecture
------------------------------------------

The effect of a cnn on image array is the following.

The image is a 3d array, if it is colored, if it is grayscale, we can think
of it as having a depth of 1.
So a color image shape example: (10, 10, 3), one cell of the matrix has 3 values
: red pixel, blue pixel, green pixel
a grayscale image shape example: (10, 10, 1), one cell of the matrix has 1 value
: brightness

Cnn architecture will be designed to:

- increase the depth of the image: Done by Convolutional layers
- decrease the width and the height of the image: Done by Max Pooling layers

General Structure:

- Input: image array
- Convolutional Layer 1: takes the input shape as a parameter,
  as well as other
  parameters that are present in a CNN,
  filter number X,
  stride=1
- Max Pooling Layer 1: takes the Convolutional Layer 1 as the input,
  stride=2, filter_size = 2, making it half of what they were in previous layer
- Convolutional Layer 2: filter number 2X.
- Max Pooling Layer 2
- Convolutional Layer N-1: filter number 2^{N-1}X
- Max Pooling Layer N-1
- Convolutional Layer N: filter number 2^{N}X
- Max Pooling Layer N
- Vector: Flattened version of the result of the last max pooling layer
- Fully Connected Layer, that connects to every element of this vector
- Output layer, with the activation function of softmax for having probabilities
  The density of the output layer should be equal to the number of output
  classes in an image classification system.
  If i am trying to guess whether a picture contains a dog or a cat,
  the density of the output layer should be equal to 2.


Hyperparameters in CNNs
------------------------

- Number of filters:
- kernel_size: This is usually constant for all of the layers
- strides: this is usually 1, since max pooling layer handles the dimensions
- padding: it is better to set this to true, so that it is done, in keras it is
  set to  "same".
- activation: most of the time "relu" is used except in the 
- input_shape

Image augmentation
--------------------

The thing is we deal with a lot of unnecessary information when we are dealing
with images, like its size, its angle, its position in the image etc.

We want our algorithm to learn an invariant representation of our image.

- Scale invariance, is invariance to object size
- Rotation invariance, is invariance to object's angle
- Translation invariance, is invariance to the position of the object in the
  image

CNNs have some built-in translation invariance due to max pooling layer,
but the real method for dealing with invariance problem, is data augmentation.

The idea is simple, if want our algorithm to be scale invariant, we add
zoomed images to our training set, if want our algorithm to be rotation
invariant, we add a little random rotation to images in our training set,
if want our algorithm to be translation variant we add flipped images
to our training dataset

Its usage in keras is given in the notebook cifar10_augmentation.ipynb

Common elements of Ground Breaking CNN Architectures
-----------------------------------------------------

They are all stacks of recurrent structures.
Like (two 3x3 filter, 1 pooling layer + 1 dropout ) * 140

Transfer learning
-------------------

Convolutional filters in a trained CNN are in hierarchy
The filters in the first two - three layers detect features like
colors, edges, general shapes like triangle, rectangular.


Simple Technique
~~~~~~~~~~~~~~~~~~

We can then get rid of the final layers and simply add one new layer
to general primitive layers we have seen, and train that layer.
This would work if we have a small dataset with large resemblance to
the dataset that the CNN was previously trained on.

Basically we are taking the last layer of the previously trained
cnn as the input layer, and feed it to another final layer.

Complex technique:
~~~~~~~~~~~~~~~~~~~~

- Randomly initialise the weights in the new fully connected layer
- Initialise the rest of the weights using the pre-trained weights
- Retrain the entire network

This is basically fine tuning the parameters of the network with a
different final classification layer

**ImageNet database for hieroglyphics** 
