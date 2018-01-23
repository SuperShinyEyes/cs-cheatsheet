==============
Regularization
==============



Bagging
=======

.. role:: red

**Bootstrap AGGregatING**. Reduces generalization error by combining several models. Train several different models separately, then have all the models vote on the output for test examples. It's an example of **model averaging** or **ensemble methods**. Averaging works because different models will usually not make all the same errors on the test set. :red:`Boosting constructs an ensemble with higher capacity than the individual models.`

Dropout
=======
Technique for resolving overfitting in a large network. It randomly drop units(along with their connections) from the NN during training. [Dropout_A_Simple_Way_to_Prevent_Neural_Networks_from_Overfitting]_

.. [Dropout_A_Simple_Way_to_Prevent_Neural_Networks_from_Overfitting] https://www.cs.toronto.edu/~hinton/absps/JMLRdropout.pdf


Dropout rate
############
Dropout rate p = 1, implies no dropout and low values of p mean more dropout. Typical values of p for hidden units are in the range 0.5 to 0.8. For input layers, the choice depends on the kind of input. For real-valued inputs (image patches or speech frames), a typical value is 0.8. For hidden layers, the choice of p is coupled with the choice of number of hidden units n. Smaller p requires big n which slows down the training and leads to underfitting. Large p may not produce enough dropout to prevent overfitting.

Learning rate and Momentum
##########################
Dropout introduces a lot of noise in the gradients compared to standard stochastic gradient descent. Therefore, a lot of gradients tend to cancel each other. You can use 10-100 times the learning rate to fix this. While momentum values of 0.9 are common for standard nets, with dropout 0.95-0.99 works well.

Max-norm Regularization
#######################
Large momentum and learning rate can cause the network weights to grow very large. Max-norm regularization constrains the norm of the vector of incoming weights at each hidden unit to be bound by a constant :math:`c` in the interval of 3-4.
