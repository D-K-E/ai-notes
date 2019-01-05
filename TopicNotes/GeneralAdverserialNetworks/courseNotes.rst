#############################################
Introduction to General Adverserial Networks
#############################################

What can you do with GANs ?
===========================

GANs are used for generating realistic data. Most of the applications of gans so
far have been on images

StackGAN takes a description of a bird and generates an image of a bird
based on that description.
In that context GAN is drawing a sample from the probability distribution of all
the images that match the description

Pix2Pix
GAN is used by Adobe and Berkeley, as the user draws crude sketches GANs
transform that to more realistic images.

They can be used for image to image translation, for example blueprints for
building can be transformed into photos of finished buildings

CycleGAN
is used by Berkeley is especially good at transforming images through
unsupervised learning

GANs can be used to create realistic simulated training sets or environments
in which the other machine learning models train, like reinforcement learning
agents

How GANs work ?
================

Generative Adverserial model is like other generative models.

Gans are a kind of generative model that let's us generate a whole image in
parallel. Along with other generative models gans use a differentiable function
represented by a neural network as a generator network.

The generator network takes random noise as the input, then runs that noise
through the differentiable function to transform and reshape the noise to
have recognizable structure.
The output of the generative network is a realistic image the choice of the
random noise determines what would come out of the network

Running the generator network with many different input noise values produces
many different realistic output images.
The goal for these images to be fair samples from the distribution over real
data

Of course the generator net does not start out producing real images it needs to
be trained. However training process is quite different from what we had seen so
far in supervised learning model

For most generative models we simply adjust the parameters to maximize the
probability that the model would generate the training set.
But it is very difficult to compute this probability, most generative models
get around that with some kind of approximation.
GANs use a second network called the discriminator in order to guide the
generator, the discriminator is a regular classifier.
During the training process the discriminator is shown real images half of the
time and fake images from the generator the other half.

Discriminator is trained to output the probability that the input is real,
so it tries to assign a probability close to 1 to real images and a probability
close to 0 to fake images.
Meanwhile the generator tries to produce images that would get a probability
close to 1 from the discriminator.

Over time the generator is forced to produce more realistic outputs in order to
fool the discriminator.

Games and Equilibra
--------------------

The inspiration for GANs come from Game theory.
Basically the generator and the discriminator are competing against each other.
Where each agent can choose a from some set of actions and the choice of actions
determines a well defined payoff for each player.
A state of equilibrium in game theory is where neither player can improve their
payoff by changing their strategy assuming the other players strategy stays the
same

In GANs, we have two agents, each with their own costs. They work with cost
functions, where you try to minimize the cost. In GANs you try to find a point
where it minimises the cost functions of both agents

For example for discriminator the local maxima occurs when the discriminator
accurately estimates the probability that the input is real rather than fake.
This probability is given by the ratio between the data density at the input and
the sum of both the data density and the model density induced by the generator
at the input. We can think of this ratio as measuring how much probability mass
in an area comes from the data rather than the generator

Deep Convolutional GANs
------------------------

The main difference being the convolutional networks for generator, and
discriminator.

Transposed convolutional layers do the exact opposite of convolutional layers.
They take something narrow, and make it larger and wider.
