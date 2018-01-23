==============
Regularization
==============

Bagging
=======

.. role:: red

**Bootstrap AGGregatING**. Reduces generalization error by combining several models. Train several different models separately, then have all the models vote on the output for test examples. It's an example of **model averaging** or **ensemble methods**. Averaging works because different models will usually not make all the same errors on the test set. :red:`Boosting constructs an ensemble with higher capacity than the individual models.`
