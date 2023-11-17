Chapter 3
=========
Storage Technologies
--------------------

Variables
~~~~~~~~~

Energy storage technologies are modeled with two types of flow variables, and one or two types of capacity variables.

The energy flows in a storage go in two directions: into and out of the storage facility. These two types of flows are represented by two types of activity variables, the input and the output variables of storage technologies.

The capacity variables represent the capacity for energy input and output and the volume capacity of the technology. In this case the relation between I/O capacity and volume capacity is determined by the optimization. Alternatively the relation can be predefined, no volume capacity variables are generated.

Input to Energy Storage Technologies
------------------------------------

.. math::

    SI_{szvlt},

where

.. list-table::
   :widths: 5 15
   :header-rows: 1

   * - Variable
     - Description
   * - :math:`SI`
     - identifies storage input variables,
   * - :math:`s`
     - is the identifier of the energy carrier to be stored,
   * - :math:`z`
     - identifies the level on that the energy carrier is defined,
   * - :math:`v`
     - is an additional identifier of the storage technology (this is necessary in the case that several storage technologies for the same energy carrier are defined),
   * - :math:`l`
     - identifies the load region in that the energy is put into storage, and
   * - :math:`t`
     - is the period identifier.

The storage input variables are energy flow variables and represent the amount of fuel s that is stored in storage with identifier v in load region l and period t (if storing energy entails energy losses, the variable represents the amount before the losses apply, i.e. the amount used for storing).

Output from Energy Storage Technologies
---------------------------------------

.. math::

    O_{szvlt},

where

.. list-table::
   :widths: 5 15
   :header-rows: 1

   * - Variable
     - Description
   * - :math:`O`
     - identifies storage output variables,
   * - :math:`s`
     - is the identifier of energy carrier stored in the technology,
   * - :math:`z`
     - identifies the level on that the energy carrier is defined,
   * - :math:`v`
     - is an additional identifier of the storage technology,
   * - :math:`l`
     - is the load region in that the energy was put into storage,
   * - :math:`m`
     - is the load region in that the energy is retrieved, and
   * - :math:`t`
     - is the period identifier.

The storage output variables are energy flow variables and represent the amount of fuel s that was put into storage in load region l and is retrieved in load region m (if retrieving energy entails energy losses, the variable represents the amount before the losses apply, i.e. the amount by that the contents of the storage is reduced).

Input/Output Capacity
---------------------

.. math::

    Y^{G}_{szut},

where

.. list-table::
   :widths: 5 15
   :header-rows: 1

   * - Variable
     - Description
   * - :math:`Y`
     - identifies the capacity variables,
   * - :math:`z`
     - is the level on that this energy form is defined,
   * - :math:`G`
     - identifies the I/O capacity variables for storage technologies (generation capacity),
   * - :math:`s`
     - is the identifier of energy carrier stored in the technology,
   * - :math:`u`
     - is an additional identifier of the storage technology, and
   * - :math:`t`
     - is the period in that the new capacity is built.

The I/O capacity variables of storage technologies are power variables and represent the annual construction of capacity to fill and empty the storage. They can be defined continuous or integer like the capacity variables of conversion technologies.

Volume Capacity
---------------

.. math::

    Y^{V}_{szt},

where

.. list-table::
   :widths: 5 15
   :header-rows: 1

   * - Variable
     - Description
   * - :math:`Y`
     - identifies the capacity variables,
   * - :math:`z`
     - is the level on that this energy form is defined,
   * - :math:`V`
     - identifies the volume capacity variables for storage technologies,
   * - :math:`s`
     - is the identifier of energy carrier stored in the technology.

Constraints
-----------

The flows into and out of a storage technology are balanced by the storage balance constraint.

The input/output capacity of the storage is directly determined from the energy flows in the I/O capacity constraint. The volume capacity is calculated from these variables and either used to determine the volume capacity variables or once more the I/O capacity variables via the relation between volume and I/O capacity.

Storage Balance Constraint
--------------------------

.. math::

    S_{szu..t}

The volume capacity variables of storage technologies are stock-pile variables and have an energy- and not power-related unit. They represent the annual new installation of the "container". They can be defined continuous or integer like the other capacity variables. If the relation between I/O and volume capacity in a storage system is predefined, the volume capacity is represented by the I/O capacity with this relation as a coefficient.

Section 10.5 describes the background of the implementation of energy storage in MESSAGE. In the storage balance constraints the energy flows into and out of the storage technologies are balanced. MESSAGE keeps track of the time that a certain amount is kept in storage by using a separate storage output variable for each pair of input and output load regions. In the following two examples are given; the equations differ for different kinds of storage (e.g., daily, weekly, seasonal).

Daily Storage
-------------

The energy can only be balanced over the load regions of one day, not between the seasons.

.. math::

    c_{su} \times SI_{szult} - \sum_{m=l+1}^{l+n} \frac{1}{\eta_{l,m}} \times O_{szumt} \geq 0,

Seasonal Storage
----------------

The storage can be used for all types of load defined in the model, since the season is the highest category. An adequate amount of the energy stored at the end of the year is put forward to the next period.

.. math::

    c_{su} \times SI_{szult} - \sum_{m=l+1}^{l+n_{y}} \frac{f^{1}_{l,m}}{\eta_{l,m}} \times O_{szumlt} - \sum_{m=l+1}^{l+n_{y}} f^{2}_{l,m} \times \frac{1}{\eta_{l,m}} \times O_{szumlt}(t + 1) \geq 0,

In the above equations the following notation is used:

.. list-table::
   :widths: 5 15
   :header-rows: 1

   * - Notation
     - Meaning
   * - :math:`f_{l,m}`
     - forwards the appropriate amount of fuel to the next period (this is important for small time steps, for instance :math:`\Delta t = 1`),
   * - :math:`f^{1}_{l,m}`
     - :math:`\begin{cases} 1 & \text{for } l < m \\ \frac{1}{\Delta t} & \text{for } l \geq m \end{cases}`
   * - :math:`f^{2}_{l,m}`
     - :math:`\begin{cases} 0 & \text{for } l < m \\ \frac{1}{(\Delta t + 1)} & \text{for } l \geq m \end{cases}`
   * - :math:`SI_{szult}`
     - is the amount of fuel s put into storage in load region l,
   * - :math:`O_{szumlt}`
     - is the amount of fuel s taken out of storage in load region m,
   * - :math:`c_{su}`
     - is the efficiency of putting fuel s into storage v (e.g. the pumping losses in pumped hydro storage plants can be accounted for this way),
   * - :math:`n_{y}`
     - is the number of load regions that the fuel can be stored. It depends on the kind of storage (for daily storage it is the number of load regions that represent one day, for seasonal storage the whole year, therefore all load regions) and if there is an explicit limit given (e.g., the temperature inside a heat storage can fall below the level where it still can be retrieved after a certain time),
   * - :math:`\eta_{l,m}`
     - is the decrease of storage contents from load region l to load region m, used for heat storage (exponential decay), and
   * - :math:`\Delta t`
     - is the length of period t in years.

Input/Output Capacity
---------------------

.. math::

    C^{G}_{su..t}

This equation defines the capacity of storing or releasing energy per unit of time in a certain storage technology.

.. math::

    \frac{c_{su}}{\lambda_{l}} \times \left[ SI_{szult} + \sum_{m=l-n_{su}}^{l-1} O_{szumlt} \right] - \min(t_{su},t) \sum_{\tau=t_{su}}^{t} \pi_{su} \times (\Delta \tau - 1) \times f_{i} \times Y^{G}_{su..t} - h^{d,fg}_{su,G} \times \tau ,

where

.. list-table::
   :widths: 5 15
   :header-rows: 1

   * - Notation
     - Meaning
   * - :math:`SI_{szult}`
     - and :math:`O_{szumlt}` are the flows into and out of the storage technology v, as described in sections 3.1.1 and 3.1.2
   * - :math:`Y^{G}_{su..t}`
     - is the generation capacity of storage v as described in section 3.1.3
   * - :math:`c_{su}`
     - is the efficiency of storage technology v,
   * - :math:`\lambda_{l}`
     - is the length of load region l as fraction of the year,
   * - :math:`n_{su}`
     - is the last period in that technology v can be constructed,
   * - :math:`\pi_{su}`
     - is the plant factor of technology v,
   * - :math:`\Delta \tau`
     - is the length of period t − 1 in periods,
   * - :math:`\tau_{su}`
     - is the plant life of technology v in years,
   * - :math:`h^{d,fg}_{su,G}`
     - represents the installations built before the time horizon under consideration, that are still in operation in period t,
   * - :math:`f_{i}`
     - is 1 if the capacity variable is continuous, and equal to the minimum installed capacity per year (unit size) if the variable is integer.

Volume Capacity
---------------

.. math::

    C^{V}_{su..t}

The amount of energy that can be stored (the maximum content at a time) can either be linked to the I/O capacity or evaluated during optimization. Thus either a predefined storage technology like batteries can be modelled or the model can have the choice to optimize the relation between I/O capacity and storage volume.

.. math::

    \min \left( \sum_{m=l-n_{b}}^{l} c_{sm,l} \times \left[ c_{su} \times SI_{szumlt} - \sum_{n=m+1}^{m+n_{y}} \frac{1}{\eta_{m,n}} \times O_{szumnt} \right] \right)

.. math::

    \frac{1}{n_{l} \times \lambda_{l}} \times \left[ \min(t_{su},t) \sum_{\tau=t_{su}}^{t} \Delta (\tau - 1) \times \pi_{su} \times f_{i} \times Y^{V}_{su..t} - h^{d,fg}_{su,V} \times \tau \right],

where

.. list-table::
   :widths: 5 15
   :header-rows: 1

   * - Notation
     - Meaning
   * - :math:`SI_{szult}`
     - and :math:`O_{szumlt}` are the flows into and out of the storage technology v, as described in sections 3.1.1 and 3.1.2
   * - :math:`Y^{G}_{su..t}`
     - is the generation capacity of storage v as described in section 3.1.3,
   * - :math:`f_{gv}`
     - is the relation of I/O to volume capacity,
   * - :math:`Y^{V}_{su..t}`
     - is the volume capacity of storage as described in section 3.1.4,
   * - :math:`n_{l}`
     - is the number of occurrences per year (1 for seasonal, 365 for daily, etc.),
   * - :math:`h^{d,fg}_{su,V}`
     - represents the installations built before the time horizon under consideration, that are still in operation in period t,
   * - :math:`f_{i}`
     - is 1 if the capacity variable is continuous, equal to the minimum installed capacity per year (unit size) if the variable is integer,
   * - :math:`\Delta \tau`
     - is the decrease parameter as described in section 10.5,
   * - :math:`\pi_{su}`
     - is described in section 10.5.

