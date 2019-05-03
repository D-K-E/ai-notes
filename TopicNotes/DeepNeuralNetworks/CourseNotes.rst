####################
Deep Neural Networks
####################

Deep Learning is used in anywhere from go playing agents to self driving cars

Building Blocks of the Neural Networks
=======================================

What does a deep neural network does ?
---------------------------------------

It separates things:

- I have students with successfull notes and with failing notes. I ask what is the best line that separates the two ?
  Neural Network gives an answer like, from here.

Basically it is good for classification problems

Linear boundries
-----------------

Usually what separates the areas in these problems are a some sort of a line
with the equation :math:`Wx+b=0`, :math:`W=(w_1, w_2)` and :math:`x=(x_1, x_2)`:
The final equation is :math:`(w_1x_1)+(w_2x_2) + b = 0`:

- :math:`b` being the bias term, and 
- :math:`W` being the weights

In most cases we try to find :math:`y` value, that is a value that is to be predicted,
based on the values we have. Most of the time we encode the :math:`y` as either 1 or 0.

The algorithm for predicting if a students get accepted or rejected would be something like:

.. math::

   {\hat y} = \begin{cases}
   1 & if Wx + b {\ge} 0 \\
   0 & if Wx + b < 0
   \end{cases}

Higher Dimensions and Boundries
---------------------------------

The algorithm generalizes the same way to higher dimensions.:

- That is when we have 2 dimensions we try to find a line, line is a 1 d shape
- When we have 3 dimensions we try to find a plane/surface, plane is a 2 d shape
- When we have n dimensions we try to find a n-1 structure that divides the data.

The algorithm is still:


.. math::

   {\hat y} = \begin{cases}
   1 & if Wx + b {\ge} 0 \\
   0 & if Wx + b < 0
   \end{cases}

except that this time:

- :math:`W=w_1, w_2, \dots, w_n`
- :math:`X=x_1, x_2, \dots, x_n`


Perceptron
-----------

A perceptron is a combination of nodes. It contains the following node types:

- Input nodes which are :math:`x_1, x_2, \dots, x_n`
  - They take weights as edges
    
- Bias term node which is equal to 1:
  - It takes the bias term value as its edge

- Function nodes:
  - Linear function node: which takes the input nodes
    and the bias term node and the result of the linear equation
    :math:`Wx + b = 0` where:

       - :math:`W=w_1, w_2, \dots, w_n`
       - :math:`X=x_1, x_2, \dots, x_n`

  - Step function node: It takes the output of the linear function node
    and gives the result. For example in the above mentioned algorithm
    if the input is 0 or positive, step function gives 1 as the result,
    if it is negative, step function gives 0 as the result

A graphical representation would be something like:

         x_1                               1, yes
           \                              /
           w_1                          if 0, or positive
             \                          /
x_2 -- w_2 --  (w_1x_1)+(w_2x_2) + 1b = 
             /                          \
            b                           if negative
           /                              \
          1                                0, no

AND perceptron quiz
~~~~~~~~~~~~~~~~~~~~

weight1 = 1.0
weight2 = 1.0
bias = -2.0

Not Perceptron quiz
~~~~~~~~~~~~~~~~~~~

weight1 = 3.0
weight2 = -5.0
bias = 1.0

Xor multilayer perceptron
~~~~~~~~~~~~~~~~~~~~~~~~~

To construct this we need to combine three perceptrons we had seen so far.

We combine the result of a AND perceptron with NOT perceptron then combine input nodes 
with OR perceptron

Perceptron Algorithm
---------------------

1. Start with random weights and bias

2. For every misclassified point :math:`(x_1, x_2, \dots, x_n)`:

   - if *the prediction is 0*:

     - For :math:`i = 1, \dots, n`  
       - change :math:`w_i` to :math:`w_i + {\alpha}x_i`
       - change b to :math:`b + {\alpha}`

   - if *the prediction is 1*:

     - For :math:`i = 1, \dots, n`  
       - change :math:`w_i` to :math:`w_i - {\alpha}x_i`
       - change b to :math:`b - {\alpha}`

Explanation:

- alpha is the learning rate, it is used as a step term.
- Basically we put a random line on to n-1 dimensional hyperplane
- Then we ask ourselves how well we are doing. Since this is all done with
  labeled data. We can determine the misclassified points

- For every misclassified point vector, we change the weights and bias term
  accordingly, so that the seperator line comes either closer or farther
  towards the point vector.

- If the prediction is 0, that means normally the prediction should have been
  1 but it is classified as 0, then for the given point the line equation
  should have given a positive result, but with the current weights and bias
  it did not, so we increase the weights and the bias

- If the prediction is 1, that means normally the prediction should have been
  0 but it is classified as 1, then for the given point the line equation
  should have given a negative result, but with the current weights and bias
  it gave a positive result, so we decrease the weights and the bias.

.. code:: python3

    # Reference implementation

    def perceptronStep(X, y, W, b, learn_rate = 0.01):
        for i in range(len(X)):
            y_hat = prediction(X[i],W,b)
            if y[i]-y_hat == 1:
                W[0] += X[i][0]*learn_rate
                W[1] += X[i][1]*learn_rate
                b += learn_rate
            elif y[i]-y_hat == -1:
                W[0] -= X[i][0]*learn_rate
                W[1] -= X[i][1]*learn_rate
                b -= learn_rate
        return W, b

    # my implementation

    def stepFunction(t):
      if t >= 0:
          return 1
      return 0

    def prediction(X, W, b):
      return stepFunction((np.matmul(X,W)+b)[0])

    # TODO: Fill in the code below to implement the perceptron trick.
    # The function should receive as inputs the data X, the labels y,
    # the weights W (as an array), and the bias b,
    # update the weights and bias W, b, according to the perceptron algorithm,
    # and return W and b.
    def perceptronStep(X, y, W, b, learn_rate = 0.01):
      # Fill in code
      y_hat = np.array([prediction(X[i],W,b) for i in range(len(X))], 
                      dtype=X.dtype, ndmin=y.ndim)
      #print("y_hat: ",y_hat)
      #print("y: ", y)
      compr = y == y_hat
      for index in np.ndindex(compr.shape):
          if compr[index] == False:
              if y_hat[index] == 0:
                  #print("X index: ", X[index])
                  #print(W)
                  W[0] = W[0] + learn_rate * X[index][0]
                  W[1] = W[1] + learn_rate * X[index][1]
                  #print(W)
                  b = b + learn_rate
              elif y_hat[index] == True:
                  W[0] = W[0] - learn_rate * X[index][0]
                  W[1] = W[1] - learn_rate * X[index][1]
                  b = b - learn_rate
      print("W: ", W)
      print("b: ", b)
      return W, b


Error Functions
----------------

The problem with gradient descent, if the problem is discreet, we can not use it,
if it is continuous, we can use it. That is the error function has to be continuous and
we need to be doing continuous predictions

.. note:: Discreet == finite number of states in the environment, continous == infinite number of states

Softmax
~~~~~~~~

Let's say we have a classification problem, we want to know how good we did it ?
The idea is again to use error functions to determine it.
The way to do that is to calculate the distance between the data points and
the computed line. 
This distance would give how deep the data point is in the
classified space. If it is in the good area then the distance to the line
would indicate a larger probability that the point is well classified. If it
is around the frontier defined by the line, and still is in the good area,
then the distance would indicate that the *correctness* of the data point is
not that deep. If the point is misclassified, then it would have a higher
error rate, in the end we add all the errors together to find their sum, and
it is this value that we are trying to minimise.

Softmax function is just a generalisation of this phenomenon to N classes.

- We have N classes, which gives the following scores as the result of the linear function

  - :math:`Z_1, Z_2, ..., Z_n`, so there is a score for each class
  - The probability of a class i in :math:`i {\in} N`:
    - :math:`P(i) = {\frac{e^{Z_i}}{e^{Z_1} + e^{Z_2} + ... + e^{Z_N} }}`

.. code:: python3

   # softmax my implementation

   import numpy as np

   # Write a function that takes as input a list of numbers, and returns
   # the list of values given by the softmax function.
   def softmax(L):
      total_array = np.exp(L)
      sum_array = np.sum(L)
      result_array = total_array / sum_array
      return result_array

    

One Hot Encoding
~~~~~~~~~~~~~~~~

One hot encoding is an encoding procedure that is used a lot for providing numerical variables for the classes
in nodes.
For example let's say I have three classes: apple, orange, tomato
I can't pass this to an algorithm, because they are not numerical
What I can do though is to make a table of ones and zeros for these

Encoding table:

+----------+-------+--------+--------+
|          | Apple | Orange | Tomato |
+==========+=======+========+========+
| Apple    | 1     | 0      | 0      |
+----------+-------+--------+--------+
| Orange   | 0     | 1      | 0      |
+----------+-------+--------+--------+
| Tomato   | 0     | 0      | 1      |
+----------+-------+--------+--------+

So each instance has a numerical variable:

- Apple 1,0,0
- Orange 0,1,0
- Tomato 0,0,1

Maximum Likelihood
~~~~~~~~~~~~~~~~~~

Maximum likelihood is a obtained by generating probabilities that would give us
an overall higher probability scores from softmax function
That is:

- for :math:`i {\in} N, N = \{ 1, 2, ..., n \}`
- if :math:`P(i) = softmax(WX_i + b)`
  - what is :math:`max(P(N)) = ?`

This can be obtained by minimising the cross enthropy.

Cross Enthropy
~~~~~~~~~~~~~~

I have a bunch of events and a bunch of probabilities, how likely is it that 
those events happen based on the probabilities, if it is very likely than we 
have small cross enthropy if it is less likely than we have a big cross
enhtropy

It is obtained by summing up the negative logarithm of the probabilities.
Why logarithm ?
Because assuming that the events are independent, that is events that are
subject to probabilities are
independent, their total probability is their product. So:

- P(all) = P(1) × P(2) × ... × P(n)

This is very expensive to compute and it is very sensible to changes in the
probabilities
Logarithm transforms this equation to summation formula:

- log(P(all) = log(P(1) × P(2) × ... × P(n)) = log(P(1)) + log(P(2)) + ... +
  log(P(N))
- Now since we are dealing with numbers between 0 and 1, the result of the log
  is negative.
  To compensate that and have positive numbers we take the negatives of the
  logarithm

- The moment we take the negatives of the logarithms, we have a positive
  result, the greater the result, greater the cross enthropy. Since, closer
  the number is to 1, smaller its logarithm will be, seeing that log(1) is
  equal to 0.

.. code:: python3

   # my implementation

   import numpy as np

   # Write a function that takes as input two lists Y, P,
   # and returns the float corresponding to their cross-entropy.
   def cross_entropy(Y, P):
       Y = np.array(Y, dtype="float")
       P = np.array(P, dtype="float")
       log_p = np.log(P)
       p_one = 1 - P
       log_p_one = np.log(p_one)
       y_one = 1 - Y
       y_p_array = Y * log_p
       y_p_one_array = y_one * log_p_one
       total_array = y_p_array + y_p_one_array
       total_sum = np.sum(total_array)
       total_negative = np.negative(total_sum)
       #
       return total_negative

Multi-Class Cross Enthropy:

.. math::

   crossEnthropy = -{\frac{1}{m}}{\sum_{i=1}^{n}}{\sum_{j=1}^{m}} y_{ij} ln(p_{ij})

- m is the number of class
- n is the number of labels

For example if we have 3 doors which might have a gift behind,
if we have 4 type of gift: 


+----------+-------+--------+--------+
|          | Door1 | Door2  | Door3  |
+==========+=======+========+========+
| Gift1    | 0.4   | 0.3    | 0.2    |
+----------+-------+--------+--------+
| Gift2    | 0.3   | 0.2    | 0.1    |
+----------+-------+--------+--------+
| Gift3    | 0.2   | 0.3    | 0.1    |
+----------+-------+--------+--------+
| Gift4    | 0.1   | 0.2    | 0.6    |
+----------+-------+--------+--------+

- See that every column adds up to 1:

Why Cross Enthropy works ?

The formula for cross enthropy is the following:

- :math:`crossEnthropy = -{\frac{1}{m}}{\sum_{i=1}^{m} y_i × ln(p_i) + (1-y_i)×(1-p_i) }`

if y_i = 1:

P(blue) = :math:`\hat y_i`

Error = :math:`-ln({\hat y_i})`

if y_i = 0

P(Red) = 1 - P(Blue) = 1 - :math:`\hat y_i`

since there are only two possiblities red and blue

Error = :math:`-ln(1-{\hat y_i}`

Total Error = :math:`-ln({\hat y_i})-ln(1-{\hat y_i}`

Logistic Regression Algorithm
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Here is how the gradient descent algorithm works:

1. Start with random weights: :math:`w_1, w_2, \dots, w_n, b`
   - This gives a line
   - Now for every point we will calculate an error rate
     - The error is high for misclassified points but low for classified ones

2. For every point in x: :math:`x_1, x_2, \dots, x_n`
   - for :math:`i = 1, 2, \dots, n`
     - update the :math:`w_i' <-- w_i - {\alpha} {\hat y_i - y_i}{\times}x_i`
     - update the :math:`b' <-- b - {\alpha} {\hat y_i - y_i}`

3. Repeat untill the error is really small, and stays that way

.. code:: python3

   # Here is my implementation

   import numpy as np
   # Setting the random seed, feel free to change it and see different solutions.
   np.random.seed(42)

   def sigmoid(x):
       return 1/(1+np.exp(-x))
   def sigmoid_prime(x):
       return sigmoid(x)*(1-sigmoid(x))
   def prediction(X, W, b):
       return sigmoid(np.matmul(X,W)+b)
   def error_vector(y, y_hat):
       return [-y[i]*np.log(y_hat[i]) - (1-y[i])*np.log(1-y_hat[i]) for i in range(len(y))]
   def error(y, y_hat):
       ev = error_vector(y, y_hat)
       return sum(ev)/len(ev)

   # TODO: Fill in the code below to calculate the gradient of the error function.
   # The result should be a list of three lists:
   # The first list should contain the gradient (partial derivatives) with respect to w1
   # The second list should contain the gradient (partial derivatives) with respect to w2
   # The third list should contain the gradient (partial derivatives) with respect to b
   def dErrors(X, y, y_hat):
       error_array = error(y, y_hat)
       DErrorsDx1 = np.array([X[i][0]*(y_hat[i] - y[i]) for i in range(y.size)], dtype="float")
       DErrorsDx2 = np.array([X[i][1]*(y_hat[i] - y[i]) for i in range(y.size)], dtype="float")
       DErrorsDb = np.array([(y_hat[i] - y[i]) for i in range(y.size)], dtype="float")
       return DErrorsDx1, DErrorsDx2, DErrorsDb

   # TODO: Fill in the code below to implement the gradient descent step.
   # The function should receive as inputs the data X, the labels y,
   # the weights W (as an array), and the bias b.
   # It should calculate the prediction, the gradients, and use them to
   # update the weights and bias W, b. Then return W and b.
   # The error e will be calculated and returned for you, for plotting purposes.
   def gradientDescentStep(X: np.ndarray(ndim=2),
                           y: np.ndarray(ndim=2),
                           W: np.ndarray(ndim=2), b: int, learn_rate = 0.01):
       # TODO: Calculate the prediction
       # TODO: Calculate the gradient
       # TODO: Update the weights
       # This calculates the error
       y_hat = prediction(X, W, b)
       gradientw1, gradientw2, gradientb = dErrors(X, y, y_hat)
       gradientw1_learn = gradientw1 * learn_rate
       gradientw2_learn = gradientw2 * learn_rate
       gradientb_learn = gradientb * learn_rate
       gradientw1_sum = np.cumsum(gradientw1_learn)
       gradientw2_sum = np.cumsum(gradientw2_learn)
       gradientb_sum = np.cumsum(gradientb_learn)
       W[0] = W[0] - gradientw1_sum[-1]
       W[1] = W[1] - gradientw2_sum[-1]
       b = b - gradientb_sum[-1]
       e = error(y, y_hat)
       return W, b, e

   # This function runs the perceptron algorithm repeatedly on the dataset,
   # and returns a few of the boundary lines obtained in the iterations,
   # for plotting purposes.
   # Feel free to play with the learning rate and the num_epochs,
   # and see your results plotted below.
   def trainLR(X, y, learn_rate = 0.01, num_epochs = 100):
       x_min, x_max = min(X.T[0]), max(X.T[0])
       y_min, y_max = min(X.T[1]), max(X.T[1])
       # Initialize the weights randomly
       W = np.array(np.random.rand(2,1))*2 -1
       b = np.random.rand(1)[0]*2 - 1
       # These are the solution lines that get plotted below.
       boundary_lines = []
       errors = []
       for i in range(num_epochs):
           # In each epoch, we apply the gradient descent step.
           W, b, error = gradientDescentStep(X, y, W, b, learn_rate)
           boundary_lines.append((-W[0]/W[1], -b/W[1]))
           errors.append(error)
           #
       return boundary_lines, errors

   # reference implementation

   def dErrors(X, y, y_hat):
       DErrorsDx1 = [-X[i][0]*(y[i]-y_hat[i]) for i in range(len(y))]
       DErrorsDx2 = [-X[i][1]*(y[i]-y_hat[i]) for i in range(len(y))]
       DErrorsDb = [-(y[i]-y_hat[i]) for i in range(len(y))]
       return DErrorsDx1, DErrorsDx2, DErrorsDb

   def gradientDescentStep(X, y, W, b, learn_rate = 0.01):
       y_hat = prediction(X,W,b)
       errors = error_vector(y, y_hat)
       derivErrors = dErrors(X, y, y_hat)
       W[0] -= sum(derivErrors[0])*learn_rate
       W[1] -= sum(derivErrors[1])*learn_rate
       b -= sum(derivErrors[2])*learn_rate
       return W, b, sum(errors)

Perceptron vs Gradient Descent
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Same algorithm, except that the gradient descent changes weights for every point
whereas perceptron changes for those only that are misclassified.
In gradient descent points that are classified asks the separating line to go further away from the point,
In perceptron points that are classified asks nothing from the line

Neural Network Architecture
============================

Combining Regions
------------------

When we are combining to perceptrons, we are basically joining two linearly separating model of the same region
and add them to each other.
This happens the following way:

- We calculate the probability of the point in one model,M1: Ex. 0,7 for a blue point in blue region
- We calculate the probability of the same point in the other model, M2: Ex. 0,8 for the same blue point in the blue region
- We add these to each other in order to combine them, M3: 0,7 + 0,8 = 1,5.
  Problem, this is not a probability anymore since it is bigger than 1.
- Thus we apply the softmax function to this result and obtain,M3, 0,82.

Cool thing we can add weights to models we are using:

- I want to increase M1 7 times, M2 5 times, I can even add a bias like -6.

  - Same calculation: 0,7 × 7 + 0,8 × 5 - 6 = 2,9
  - We apply the softmax and: 0,95 we have.

Layers
-------

Neural Networks have layers:

- The first layer is called the input layer
- The second layer is called the hidden layer, it is a set of linear models
- The third layer is the output layer, it is the layer that gives the combined linear models

If we have more output nodes, then we are just dealing with a multiclass classification model.
For example we have an image in the input node, and we are trying to find if it is an image of
a bird, or a cat, or a dog.

Feedforward
------------

We have layers. We combine them, in order to obtain the output layer.
The process of combination is called the feedforward.

General structure of the feedforwarding is the following:

1. sigmoid(InputLayerNodes · WeightMatrix1) = FirstLayerResultNodes 

2. sigmoid(FirstLayerResultNodes · WeightMatrix2) = SecondLayerResultNodes

3. sigmoid(SecondLayerResultNodes · WeightMatrix3) = ThirdLayerResultNodes
.
.
.
N. sigmoid(N-1LayerResultNodes · WeightMatrixN-1) = y_hat or the prediction

Backpropagation
----------------

In a nutshell, backpropagation will consist of:

- Doing a feedforward operation.
- Comparing the output of the model with the desired output.
- Calculating the error.
- Running the feedforward operation backwards (backpropagation) to spread the error to each of the weights.
- Use this to update the weights, and get a better model.
- Continue this until we have a model that is good.

We have a hidden layer that consists of several models, some of them are correctly classifying the point,
some of them are not. This is the result of the feedforwarding.
In backpropagation we look at the points, we calculate the errors. Then we look back at the models that
misclassify the point, and update their weights, and bias, so that either they are taken less into account, or
they misclassify less, that is their error rate gets smaller

Chain Rule in Backpropagation
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

This rule says that:
If we have a variable x, and a function f:

x ---> A
   f

where f(x) = A
and

x ---> A ---> B
   f      g

where g(A) = B

partialDerivative B / partialDerivative x

= partialDerivative A /partialDerivative x × partialDerivative B / partialDerivative A


.. code:: python3

          import numpy as np
          from keras.utils import np_utils
          import tensorflow as tf
          # Using TensorFlow 1.0.0; use tf.python_io in later versions
          tf.python.control_flow_ops = tf

          # Set random seed
          np.random.seed(42)

          # Our data
          X = np.array([[0,0],[0,1],[1,0],[1,1]]).astype('float32')
          y = np.array([[0],[1],[1],[0]]).astype('float32')
          
          # Initial Setup for Keras
          from keras.models import Sequential
          from keras.layers.core import Dense, Activation

          # Building the model
          xor = Sequential()

          # Add required layers
          xor.add(Dense(8, input_dim=X.shape[1]))# Input layer
          xor.add(Activation("tanh"))# Activation function
          xor.add(Dense(1))# Output layer
          xor.add(Activation("sigmoid"))
          
          # Specify loss as "binary_crossentropy", optimizer as "adam",
          # and add the accuracy metric
          xor.compile(loss="binary_crossentropy",
          optimizer="adam",
          metrics=["accuracy"])

          # Uncomment this line to print the model architecture
          xor.summary()

          # Fitting the model
          history = xor.fit(X, y, nb_epoch=50, verbose=0)

          # Scoring the model
          score = xor.evaluate(X, y)
          print("\nAccuracy: ", score[-1])
          
          # Checking the predictions
          print("\nPredictions:")

TODO
------
Code backpropagation and feedforwarding

Learning Rate
--------------

Learning rate means steps in gradient descent. Big learning rate, means big steps, then you can miss the minimum,
small learning rate means computation can take a lot of time

Rule of thumb:

If your model is not working, decrease the learning rate

Overfitting and Underfitting
----------------------------

Underfitting: we are oversimplifying the problem. We are trying to solve a complex problem with a very
simple process.

Overfitting: we are trying to solve a simple problem with a very complex process/model

- Underfitting: high bias
- OVerfitting: high variance

Choosing the right model
~~~~~~~~~~~~~~~~~~~~~~~~~

Underfitting model:

- Training error: Big
- Test error: Big

Good model:

- Training error: Small
- Test error: Small

Overfitting model:

- Training error: Small
- Test error: Medium

Hugely Overfitting model:

- Training error: Tiny
- Test error: Large

The graph of this phenomenon is the model complexity graph

TODO: Implement Early Stopping Algorithm

L1 and L2 Regularisation
-------------------------

- L1 creates a lot of sparse vectors, it is good for feature selection

- L2 is good for training models

Both of them are simply a means to penalize large coefficients of the weights and in that
they are modifications to error functions

In L1 regularization:

.. math::

   crossEnthropy = -{\frac{1}{m}}{\sum_{i=1}^{m} y_i {\times} ln(p_i)
                    + (1-y_i){\times}(1-p_i) }
                    + {\lambda}{\times}(|w_1|, |w_2|, \dots, |w_n|)

The lambda part is the term we are adding in L1 regularisation.

In L2 regularization:

.. math::

   crossEnthropy = -{\frac{1}{m}}{\sum_{i=1}^{m} y_i {\times} ln(p_i)
                    + (1-y_i){\times}(1-p_i) }
                    + {\lambda}{\times}(w_{1}^2, w_{2}^2, \dots, w_{n}^2)

The lambda part is the term we are adding in L2 regularisation.

Dropout
----------

During the training process some nodes tend to have a lot of weights
so much so that they dominate the whole training. What is better is to
drop some of the nodes randomly based on a probability. For example, each node
would be dropped during the feedforwarding and backpropagation with the probability
of 0,2.

Hyperbolic tangent function
----------------------------

Sigmoid function gives very small derivatives making it difficult to use with gradient descent.
The way to optimize that is to use another step function. Here we introduce hyperbolic
tangent function: :math:`tanh(x) = \frac{e^x - e^{-x}}{e^x + e^{-x}}`

The difference between this function and the sigmoid function is that this one provides
a range between -1 and 1 so the derivatives are larger.

Rectified Linear Unit function
------------------------------

ReLU is a very simple function. If the derivative is positive it returns the derivative,
if it is negative it returns 0.

.. math::

   relu(x) = \begin{cases}
   x & if x {\ge} 0 \\
   0 & if x < 0 
   \end{cases}

Local Minima and Momentum
-------------------------

The key weakness of the gradient descent is the local minima that can occur in the dataset.
Once we hit to a local minima a gradient descent algorithm would not find a direction for
proceeding and it will be stuck in the local minimum and thus won't find the global minimum.

There are several ways to solve this question:

- Use random restarts, that is started descending from several places in the graph,
  take the lowest point computed.
- Use a different algorithm: Stimulated annealing, proved to find the global minimum
- Use some variant in stepping:

Momentum is a different way of calculating the steps:

step = average of previous steps

The term we introduce the momentum is :math:`{\beta}`, it is a constant between 1 and 0.
Simply put:

:math:`step(n) -> step(n) + {\beta}{\times}{step(n-1)} + {\beta}^2{\times}{step(n-2)} + \dots`

Optimizers in Keras
-------------------

SGD
~~~~~

This is Stochastic Gradient Descent. It uses the following parameters:

- Learning rate.
- Momentum (This takes the weighted average of the previous steps, in order to get a bit of momentum and go over bumps,
  as a way to not get stuck in local minima).
- Nesterov Momentum (This slows down the gradient when it's close to the solution).

Adam
~~~~~

Adam (Adaptive Moment Estimation) uses a more complicated exponential decay that consists of
not just considering the average (first moment), but also the variance (second moment) of the previous steps.

RMSProp
~~~~~~~

RMSProp (RMS stands for Root Mean Squared Error) decreases the learning rate by dividing it by an exponentially decaying average of squared gradients. 
