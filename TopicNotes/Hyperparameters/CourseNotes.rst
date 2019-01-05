#########################
Intro to Hyperparameters
#########################

The hyperparameter is the value we set before applying a learning algorithm into
a dataset.

The challenge is that there are no magic way to work with them. No trick or
principal that explains why a certain hyperparameter is better than the other
universally. Rather their capacity depends on the task at hand.

In general hyperparameters can be categorised into two categories:

- Optimizer Parameters
- Model Parameters

Optimizer Hyperparameters
~~~~~~~~~~~~~~~~~~~~~

They relate more to the optimization of the training process.
These include:

- learning rate
- mini batch size
- number of training iterations or epochs

Model Hyperparameters
~~~~~~~~~~~~~~~~~~~~~

These concern the structure of the model:

- The number of layers and hidden units
- And model specific stuff


Learning Rate
--------------

It is the single most important hyperparameter and one should always make sure
that it has been tuned.

If you have normalized the inputs to your model, then a good starting point is

0.01

A good list of indicators for learning rates is the following:

- 0,1
- 0,01
- 0,001
- 0,0001
- 0,00001
- 0,000001

If you try one and the model does not train, you try the other ones, and see how
the model responds to it.

Learning rate determines the step size of the descent during the gradient
descent.

Here are the possible cases that are possible with learning rate:

- Validation error: Decreasing during training 
  due to reasonable learning rate [✓]
- Validation error: Decreasing during training 
  due to large learning rate [✓] 
- Validation error: Decreasing too slowly during training 
  due to too small learning rate [x] 
- Validation error: Increasing during training 
  due to too large learning rate [x] 

Here is a good technique to deal with these problems:

- Learning rate decay:

  - We decrease the learning rate by half in every 5 epochs etc to see how well
    it functions

Minibatch size
---------------

It is the middle way between stochastic training and batch training.
Batch is we send everything at once, stochastic is we sent them one by one.

Increasing it would increase the computational requirement, decreasing it
would slower the training process, good numbers are:

2,4,6,8,16,32,64,128,256

One should think of adjusting the learning rate when one changes the mini batch
size

Number of iterations
---------------------

The metric we should focus in validation error. Idealy we start with a number
that is large to continue decreasing the validation error, and stop early
when the validation error stops decreasing.

However one should be flexible in the definition of the stopping trigger,
because the validation error would move back and forth, even if it is on
the downward trend.
In most cases, we stop the training if the validation error has not decreased
in the last 10 - 20 steps

Number of Hidden Units
----------------------

The number and architecture of the hiddent units are the main measures for a
model's learning capacity.

More capacity might lead to overfitting
Less capacity might lead to underfitting

Dealing with this can be done by using regularization techniques like:
- L2 regularization
- Dropout

Tips:

- For the first hidden layer:
  - It should be larger than the number of inputs
- Number of layers:
  - 3 layer would often perform better than 2, but more than 3 helps rarely
  - This is not true for CNNs where depth of +10 layers gives better results

RNN Hyperparameters
--------------------

There are two main parameters:

- Cell types: LSTM, Gated Recurrent Units Neural Network
- Depth of the model: how many layers we need

One thing is for sure: LSTM and GRU perform better than normal RNN
Discussion on LSTM and GRU is not resolved they depend on the task

Embedding size for word models should be around 50 - 200

LSTM Vs GRU

"These results clearly indicate the advantages of the gating units over the more
traditional recurrent units. Convergence is often faster, and the final
solutions tend to be better. However, our results are not conclusive in
comparing the LSTM and the GRU, which suggests that the choice of the type of
gated recurrent unit may depend heavily on the dataset and corresponding task."

Empirical Evaluation of Gated Recurrent Neural Networks on Sequence Modeling by Junyoung Chung, Caglar Gulcehre, KyungHyun Cho, Yoshua Bengio

"The GRU outperformed the LSTM on all tasks with the exception of language
modelling"

An Empirical Exploration of Recurrent Network Architectures by Rafal Jozefowicz,
Wojciech Zaremba, Ilya Sutskever

"Our consistent finding is that depth of at least two is beneficial. However,
between two and three layers our results are mixed. Additionally, the results
are mixed between the LSTM and the GRU, but both significantly outperform the
RNN."

Visualizing and Understanding Recurrent Networks by Andrej Karpathy, Justin
Johnson, Li Fei-Fei

"Which of these variants is best? Do the differences matter? Greff, et al.
(2015) do a nice comparison of popular variants, finding that they’re all about
the same. Jozefowicz, et al. (2015) tested more than ten thousand RNN
architectures, finding some that worked better than LSTMs on certain tasks."

Understanding LSTM Networks by Chris Olah

"In our [Neural Machine Translation] experiments, LSTM cells consistently
outperformed GRU cells. Since the computational bottleneck in our architecture
is the softmax operation we did not observe large difference in training speed
between LSTM and GRU cells. Somewhat to our surprise, we found that the vanilla
decoder is unable to learn nearly as well as the gated variant."

Massive Exploration of Neural Machine Translation Architectures by Denny Britz,
Anna Goldie, Minh-Thang Luong, Quoc Le


Example RNN Architectures
~~~~~~~~~~~~~~~~~~~~~~~~~~

+---------------------------------------+------+---------+---------------+---------------------------+-----------+---------------+--+
| Application                           | Cell | Layers  | Size          | Vocabulary                | Embedding | Learning Rate |  |
|                                       |      |         |               |                           |    Size   |               |  |
+---------------------------------------+------+---------+---------------+---------------------------+-----------+---------------+--+
| Speech Recognition (large vocabulary) | LSTM | 5, 7    | 600, 1000     | 82K, 500K                 |           |               |  |
+---------------------------------------+------+---------+---------------+---------------------------+-----------+---------------+--+
| Speech Recognition                    | LSTM | 1, 3, 5 | 250           |                           |           | 0.001         |  |
+---------------------------------------+------+---------+---------------+---------------------------+-----------+---------------+--+
| Machine Translation (seq2seq)         | LSTM | 4       | 1000          | Source: 160K, Target: 80K | 1,000     | --            |  |
+---------------------------------------+------+---------+---------------+---------------------------+-----------+---------------+--+
| Image Captioning                      | LSTM |         | 512           |                           | 512       | (fixed)       |  |
+---------------------------------------+------+---------+---------------+---------------------------+-----------+---------------+--+
| Image Generation                      | LSTM |         | 256, 400, 800 |                           |           |               |  |
+---------------------------------------+------+---------+---------------+---------------------------+-----------+---------------+--+
| Question Answering                    | LSTM | 2       | 500           |                           | 300       |               |  |
+---------------------------------------+------+---------+---------------+---------------------------+-----------+---------------+--+
| Text Summarization                    | GRU  |         | 200           | Source: 119K,             | 100       | 0,001         |  |
|                                       |      |         |               | Target: 68K               |           |               |  |
+---------------------------------------+------+---------+---------------+---------------------------+-----------+---------------+--+
