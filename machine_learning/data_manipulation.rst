=================
Data Manipulation
=================

Feature Scaling
###############

Make sure features are on a similar scale such as :math:`-1 \leq x_i \leq 1`. For instance,

.. math::
      x_1 &= \text{size}, & \text{(0 to 2000 meters)} 

      x_2 &= \text{number of bedrooms}, & \text{(1 to 5 rooms)}

Rescale:

.. math::
      x_1 &= \frac{\text{size}}{2000}

      x_2 &= \frac{\text{number of bedrooms}}{5}
