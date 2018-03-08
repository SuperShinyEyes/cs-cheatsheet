======================
Probabilistic modeling
======================

Intro
=====

What is a probabilistic programming language?
#############################################
Probabilistic modeling is a powerful approach for analyzing empirical information and a probabilistic programming language is a language that enables you to implement the method.

Why is probabilistic programming needed?
########################################
We live in an era where data became abundant and the efficient computing hardware became accessible. Probabilistic modeling is essential to fields related to its methodology, such as statistics and machine learning, as well as fields related to its application, such as computation biology, NLP and etc.

We can take advantage of probabilistic modeling to tackle difficult challenges and therefore we need probabilistic modeling.  

What is Edward?
###############
It's a probabilisitic modeling library built on iterative process for probabilistic modeling. For software systems to enable fast experimentation, Edward provides rich abstractions that embeds both a broad class of probabilistic models and a broad class of algorithms for efficient inference. As well, in order to meet the needs of high performance computing Edward supports distributed training and integration of hardware such as (multiple) GPUs.

What are the three steps in the iterative process for probabilistic modeling that Edward is built around?
#########################################################################################################
Given data from some unknown phenomena, 

1. Formulate a model of the phenomena
2. Use an algorithm to infer the model’s hidden structure, thus reasoning about the phenomena
3. Criticize how well the model captures the data’s generative process. As we criticize our model’s fit to the data, we revise components of the model and repeat to form an iterative loop.


Identify parts of the example probabilistic program (on p. 3–4) that correspond to the three steps mentioned in the previous question.
######################################################################################################################################

1. Modeling
^^^^^^^^^^^

.. code-block:: python

	import tensorflow as tf
	from edward.models import Normal
	W_0 = Normal(mu=tf.zeros([1, 2]), sigma=tf.ones([1, 2]))
	W_1 = Normal(mu=tf.zeros([2, 1]), sigma=tf.ones([2, 1]))
	b_0 = Normal(mu=tf.zeros(2), sigma=tf.ones(2))
	b_1 = Normal(mu=tf.zeros(1), sigma=tf.ones(1))
	x = x_train
	# They defined a two-layer Bayesian NN below.
	y = Normal(mu=tf.matmul(tf.tanh(tf.matmul(x, W_0) + b_0), W_1) + b_1, sigma=0.1)


2. Model hidden structure inference
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: python

  # Use variational inferences to make inferences about the model from data.
  qW_0 = Normal(mu=tf.Variable(tf.zeros([1, 2])),
                sigma=tf.nn.softplus(tf.Variable(tf.zeros([1, 2]))))
  qW_1 = Normal(mu=tf.Variable(tf.zeros([2, 1])),
                sigma=tf.nn.softplus(tf.Variable(tf.zeros([2, 1]))))
  qb_0 = Normal(mu=tf.Variable(tf.zeros(2)),
                sigma=tf.nn.softplus(tf.Variable(tf.zeros(2))))
  qb_1 = Normal(mu=tf.Variable(tf.zeros(1)),
                sigma=tf.nn.softplus(tf.Variable(tf.zeros(1))))

  import edward as ed
  inference = ed.KLqp({W_0: qW_0, b_0: qb_0,
                       W_1: qW_1, b_1: qb_1}, data={y: y_train})
  inference.run(n_iter=1000)


3. Criticize
^^^^^^^^^^^^

.. figure:: /images/bayesian/probabilistic_model_criticize_plot.png
  :align: center
  :alt: alternate text
  :figclass: align-center

  < The scatter plot is the observed data and the line the model. >
