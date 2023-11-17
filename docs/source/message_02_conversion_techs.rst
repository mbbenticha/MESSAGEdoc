Chapter 2
=========

Conversion Technologies
-----------------------

2.1 Variables
-------------

Energy conversion technologies are modelled using two types of variables, that represent:

- the amount of energy converted per year in a period (activity variables)
- the capacity installed annually in a period (capacity variables).

2.1.1 Activities of Energy Conversion Technologies
--------------------------------------------------

The activity variable of an energy conversion technology is an energy flow variable. It represents the annual consumption of this technology of the main input per period. If a technology has no input, the variable represents the annual production of the main output.

.. list-table::
   :header-rows: 1
   :widths: 10 30

   * - Variable
     - Description
   * - **z**
     - The level identifier of the main output of the technology.
   * - **z = U**
     - Identifies the end-use level. This level is handled differently from all other levels; it must be the demand level and technologies with the main output on this level are defined without load regions.
   * - **s**
     - The main energy input of the technology (supply). If the technology has no input, s is set to "." (e.g., solar technologies).
   * - **v**
     - Additional identifier of the conversion technology (used to distinguish technologies with the same input and output).
   * - **d**
     - The main energy output of the technology (demand).
   * - **e**
     - The level of reduction of demand due to own-price elasticities of demands (occurs only on the demand level; otherwise, or if this demand has no elasticities, e is set to ".").
   * - **l**
     - Identifies the load region, l ∈ {1,2,3,...} or l = "." if the technology is not modelled with load regions.
   * - **t**
     - Identifies the period, t ∈ {a,b,c,...}.

.. note:: If the level of the main output is not U and at least one of the energy carriers consumed or supplied is defined with load regions, the technology is defined with load regions. In this case, the activity variables are generated separately for each load region, which is indicated by the additional identifier l in position 7. However, this can be changed by fixing the production of the technology over the load regions to a predefined pattern (see section 10.4): one variable is generated for all load regions, and the distribution to the load regions is given by the definition of the user (e.g., production pattern of solar power-plants).

.. note:: If the model is formulated with demand elasticities (see section 10.10), the activity variables of technologies with a demand as main output that is defined with elasticity are generated for each elasticity class (identifier e in position 6).

2.1.2 Capacities of Energy Conversion Technologies
--------------------------------------------------

.. code-block:: none

    Yzsvd.t

where

.. list-table::
   :header-rows: 1
   :widths: 10 30

   * - Variable
     - Description
   * - **Y**
     - The identifier for capacity variables.
   * - **z**
     - Identifies the level on which the main energy output of the technology is defined.
   * - **s**
     - The identifier of the main energy input of the technology.
   * - **v**
     - Additional identifier of the conversion technology.
   * - **d**
     - The identifier of the main energy output of the technology.
   * - **t**
     - The period in which the capacity goes into operation.

The capacity variables are power variables. Technologies can be modeled without capacity variables. In this case, no capacity constraints and no dynamic constraints on construction can be included in the model. Capacity variables of energy conversion technologies can be defined as integer variables if the solution algorithm has a mixed integer option.

If a capacity variable is continuous, it represents the annual new installations of the technology in period t. If it is integer, it represents either the annual number of installations of a certain size or the number of installations of 1/Δt times the unit size (depending on the definition; Δt is the length of period t in years).

The capacity is defined in relation to the main output of the technology.

2.2 Constraints
---------------

The rows used to model energy conversion technologies limit:

- the utilization of a technology in relation to the capacity actually installed (capacity constraint), and
- the activity or construction of a technology in a period in relation to the same variable in the previous period (dynamic constraints).

2.2.1 Capacity Constraints
-------------------------

.. code-block:: none

    Czsvd.lt,

where

.. list-table::
   :header-rows: 1
   :widths: 10 30

   * - Variable
     - Description
   * - **C**
     - The identifier for capacity constraints.
   * - **z**
     - Identifies the level on which the main energy output of the technology is defined.
   * - **s**
     - The identifier of the main energy input of the technology.
   * - **v**
     - Additional identifier of the conversion technology.
   * - **d**
     - The identifier of the main energy output of the technology.
   * - **l**
     - Identifies the load region, l ∈ {1,2,3,...} or l = "." if the technology is not modelled with load regions.
   * - **t**
     - The period in which the capacity goes into operation.

For all conversion technologies modelled with capacity variables, the capacity constraints will be generated automatically. If the activity variables exist for each load region separately, there will be one capacity constraint per load region (see also section 10.4). If the technology is an end-use technology, the sum over the elasticity classes will be included in the capacity constraint.

Additionally, the activity variables of different technologies can be linked to the same capacity variable, which allows leaving the choice of the activity variable used with a given capacity to the optimization (see section 10.7).

Technologies without Load Regions
---------------------------------

For technologies without load regions (i.e., technologies where no input or output is modelled with load regions), the production is related to the total installed capacity by the plant factor. For these technologies, the plant factor has to be given as the fraction they actually operate per year. All end-use technologies (technologies with main output level "U") are modelled in this way.

.. math::

   e_{svd} \times s_{svd.t} - \sum_{\tau=T_{start}}^{t-1} \left( \Delta \tau \times f_{svd} \times X_{svd.t} \right) \leq h_{svd}^{cap} \times f_{svd} \times X_{svd.t}

Technologies with Load Regions and "Free" Production Pattern
-----------------------------------------------------------

If a technology has at least one input or output with load regions, the activity variables and capacity constraints will per default be generated separately for each load region. This can be changed by defining the production pattern over the load regions. If the production pattern remains free, the production in each load region is limited in relation to the installed capacity separately for each load region. The capacity is determined by the activity in the load region with the highest requirements. The plant factor has to be given as the fraction the system operates in peak operation mode (in general this is the availability factor).

.. note:: Maintenance times or minimum operation times could be included by using additional relations, if required (see section 8).

Technologies with Load Regions and "Fixed" Production Pattern
-------------------------------------------------------------

If a technology has at least one input or output with load regions and the production pattern over the load regions is predefined, only one activity variable and one capacity constraint is generated per period. The plant factor must be given for the load region with the highest capacity utilization (i.e., the highest power requirement). The capacity constraint is generated for only this load region.

.. math::

   e_{svd} \times \left( \frac{T_{ls,svd}}{\Delta m} \right) \times X_{svd.t} - \sum_{\tau=T_{start}}^{t-1} \Delta (\tau - 1) \times f_{svd} \times X_{svd.\tau} \leq h_{svd}^{cap} \times f_{svd} \times X_{svd.t}

Technologies with Varying Inputs and Outputs
--------------------------------------------

Many types of energy conversion technologies do not have fixed relations between their inputs and outputs. MESSAGE has the option to link several activity variables of conversion technologies into one capacity constraint. For the additional activities linked to a capacity variable, a coefficient defines the maximum power available in relation to one power unit of the main activity.

In the following, this constraint is only described for technologies without load regions; the other types are constructed in analogy (see also section 10.7).

.. math::

   \sum_{c/s} e_{resv,d}^{c/s} \times C_{var}^{c/s} \times X_{c/sv,d.t} - \sum_{\tau=T_{start}}^{t-1} \Delta (\tau - 1) \times f_{svd} \times X_{svd.\tau} \leq h_{svd}^{cap} \times f_{svd} \times X_{svd.t}

The following notation is used in the above equations:

.. list-table::
   :header-rows: 1
   :widths: 10 30

   * - Variable
     - Description
   * - **x_{svd.t}**
     - The activity of conversion technology v in period t and, if defined so, load region l (see section 2.1.1).
   * - **Y_{svd.t}**
     - The capacity variable of conversion technology v (see section 2.1.2).
   * - **\eta_{svd}**
     - The efficiency of technology v in converting the main energy input, s, into the main energy output, d.
   * - **T_{svd}**
     - The last period in which technology v can be constructed.
   * - **f_{svd}**
     - The "plant factor" of technology v, having different meanings depending on the type of capacity equation applied.
   * - **\Delta \tau**
     - The length of period t in years.
   * - **T_{svd}**
     - The plant life of technology v in periods.
   * - **h^{old}_{svd}**
     - Represents the installations built before the time horizon under consideration that are still in operation in the first year of period t.
   * - **f_i**
     - Is 1 if the capacity variable is continuous, and represents the minimum installed capacity per year (unit size) if the variable is integer.
   * - **l_m**
     - The load region with maximum capacity use if the production pattern over the year is fixed.
   * - **\pi(l_{m,svd})**
     - The share of output in the load region with maximum production.
   * - **\pi(rel_{svd})**
     - The relative capacity of main output of technology (or operation mode) svd to the capacity of main output of the alternative technology (or operation mode)ou'6.
   * - **\lambda_{l}**
     - The length of load region l as a fraction of the year.
   * - **\lambda_{l_m}**
     - The length of load region l_m, the load region with maximum capacity requirements, as a fraction of the year.

2.2.2 Upper Dynamic Constraints on Construction Variables
--------------------------------------------------------

.. code-block:: none

    MY_{svd.t}

The dynamic capacity constraints relate the amount of annual new installations of a technology in a period to the annual construction during the previous period.

.. math::

   Y_{svd.t} - \gamma^{max}_{svd,t} \times Y_{svd.(t-1)} \leq g^{init}_{svd,t}

where

.. list-table::
   :header-rows: 1
   :widths: 10 30

   * - Variable
     - Description
   * - **\gamma^{max}_{svd,t}**
     - The maximum growth rate per period for the construction of technology v.
   * - **g^{init}_{svd,t}**
     - The initial size (increment) that can be given for the introduction of new technologies.
   * - **Y_{svd.t}**
     - The annual new installation of technology v in period t.

2.2.3 Lower Dynamic Constraints on Construction Variables
--------------------------------------------------------

.. code-block:: none

    LY_{svd.t}

.. math::

    Y_{svd.t} - \gamma^{min}_{svd,t} \times Y_{svd.(t-1)} \geq -g^{end}_{svd,t}

where

.. list-table::
   :header-rows: 1
   :widths: 10 30

   * - Variable
     - Description
   * - **\gamma^{min}_{svd,t}**
     - The minimum growth rate per period for the construction of technology v.
   * - **g^{end}_{svd,t}**
     - The “last” size (decrement) allowing technologies to go out of the market.
   * - **Y_{svd.t}**
     - The annual new installation of technology v in period t.

2.2.4 Upper Dynamic Constraints on Activity Variables
-----------------------------------------------------

.. code-block:: none

    MU_{svd.t}

The dynamic production constraints relate the production of a technology in one period to the production in the previous period. If the technology is defined with load regions, the sum over the load regions is included in the constraint.

.. math::

    \sum_{l} e_{svd} \times [ x_{svd.l.t} - \gamma^{max}_{svd,t} \times x_{svd.l.(t-1)} ] < g^{max}_{svd,t}

where

.. list-table::
   :header-rows: 1
   :widths: 10 30

   * - Variable
     - Description
   * - **\gamma^{max}_{svd,t}** and **g^{max}_{svd,t}**
     - The maximum growth rate and increment as described in section 2.2.2 (the increment is to be given in units of main output).
   * - **x_{svd.l.t}**
     - The activity of technology v in load region l.

If demand elasticities are modelled, the required sums are included for end-use technologies.

2.2.5 Lower Dynamic Constraints on Activity Variables
-----------------------------------------------------

.. code-block:: none

    ML_{svd.t}

.. math::

    \sum_{l} e_{svd} \times [ x_{svd.l.t} - \gamma^{min}_{svd,t} \times x_{svd.l.(t-1)} ] \geq -g^{min}_{svd,t}

where

.. list-table::
   :header-rows: 1
   :widths: 10 30

   * - Variable
     - Description
   * - **\gamma^{min}_{svd,t}** and **g^{min}_{svd,t}**
     - The minimum growth rate and increment as described in section 2.2.3.
   * - **x_{svd.l.t}**
     - The activity of technology v in load region l.
