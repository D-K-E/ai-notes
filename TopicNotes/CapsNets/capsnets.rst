#########
CapsNet
#########

Intuition
===========

Drawback of CNNs is that they can not model spatial relationships between
features.

We can summarize this by saying, cnn's work fundamentally on 2d objects.
Thus they model 2d information. What's important is to model 3d relationships.
That is the position of features with respect to each other.

The thing is for an object, these relational positioning is an invariant.
For example your viewpoint can change but that does not change the body
proportions of the object you are looking at.

CNN's loose this information, due to usage of max pooling.

Let's recap how cnn's work.

CNN's have three layers:

- Convolution Layers
- Pooling Layers
- Fully connected (Dense) Layers

Convolution layers

Basically it is constructed by a convolutional window/frame which is
like a mask that passes through the image horizontally and vertically. Each
passage defines a node, and the resulting nodes are grouped in a layer called
convolutional layer.

The value for the convolutional layer node is created as in any linear
regression, that is we multiply the value of the pixel with the weights and sum
everything up including a bias term. Then we apply an activation function like
relu.
The weights are usually represented inside the convolution window

Pooling layers

They take convolutional layers as input and reduce its dimensionality. Either 
by finding the maximum value in the filters, thus keeping the number of
filters the same with reduced height and width, or by averaging all the
feature maps.

Fully Connected Layers

Takes all the input from previous layer makes a linear regression of the
input.

The problem for CNN is:
Internal data representation of a convolutional neural network does not take
into account important spatial hierarchies between simple and complex objects.

What does brain do:
From visual information received by eyes, they deconstruct a hierarchical
representation of the world around us and try to match it with already learned
patterns and relationships stored in the brain. This is how recognition
happens. 

What does capsules do:
It incorporates relative relationships between objects and it is represented
numerically as a 4D pose matrix.

Instead of aiming for viewpoint invariance in the activities of “neurons” that
use a single scalar output to summarize the activities of a local pool of
replicated feature detectors, artificial neural networks should use local
“capsules” that perform some quite complicated internal computations on their
inputs and then encapsulate the results of these computations into a small
vector of highly informative outputs. Each capsule learns to recognize an
implicitly defined visual entity over a limited domain of viewing conditions
and deformations and it outputs both the probability that the entity is
present within its limited domain and a set of “instantiation parameters” that
may include the precise pose, lighting and deformation of the visual entity
relative to an implicitly defined canonical version of that entity. When the
capsule is working properly, the probability of the visual entity being
present is locally invariant — it does not change as the entity moves over the
manifold of possible appearances within the limited domain covered by the
capsule. The instantiation parameters, however, are “equivariant” — as the
viewing conditions change and the entity moves over the appearance manifold,
the instantiation parameters change by a corresponding amount because they are
representing the intrinsic coordinates of the entity on the appearance
manifold.

Capsules encode probability of detection of a feature as the length of their
output vector. And the state of the detected feature is encoded as the
direction in which that vector points to (“instantiation parameters”). So when
detected feature moves around the image or its state somehow changes, the
probability still stays the same (length of vector does not change), but its
orientation changes.


Inner working of the Capsules
------------------------------

It is easier to understand the difference of capsules with respect to neurons
by remembering what neurons do.

Neurons:

- take a scalar input
- Multiply it with a weight
- Add a bias to it.

- Then we apply a function to map the resulting scalar value to a range of
  probabilities

We can think the scalar input as scalars as well. They take scalars, multiply
each with a weight, add each a bias, etc.

Capsules:

- Take vectors as input.
- Multiply it with a weight matrix that encode a spatial relation between
  lower level features (nose, mouth) and a higher level feature (face).
  For example if first weight matrix encode the relations between a nose and a
  face. The nose is ten times smaller than a face, and its direction is the
  same with a face since they lie in the same plane.
  Once we multiply it with a weight, the output would the position of the
  face. So from 3 input vectors (eyes, nose, mouth), we obtain a prediction
  about the position of a face.
  Evidently if 3 predictions point to a same position and a state of a face,
  then, the probability of having a face there is quite high.

- Scalar weighting of the input vector:
  Let's say we have 2 capsules which correspond to a higher level feature. How
  does a lower level capsule decide which of the two capsules should receive
  its input. For example, I have a capsule for a nose, should it send its
  output to human face or dog face. Assuming higher level capsules have
  already input that are close to each other, the problem becomes that of
  clustering. Can the output coming from the nose capsule form a cluster with
  those of human face or dog face ? This is decided by the scalar weighting.
  The main idea is since the scalar multiplication does not change the
  direction of the vector but changes its length. This is the essence, it
  shall be explained further later on.

- We sum the weighted output vectors. This is essentially same as a neuron
  where we combine the outputs of weights.

- We squash the resulting vector into a unit vector. The squashing function
  has two parts: Unit scaling and additional squashing. The formula is the
  following: 
  :math:`v_i = \frac{||s_i||^2}{1+ ||s_i||^2} \times \frac{s_i}{||s_i||}`
  Basically each input vector becomes a scalar and makes up the resulting
  vector.


Dynamic Routing: Scalar weighting of Input vectors
---------------------------------------------------

The algorithm is the following:

.. code:: python

    def squashVector(u_ij: [float]):
        """
        Squash a vector's length to a range between 0 - 1
        """
        vecnorm = u_ij.length
        unitvector = u_ij / vecnorm
        firstTerm = vecnorm ** 2 / (1 + vecnorm ** 2)
        return firstTerm * unitvector

    def dynamicRouting(r: int, u_ij_hat: [float], 
                        currentLayer, nextLayer):
        """
        Implementation of dynamic routing algorithm from:
        https://arxiv.org/pdf/1710.09829.pdf
        procedureROUTING(u_ji_hat, r, l ):
        for all capsule i in layer l and capsule j in layer(l+1):
            b_ij←0
            for r iterations do:
                for all capsule i in layer l:
                    c_i←softmax(b_i)
                for all capsule j in layer(l+1):
                    s_j←sum_i( c_ij * u_ji_hat)
                for all capsule j in layer(l+ 1):
                    v_j←squash(sj)
                for all capsule i in layer l and capsule j in layer(l+1):
                    b_ij←b_ij+ dotProduct(u_ji_hat, v_j)
            return v_j

        Parameters
        ------------
        r: number of iterations
        u_ij_hat: vector containing a list of float points, 
            like every vector it has length and a direction.
            length is the number of floating points, and direction
            is the angle that it makes with respect to origin of the space.

        currentLayer: contains a list of capsules
        nextLayer: contains a list of capsules as well.
        """
        # setting up priors
        nb_next_capsules = getNumberOfCapsules(nextLayer)
        c_ij = 1 / nb_next_capsules
        b_ij = 0
        for iteration in range(r):


