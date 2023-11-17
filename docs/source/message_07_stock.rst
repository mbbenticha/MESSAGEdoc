Chapter 7
=========

Stock-piles
-----------

7.1 Variables
-------------

Generally, MESSAGE does not generate any variables related to an energy carrier alone. However, in the case of man-made fuels that are accumulated over time, a variable that shifts the quantities to be transferred from one period to the other is necessary.

7.1.1 Stock-pile Variables
^^^^^^^^^^^^^^^^^^^^^^^^^^

.. math::

    Q_{f,b,t},

where:

- :math:`Q` identifies stock-pile variables,
- :math:`f` identifies the fuel with stock-pile,
- :math:`b` distinguishes the variable from the equation, and
- :math:`t` is the period identifier.

The stock-pile variables represent the amount of fuel :math:`f` that is transferred from period :math:`t` into period :math:`t + 1`. Note that these variables do not represent annual quantities, they refer to the period as a whole. These variables are a special type of storage, that just transfers the quantity of an energy carrier available in one period into the next period. Stock-piles are defined as a separate level. For all other energy carriers, any overproduction that occurs in a period is lost.

7.2 Constraints
---------------

7.2.1 Stock-piling Constraints
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. math::

    Q_{f,...}

.. note::

    Q is a special level on that energy forms can be defined that are accumulated over time and consumed in later periods. One example is the accumulation of plutonium and later use in fast breeder reactors.

The general form of this constraint is:

.. math::

    Q_{f,b,t} = Q_{f,b,t-1} + \sum_{v} \Delta t \times (s_{f,v,d,t} + \beta_{f,v} \times x_{f,v,d,t} - \epsilon_{f,v} \times z_{s,v,f,u,l,t} - \phi_{f,v} \times z_{s,v,d,l,t}) + \Delta t \times \ell(s_{u,d,f}) \times Y_{z,s,v,d,t+1} - (\Delta t - T_{s,v,d}) \times p(s_{v,d,f}) \times Y_{z,s,v,d,t-T_{s,v,d}} = 0,

where:

- :math:`f` is the identifier of the man-made fuel (e.g., plutonium, U233),
- :math:`T_{s,v,d}` is the plant life of technology :math:`v` in periods,
- :math:`\ell(s_{u,d,f})` is the "first inventory” of technology :math:`v` of :math:`f` (relative to capacity of main output),
- :math:`p(s_{u,d,f})` is the "last core” of :math:`f` in technology :math:`v`, see also section 6.1.5,
- :math:`\Delta t` is the length of period :math:`t` in years,
- :math:`s_{f,v,d,t}` is the annual input of technology :math:`v` of fuel :math:`f` in load region :math:`l` and period :math:`t` (if :math:`v` does not have load regions), and
- :math:`Y_{z,f,v,d,t}` is the annual new installation of technology :math:`v` in period :math:`t`.


