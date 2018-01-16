=================
Fundamentals
=================

Basis
=====
It's a set of vectors in a vector space which are linearly independent. All vectors in the vector space are linear combinations of the basis.
Read `wiki <wiki_basis_>`_.

.. _wiki_basis: https://en.wikipedia.org/wiki/Basis_(linear_algebra)


Eigenvector & Eigenvalue
==========================
Intuitively, eigenvectors and eigenvalues are related to transformation. When a transformation is applied to a vector space, veectors span. While most vectors drift away from their spans some remain on its own span – eigenvectors. Eigenvectors remain on its own span after a transformation but they may scale – by a factor of their eigenvalues. An interesting fact is that any vector that lies on the same span as eigenvectors is itself an eigenvector. Therefore **there can be infinitely many eigenvectors**. However, **there could be only one eigenvector as well**. Consider a 3D transformation matrix :math:`A`:

.. math::
    \begin{align}
   A &= 
   \begin{bmatrix}
       0       & 0 & 0 \\
       0       & 0 & 0 \\
       0       & 0 & 0
   \end{bmatrix}
   \end{align}

:math:`A` will squash everything into *null* and there will be only one eigenvector :math:`\begin{bmatrix} 0 \\ 0 \\ 0 \end{bmatrix}`

Eigenvector in 3D rotations
^^^^^^^^^^^^^^^^^^^^^^^^^^^
An eigenvectors in a 3D rotation means the **axis of a rotation**. And because a rotation doesn't change the scale, the **eigenvalue should be 1**. 


Eigenvector in 2D rotations
^^^^^^^^^^^^^^^^^^^^^^^^^^^
There is no eigenvector in 2D rotations. No eigenvector implies no real-valued eigenvalue but imaginary-valued.

Single eigenvalue with multiple eigenvectors
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
There could be multiple eigenvectors but only one single eigenvalue. Consider a transformation :math:`A`:

.. math::
    \begin{align}
   A &= 
   \begin{bmatrix}
       2       & 0 \\
       0       & 2
   \end{bmatrix}
   \end{align}

:math:`A` scales every eigenvector by 2 and only 2.

Calculation
^^^^^^^^^^^
.. math::
	\overbrace{
	  A \vec{v}
	}^\text{Matrix-vector multiplication}
	= \underbrace{\lambda \vec{v}}_\text{Scalar multiplication}
   
The matrix :math:`A` changes only the scale of the vector :math:`\vec{v}` by a factor of :math:`\lambda`. We could rewrite the right hand as 

.. math::
   \begin{align}
    	\lambda \vec{v} &= 
      	\begin{bmatrix}
					\lambda 	& 0 			& 0 \\
					0 				& \lambda & 0 \\
					0 				& 0 			& \lambda
    		\end{bmatrix} \vec{v} \\
    	&= \lambda
    	  \begin{bmatrix}
					1 	& 0 			& 0 \\
					0 				& 1 & 0 \\
					0 				& 0 			& 1
    		\end{bmatrix} \vec{v} \\
    	&= \lambda I \vec{v}
   \end{align}

To get the value of the eigenvalue, take the righthand to the other side:

.. math::
  (A - \lambda I) \vec{v} = \vec{0}

Remember the squashification? :math:`(A - \lambda I)` is squashing :math:`\vec{v}`. This implies the following:

.. math::
  \det{(A - \lambda I)} = 0


Eigenbasis
^^^^^^^^^^
Consider a 2D vectorspace. If both basis vectors are eigenvectors then its transformation matrix would be diagonal. Here's step-by-step:

A typical set of eigen vectors in 2D:

.. math::
	\begin{bmatrix}
	   1     \\
	   0       
	\end{bmatrix},
	\begin{bmatrix}
	   0 \\
	   1
	\end{bmatrix}

A transformation matrix:

.. math::
   \begin{bmatrix}
       -1       & 0 \\
       0       & 2
   \end{bmatrix}

The columns of the transformation matrix happnes to be the eigenvectors and, **the basis vectors as well** with the diagonal values being their eigenvalues. Pay attention to the matrix that the matrix is diagonal. Diagonal matrices have a handy property – their power is just a power of the elements:

.. math::

   \begin{bmatrix}
       -1       & 0 \\
       0       & 2
   \end{bmatrix}^n =
   \begin{bmatrix}
       (-1)^n       & 0 \\
       0       & 2^n
   \end{bmatrix}
   

Eigenbasis for easier power
^^^^^^^^^^^^^^^^^^^^^^^^^^^
Consider a transformation :math:`A`,

.. math::

   \begin{bmatrix}
       3       & 1 \\
       0       & 2
   \end{bmatrix}

:math:`A` is not diagonal so its power will be expensive to calculate. Let's change its basis as the eigenvectors **in order to make** :math:`A` **diagonal**. The eigenvectors of :math:`A`,

.. math::
	\begin{bmatrix}
	   1     \\
	   0       
	\end{bmatrix},
	\begin{bmatrix}
	   -1 \\
	   1
	\end{bmatrix}

We can build **"Change of basis matrix"** from the eigenvectors,

.. math::
   \begin{bmatrix}
       1       & -1 \\
       0       & 1
   \end{bmatrix}

Now let's change its basis,

.. math::
	\begin{bmatrix}
	   \text{Change of basis matrix}^{-1}
	\end{bmatrix}
	A
	\begin{bmatrix}
	   \text{Change of basis matrix}
	\end{bmatrix} \\
	\Rightarrow
  \begin{bmatrix}
       1       & -1 \\
       0       & 1
   \end{bmatrix}^{-1}
  \begin{bmatrix}
       3       & 1 \\
       0       & 2
   \end{bmatrix}	
  \begin{bmatrix}
       1       & -1 \\
       0       & 1
   \end{bmatrix} \\
  = 
  \begin{bmatrix}
       3       & 0 \\
       0       & 2
   \end{bmatrix}	  
