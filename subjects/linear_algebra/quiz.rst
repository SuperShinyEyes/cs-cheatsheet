====
Quiz
====

1. Differentiate :math:`f(\mathbf{x})=\mathbf{x}^{T}\mathbf{A}\mathbf{x}`
-------------------------------------------------------------------------
Consider :math:`f(\mathbf{x})=\mathbf{x}^{T}\mathbf{A}\mathbf{x}`, where :math:`\mathbf{x} \in \mathbb{R}^{n}` and :math:`\mathbf{A} \in \mathbb{R}^{n \times n}`. Show that for a symmetric matrix :math:`\mathbf{A}^{T} =\mathbf{A}`, the gradient of :math:`f(\mathbf{x})` is given as :math:`2\mathbf{A} \mathbf{x}`.

`Solution <solution_vector_differential_>`_
###########################################

.. _solution_vector_differential: https://math.stackexchange.com/a/312271

Let :math:`\mathbf{x}^{n\times 1}=(x_1,\dots ,x_n)'` be a vector, the derivative of :math:`\mathbf y=f(\mathbf x)` with respect to the vector :math:`\mathbf{x}` is defined by 

.. math:: 
  \frac{\partial f}{\partial \mathbf x}=\begin{pmatrix} \frac{\partial f}{\partial  x_1}  \\ \vdots\\ \frac{\partial f}{\partial  x_n} \end{pmatrix}

Let 

.. math:: 
  \begin{align}
    \mathbf y&=f(\mathbf x)\\&=\mathbf x'A\mathbf x \\&=\sum_{i=1}^n\sum_{j=1}^n a_{ij}x_ix_j\\&=\sum_{i=1}^na_{i1}x_ix_1+\sum_{j=1}^na_{1j}x_1x_j+\sum_{i=2}^n\sum_{j=2}^n a_{ij}x_ix_j
    \\\frac{\partial f}{\partial  x_1} &=\sum_{i=1}^na_{i1}x_i+\sum_{j=1}^na_{1j}x_j\\&=\sum_{i=1}^na_{1i}x_i+\sum_{i=1}^na_{1i}x_i \,[\text{since}\,\, a_{1i}=a_{1i}]\\ &=2 \sum_{i=1}^na_{1i}x_i
    \\ \frac{\partial f}{\partial \mathbf x}&=\begin{pmatrix} 2 \sum_{i=1}^na_{1i}x_i \\ \vdots\\ 2 \sum_{i=1}^na_{ni}x_i \end{pmatrix} \\&=2\begin{pmatrix} a_{11} & a_{12} & \dots & a_{1n}\\  \vdots & \vdots &\ddots & \vdots \\ a_{11} & a_{12} & \dots & a_{1n} \end{pmatrix}\begin{pmatrix}x_1  \\ \vdots \\ x_n \end{pmatrix}\\ &= 2A\mathbf x
  \end{align}