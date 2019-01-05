#####################
Introduction to LSTM
#####################

This stands for Long Short Term Memory Networks, and are quite useful when our
neural network needs to switch between remembering recent things, and things
from long time ago.

In small summary RNNs work as follows:

1. Memory comes in 
2. Merges with the current event
3. Merged memory goes out.

In small summary LSTMs work as follows:

1. Long term memory comes in
2. Short term memory comes in
3. Both merges with the current event.
4. Merged long term memory goes out.
5. Merged short term memory goes out.


Technically speaking LSTM architecture has several gates:

- Learn Gate
- Forget Gate
- Use Gate
- Remember Gate

Here is how they work:

- Long term memory passes through the forget gate, where it shrinks by removing
  unnecessary information
- Short term memory passes through the learn gate with the current event
- Both the information in the learn gate and the forget gate, passes through
  remember gate. This creates the new long term memory
- Both the information in the learn gate and the forget gate, passes through
  use gate. This creates the new short term memory.

Learn Gate
----------

Learn gate combines the current event, the picture we are trying to identify, and
the short term memory, that is the memory of the pictures we have just seen.
It also forgets a little of it, keeping the important part.

Mathematically it works as the following:

Short Term Memory: STM
Event: E
Hyperbolic tangent function: :math:`tanh = {\frac{e^x - e^{-x}}{e^x + e^{-x}}}`
New Information: N
Ignore factor: i
[STM_{t-1}, E_t]: combining/concatenating two vectors.
sigmoid function: :math:`{\sigma}(x) = {\frac{e^x}{e^x+1}}`

Combination works as follows:

:math:`N_t = tanh(W_n[STM_{t-1},E_t] + b_n)`

Ignoring works as follows:

:math:`N_t {\times} i_t`

In order to calculate the ignore factor we create another small neural network
with sigmoid function:

:math:`i_t = {\sigma}(W_i[STM_{t-1},E_t] + b_i)`


Forget Gate
-----------

Forget gate simply forgets least relevant parts of the long term memory

The idea is the same as ignore factor of the learn gate.
This time we multiply the long term memory with the forget factor which
is calculated using the short term memory, and the event.
Mathematically it works as follows:

Long Term Memory: LTM
Short Term Memory: STM
Forget Factor: f
Event: E
sigmoid function: :math:`{\sigma}`

Forget part is as follows:

:math:`LTM_{t-1} {\times} f_t`

And the forget factor is calculated as follows:

:math:`f_t = {\sigma}(W_f[STM_{t-1}, E_t] + b_f)`

Remember Gate
-------------

Remember gate takes the output of the forget gate, and the learn gate and it
combines them together, in order to create the new long term memory

The combination is a simple addition.
It works as follows:

Long Term Memory: LTM
Forget Factor: f
New Information: N
Ignore factor: i

:math:`LTM_t = (LTM_{t-1} {\times} f_t) + (N_t {\times} i_t)`

Use Gate
--------

Use gate creates the short term memory by combining the output of the forget
gate and the previous short term memory and the current event.

Mathematically it works as follows:

Long Term Memory: LTM
Forget Factor: f
Short Term Memory: STM
Event: E
Hyperbolic tangent function: :math:`tanh = {\frac{e^x - e^{-x}}{e^x + e^{-x}}}`
New Information: N
Ignore factor: i
[STM_{t-1}, E_t]: combining/concatenating two vectors.
sigmoid function: :math:`{\sigma}(x) = {\frac{e^x}{e^x+1}}`

It applies the following to the output of the forget gate:

Output of the forget gate: :math:`LTM_{t-1} {\times} f_t)`

:math:`U_t = tanh(W_u(LTM_{t-1} {\times} f_t) + b_u)`

It applies the following to the short term memory and the current event:

:math:`V_t = {\sigma}(W_v[STM_{t-1}, E_t] + b_v)`

It combines both :math:`U_t and V_t` in order to obtain the new short term
memory as follows:

:math:`STM_t = U_t {\times} V_t`

Other Architectures
-------------------

Other variants of LSTM architectures are Gated Recurrent Units, and
Peephole Connections
