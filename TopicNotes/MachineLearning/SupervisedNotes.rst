################################
Machine Learning - Supervised
################################

Bayes Networks are there for us to reason.
Machine Learning help us to find the bayes network model.

Machine Learning Taxanomy
==============================

Distinguishing elements in machine learning:

What do we learn ?:

- Parameters
- Structure
- Hidden Concepts

From what do we learn ?:

- Supervised, with target labels
- Unsupervised, without target labels but use replacement principals
- Reinforcement learning, when an agent learns from feedback with physical environment
like "Bravo!", "Ouf, that hurts"

What for ?

- Prediction
- Diagnosis
- Summarisation

How to learn ?

- Passive, learning agent doesn't interfere with what is being learnt
- Active, learning agent has impact on the data being learnt
- Online, learning occurs while the data is being generated
- Offline, learning occurs after the data is generated

Outputs ?

- Classification: binary, or discreet number of class
- Regression: continous

Details ?

- Generative: seeks to model the data as generally as possible
- Discriminitive: distinguish data

Supervised Learning
=====================

Data structure is of the following:

{x_1, x_2, x_3, x_4, ..., x_n -> y_1}
{a_32, a_33, a_34, ..., a_n -> y_2}

our job is to find the f() in which f(x_m) = y_m

Occam's razor:
Everything else being equal, choose the less complex hypothesis.

Trade off within the machine learning is that:

good fit <--> low complexity

Meaning that good fits for the data are usually high in complexity.

Spam Detection
-----------------

In spam detection we got an email, we try to classify it, either as SPAM or HAM,
ham means it is worth passing to another person as a person mail.

1 technique that is being used is bag of words in which we represent each word
as a frequency:
Ex:

hello my name is Soma, yes Soma is my name.


In a dictionary they are represented as the following:

hello my name is Soma yes
1     2   2   2   2    1

Bayes Networks
---------------

What we had been doing in spam detection is building a bayes network, where we
build up the parameters of the bayes network through the supervised learning by
maximum likelihood estimator based on training data.

Small refresher:

If we have structure like:

    Spam
  /      \
w1       w2

how many parameters we would need given that the dictionary has 12 words.
The answer is 25
Why ?
For nodes that have incoming edges we would need two times the number of edges
coming into the node, for nodes that are not recieving any edges, you would need
only one parameter.

Message = Sport

P(Spam | Message) = P(Message|Spam) P(Spam) / P(Message) = 0,1666...7
P(Spam) = 3/8
P(Message|Spam) = 1/9
P(Message) = 1/4

Message = Today is Secret
P(Spam | Message) = P(Message|Spam) P(Spam) / P(Message) = ?

P(Message|Spam) = P("Today is Secret"|Spam) = P(Spam) * P("Secret" under mails labeled as spam) * P("Today" under mails labeled as spam) * P("is" under mails labeled as spam)

P(Message) = P(Message|Spam)P(Spam) + P(Message|~Spam)P(~Spam)

Since today is not among the mails labeled as spam, it is 0, thus the probability of our mail being spam is 0.

This is a good example of overfitting, because we can not have a word determining the entire mail's label.

Maximum likelihood Estimate is basically, P(x) = count(x) / N
that is probability of x is equal to count of x in the class over all the data in the class.

Laplace Smoothing
-------------------

Laplace smoothing is like maximum likelihood estimator, but adds a variable k for smoothing.

Basically L(k,x) = count(x) + k / N + k|x|

This means that Probability(x) in laplace smoothing calculation, is equal to the number of the occurences of x in the given class,
N is total number of occurances in the given class, if no class is given it is equal to the total number of occurances in the data set, k is the smoother, |x| is the number of tokens in the data set

Ex:
Total number of tokens: 12

total number of occurences: 24

Spam class have: 9 occurences

Ham class have: 15 occurences

if k is 1,

What is the probability of  token with 0 occurences in Spam Class given the spam class ?

P(token|spam) = 0 + 1 / 9 + 12 = 1 / 21

Message = Today is Secret
P(Spam | Message) = P(Message|Spam) P(Spam) / P(Message) = ?

P(Message|Spam) = P("Today is Secret"|Spam) = P(Spam) * P("Secret" under mails labeled as spam) * P("Today" under mails labeled as spam) * P("is" under mails labeled as spam)

P(Spam) = 2/5
P(Secret|Spam) = 4/ 9+12
P(Today|Spam) = 1/ 9+12
P(is|Spam) = 2/ 9+12
x
\--------------------------
16 / 46305
0,000345

P(~Spam) = 3/5
P(Secret|~Spam) = 2/ 15+12
P(Today|~Spam) = 3/ 15+12
P(is|~Spam) = 2/ 15+12

36 / 98415
0,000365

P(Message) = P(Message|Spam)P(Spam) + P(Message|~Spam)P(~Spam) = 0,00071

This is basically naive bayes model which is a **generative** modal.

You can come up with other criteria for spam filtering, for example:

- Known spamming id?
- Have you emailed to this person before
- Have another 1000 people received the same message
- Email header consistent
- Written in all capitals
- Do inline urls point to where they say
- Are you adressed by your correct name.

Hand written digit recognition
--------------------------------

Applying naive bayes for hand written digit recognition.

When you are applying naive bayes, the input can be:

- Pixel vectors

  * Let's say we have a input vector of 16 x 16 you have to convolve the input vector with a gaussian variable, that is you take each pixel with neighbouring pixels for smoothing. This method is called input smoothing.

- But naive bayes is not a good method for this.

Overfitting prevention
------------------------

Remember the Occam's razor:

Cross validation is the key for overcoming the overfitting problem.

A typical division for the training data would be:

+-------------------------------------------------------------+
| Training data                                               |
+=====+===========================+===========================+
| %80 | Used for Training         | Figure out the parameters |
+-----+---------------------------+---------------------------+
| %10 | Used for Cross Validation | Find best smoothing param |
+-----+---------------------------+---------------------------+
| %10 | Used for Testing          | Verify the performence    |
+-----+---------------------------+---------------------------+

Figuring out of parameters is very much like finding out the probabilities of the bayes network.

Finding the best smoothing parameter, K, happens as the following.

You train the model with different K values and measure their performance in Cross Validation data.
Then you maximise over all K, to get the optimum k value.

Then you touch only **once** to test data, in order to verify your performance.

Regression Problems
=====================

Data structure is of the following:

{x_1, x_2, x_3, x_4, ..., x_n -> y_1}
{a_32, a_33, a_34, ..., a_n -> y_2}

our job is to find the f() in which f(x_m) = y_m

in linear regressions though the function f() has a particular form:

f() = w_1·x + w_0

Loss Function
-----------------

Loss function can differ:

Here is how quadratic loss function is calculated:

:math:`w_0 = {\frac{1}{M}{\sum}y_i - \frac{w_1}{M}{\sum}x_i}`

:math:`w_1 = {\frac{M{\sum}x_iy_i - {\sum}x_i{\sum}y_i}{M{\sum}x^{2}_i - ({\sum}x_i)^2}}`

M, is the number of training examples.


Problems with linear regression
--------------------------------

Linear regressions are very susceptible to outliers.
Outliers can change significantly the minimizing quadratic loss function.
They don't generalise well to data that is not really linear, like curves etc.
Some of the assumptions about the linear data might be wrong for such cases, for example
as the x goes to infinity in a linear data, the graph would give the impression that the
y also goes to infinity, whereas in reality that might not be the case.

Normalisation, or complexity control is also an issue to consider in linear regression.
Most of the time people use either L1 or L2 regularisation.

Perceptron
-----------

Perceptron algorithm works with a linear seperator.
That is a linear equation which seperates positives from the negatives.

f(x) = {1 if w_1 x+w_0 >=0,
        0 if w_1 x+ w_0 < 0}

Perceptron only convergence if the data is linearly seperable.

The iteration of the perceptron is of the following and it is very close to gradient descent.

Starts with a random guess for w_0, and w_1

w^m_i <-- w^m-1_i + a (y_j - f(x_j))

here m is the iteration index.
y_i, is the target label,
f(x_j) is the calculated target label.
a is the learning rate.
w^m-1_i is the weight calculated from the previous cycle
y_i - f(x_i) is our error.

Linear Methods Summary
----------------------

We talked about:

- Regression vs Classification
- Exact Solution vs Iterative Solutions
- Smoothing
- Non linear problems with linear methods, like SVMs, etc.

Parametric methods:

- Number of parameters are independent of the training size.

Non-parametric methods:

- Number of parameters are dependent of the training size, and can grow with the data size

K-Nearest Neighbor
--------------------

Simple thing, already studied in the intro to machine learning so, naive baysen.tex

Problems of KNN:

- Very large data size:
  + Solution: KDD, instead of representing the data as a list, we represent it as a tree

- Very large feature space:
  + Solution: After 2-3 dimensions, don't use knn, do tensor calculations.
