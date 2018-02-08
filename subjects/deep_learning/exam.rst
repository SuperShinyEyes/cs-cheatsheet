====
Exam
====

TERMs
=====

Capacity
########
The capacity of a model is its ability to fit a wide variety of functions, often related to the number of free parameters. Too low capacity may lead to underfitting, too high to overfitting.

CNN
###
Neural networks with certain constraints enforced on the network topology, and parametrization; convolution may be used in the computations of at least one layer. A CNN consists of an input and an output layer, as well as multiple hidden layers. The hidden layers of a CNN typically consist of convolutional layers, pooling layers, fully connected layers and normalization layers.


Jacobian matrix
###############
A matrix of all first-order partial derivatives of a vector-valued function. When the matrix is a square matrix, both the matrix and its determinant are referred to as the Jacobian in literature.

.. math:: 
  \begin{align}
   J &=
   \begin{bmatrix}
    \frac{\delta f}{\delta x_1} & \dots & \frac{\delta f}{\delta x_n} 
   \end{bmatrix} 
   &=
   \begin{bmatrix}
       \frac{\delta f_1}{\delta x_1} & \cdots & \frac{\delta f_1}{\delta x_n} \\
       \vdots                        & \ddots & \vdots \\
       \frac{\delta f_n}{\delta x_1} & \cdots & \frac{\delta f_n}{\delta x_n} \\
   \end{bmatrix}
   \end{align}

The Jacobian matrix is important because if the function f is differentiable at a point x, then the Jacobian matrix defines a linear map ℝn → ℝm, which is the best (pointwise) linear approximation of the function f near the point x. This linear map is thus the generalization of the usual notion of derivative, and is called the derivative or the differential of f at x.

Minibatch
#########
It is a training method which lies in-between Batch algorithm and SGD. Batch algorithm trains a model with the entire dataset. SGD trains with a single data point. Each batch is small enough to be fit in memory, and the model updates are frequent enough not to converge prematurely. It is not too computationally expensive compared to SGD and the variance is not too high over training epochs.

Momentum
########
It is a gradient descent optimization method inspired by physical concept of momentum which enhances global minima search. Momentum accelerates gradient descent in the early training phase. Later, when you are close to convergence, the update delta become very small so the momentum becomes small so you won't jump over the global minima. Momentum value is within [0,1]

Vanishing and exploding gradient problem
########################################




Name THREE regularization methods
=================================

L1 regularization
=================
It is a sume of the weights.
The solution is not unique. It has a built-in feature selection and it is a result of having sparse coefficients.

L2 regularization
=================
It is a sume of the squared of the weights. It is computationally efficient due to having analytical solutions. Non-sparse outputs

Bagging
#######
**Bootstrap AGGregatING**. Reduces generalization error by combining several models. Train several different models separately, then have all the models vote on the output for test examples. It's an example of **model averaging** or **ensemble methods**. Averaging works because different models will usually not make all the same errors on the test set.

Dropout
#######
Technique for resolving overfitting in a large network. It randomly drop units(along with their connections) from the NN during training. [Dropout_A_Simple_Way_to_Prevent_Neural_Networks_from_Overfitting]_

.. [Dropout_A_Simple_Way_to_Prevent_Neural_Networks_from_Overfitting] https://www.cs.toronto.edu/~hinton/absps/JMLRdropout.pdf


Dropout rate
^^^^^^^^^^^^
Dropout rate p = 1, implies no dropout and low values of p mean more dropout. Typical values of p for hidden units are in the range 0.5 to 0.8. For input layers, the choice depends on the kind of input. For real-valued inputs (image patches or speech frames), a typical value is 0.8. For hidden layers, the choice of p is coupled with the choice of number of hidden units n. Smaller p requires big n which slows down the training and leads to underfitting. Large p may not produce enough dropout to prevent overfitting.

Learning rate and Momentum
##########################
Dropout introduces a lot of noise in the gradients compared to standard stochastic gradient descent. Therefore, a lot of gradients tend to cancel each other. You can use 10-100 times the learning rate to fix this. While momentum values of 0.9 are common for standard nets, with dropout 0.95-0.99 works well.

Max-norm Regularization
#######################
Large momentum and learning rate can cause the network weights to grow very large. Max-norm regularization constrains the norm of the vector of incoming weights at each hidden unit to be bound by a constant :math:`c` in the interval of 3-4.

