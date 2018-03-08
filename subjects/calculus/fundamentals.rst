============
Fundamentals
============

Why are we using :math:`e`? [1]_
================================
:math:`e` is a special number. The derivative of exponential :math:`e^t` is always :math:`e^t`. Let's look at non-:math:`e` exponentail :math:`2^t` and its derivative.

.. math::
	\begin{align}
	\frac{\delta}{\delta t} 2^t &= \lim_{\delta t \rightarrow 0} \frac{2^{t+\delta t} - 2^t }{\delta t} \\
	&= 2^t \big( \lim_{\delta t \rightarrow 0} \frac{2^{\delta t} - 1 }{\delta t} \big)
	\end{align}

.. code-block:: python

	t= .1
	for i in range(1, 10):
	    dt = t ** i
	    d = (2**(dt) - 1 )/(dt)
	    print(dt, d)

		# 0.1 0.7177346253629313
		# 0.010000000000000002 0.6955550056718883
		# 0.0010000000000000002 0.6933874625807411
		# 0.00010000000000000002 0.6931712037649972
		# 1.0000000000000003e-05 0.6931495828199628
		# 1.0000000000000004e-06 0.6931474207938491
		# 1.0000000000000004e-07 0.6931472040783146
		# 1.0000000000000005e-08 0.6931471840943001
 		# 1.0000000000000005e-09 0.6931470952764581

So we see that as :math:`\delta t` becomes finitely small, :math:`\frac{\delta}{\delta t} 2^t` converges to 0.693. It's good to know that it converges but it would be handy if we can find a pattern and that's where :math:`e` comes in!

.. math::
	\text{Let's rewrite } 2^t

.. math::
	\begin{align}
	2^t &=  e^{t \log2} \quad \text{(as $e$ and 2 can be exchanged)} \\
	\frac{\delta}{\delta t} 2^t &= \frac{\delta}{\delta t} e^{t \log2} \\
	&= \log 2 e^{t \log2} \quad \text{(as $\frac{\delta}{\delta t} e^{at} = ae^{at}$)} \\
	(\log 2 &= 0.6931471805599453)
	\end{align}

So we got the same results by using :math:`e` and it is more universal as we can easily plug any constants and get the derivative using logarithm! 


-----------------------------------

.. rubric:: Reference

.. [1]  https://youtu.be/m2MIpDrF7Es