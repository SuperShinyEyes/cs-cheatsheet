========================
Recurrent neural network
========================

.. note::
  This part of the documentation is largely referring to Aalto CS-E4890.

.. figure:: /images/deep_learning/rnn_principle.png
   :align: center
   :alt: alternate text
   :figclass: align-center

   < Source: Aalto CS-E4890 >

RNN is a specialized NN for processing sequential data :math:`x^{(1)}, \cdots, x^{(\mathcal{T})}`. RNN employs parameter sharing just as CNN does. In CNN it is a kernel applied to a grid within images, and in RNN an n-gram sequence on sentences. RNNs are able to preserve information about the sequence order.


.. figure:: /images/deep_learning/rnn_unfolding.png
   :align: center
   :alt: alternate text
   :figclass: align-center

   < The unfolding the computational graph makes the RNN to resemble feedforward network structure. The back propagation is done normally. The parameters are shared over the training. Source: Aalto CS-E4890 >

.. figure:: /images/deep_learning/rnn_backpropagation.png
   :align: center
   :alt: alternate text
   :figclass: align-center

   < The back propagation through time is expensive as one has to calculate loss L from each time step to every hidden state i.e., :math:`L = \sum_{t=1}^{\mathcal{T}} L^{(t)}` Source: Aalto CS-E4890 >

.. figure:: /images/deep_learning/rnn_training.png
   :align: center
   :alt: alternate text
   :figclass: align-center

   < Training RNNs is unstable. Source: Aalto CS-E4890 >


Vanishing & exploding gradient problem
======================================
Here's a simple recurrence without input or activation function: 

.. math::
  h^{(t)} = W^T h^{(t-1)}

This can be also presented as several multiplications by the same weight matrix

.. math::
  h^{(t)} = (W^T)^t h^{(0)}

**W** can be factorized as 

.. math::
  \begin{align}
  \mathbf{W} = \mathcal{Q \Lambda Q^T},  & \\
  & \text{( $\mathcal{Q}$: orthogonal matrix composed of eigenvectors of $\mathbf{W}$, } \\
  &   \text{ $\Lambda$: diagonal matrix of eigenvalues)}
  \end{align}

We can thus conclude that:

.. math::
  h^{(t)} =  \mathcal{Q \Lambda Q^T} h^{(0)}

Since the eigenvalues are raised to the power of t, the gradient will explode if the largest eigenvalue is >1, and vanish if the largest eigenvalue is < 1

You can solve this issue by clipping gradients; just clip the gradient if it is larger than the threshold. Clipping can be done element-wise or in vectorized way.

.. math::
  if \|g\| > v, \quad  g \leftarrow \frac{gv}{\|g\|}





  