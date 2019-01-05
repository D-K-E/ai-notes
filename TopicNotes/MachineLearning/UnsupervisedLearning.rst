################################
Machine Learning - Unsupervised
################################

We try to guess the structure from the data.

For example you have a straight line in a coordinate system.
You can say that the space's dimensionality is equal to 2,
where as the line, can be represented in 1 dimension.

One of the basic applications of the unsupervised learning technology
is to represent higher dimensionality structures, like images, in lower
dimensions like histograms.

We learn about clustering and dimension reduction.

Some of the terminology used in unsupervised learning.
We assume that the data is IID, that is identically distributed and
independently drawn from the same distribution.

The unsupervised learning seeks to recover the underlying density of the
probability distribution that generated data.

This is called *density estimation*, following two are versions of it:

- Clustering
- Dimensionality reduction

Clustering
-------------

Two algorithms are pretty common in clustering:

- k-means
- expectation maximisation

Problem with k-means:

- need to know k
- local minima
- high dimensionality
- lack of mathematical basis.

Expectation maximisation is a generalisation of k-means, it uses actual
probability distributions to describe what we are doing.

Gaussian Learning
-------------------

Fitting gaussians to data, or gaussian learning in which we shall be given some
data points and wonder what is the best gaussian fitting the data.

.. image:: Gaussians.png

The formula actually represents a probability distribution.

Let's explain the multivariate one, since it works for the single dimension case as well.

The formula is: :math:`{(2{\pi})^{-\frac{N}{2}}} {S^{\frac{-1}{2}}} {exp({ {\frac{-1}{2}} (x-m)^T {\frac{(x-m)}{S}}})}`

N, is the number of dimensions of the data.

m, is the mu, that is the average of the samples.

x, is the probing point, that is our data point.

S, is the covariance matrix, that is the matrix that shows
how far the gaussian points are away from the mean that cuts through
the peak of the gaussian.

This thing tries to normalise the error rate by the covariance matrix

How to choose K in Expectation Maximization and K-means:

- Guess initial K
- Run EM
- Remove unnecessary clusters
- Create new random clusters
- Repeat from the second step

Dimensionality Reduction
-------------------------

Linear Dimensionality reduction:

The idea is that we are given data points, we seek to find a linear subspace to remap the data.

1. Fit a gaussian
2. Calculate the eigenvalues and eigenvectors of the gaussian
3. Pick eigenvectors with maximum eigen values
4. Project the data onto the subspace of the eigenvectors you chose.

Specteral Clustering
---------------------

Affinity to a group of data points define the cluster of a data point not its absolute position

Affinity matrix is essential to specteral clustering:
It is a matrix in which each data point is graphed relative to other data points
Affinity means the quadratic distance between points. High affinity means small quadratic distance.
This is a rank defficient matrix and it is easy to identify them with Principal Component Analysis.
PCA analyses the vectors that are similar in an aproximate rank defficient matrix.

Dimensionality = Number of Large Eigen values
