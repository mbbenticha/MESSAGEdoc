Chapter 2
==========

Conversion Technologies
-----------------------

2.1 Variables
-------------

Energy conversion technologies are modelled using two types of variables, that represent:

- the amount of energy converted per year in a period (activity variables)
- the capacity installed annually in a period (capacity variables).

2.1.1 Activities of Energy Conversion Technologies
--------------------------------------------------

.. math::

    z_{s,v,d,e,l,t}

where:

.. list-table::
   :widths: 25 75
   :header-rows: 0

   * - ``z``
     - is the level identifier of the main output of the technology.
   * - ``z = U``
     - identifies the end-use level. This level is handled differently to all other levels: It has to be the demand level and technologies with the main output on this level are defined without load regions.
   * - ``s``
     - is the main energy input of the technology (supply). If the technology has no input, s is set to "`.`" (e.g., solar technologies),
   * - ``v``
     - additional identifier of the conversion technology (used to distinguish technologies with the same input and output),
   * - ``d``
     - is the main energy output of the technology (demand),
   * - ``e``
     - is the level of reduction of demand due to own-price elasticities of demands (does only occur on the demand level; otherwise or if this demand has no elasticities e = "`.`"),
   * - ``l``
     - identifies the load region, l ∈ {1,2,3,...} or l = "`.`", if the technology is not modeled with load regions, and
   * - ``t``
     - identifies the period, t ∈ {a,b,c,...}.

The activity variable of an energy conversion technology is an energy flow variable. It represents the annual consumption of this technology of the main input per period. If a technology has no input, the variable represents the annual production of the main output.

If the level of the main output is not U and and at least one of the energy carriers consumed or
supplied is defined with load regions the technology is defined with load regions. In this case
the activity variables are generated separately for each load region, which is indicated by the
additional identifier lin position 7. However, this can be changed by fixing the production of
the technology over the load regions to a predefined pattern (see section 10.4): one variable is
generated for all load regions, the distribution to the load regions is given by the definition of
the user (e.g., production pattern of solar power-plants).

If the model is formulated with demand elasticities (see section 10.10), the activity variables
of technologies with a demand as main output that is defined with elasticity are generated for
each elasticity class (identifier e in position 6).



2.1.2 Capacities of Energy Conversion Technologies
-----------------------------------------------------------


Y_{s,v,d,t}

is the identifier for capacity variables.

identifies the level on that the main energy output of the technology is defined,
is the identifier of the main energy input of the technology,

additional identifier of the conversion technology,

is the identifier of the main energy output of the technology, and

is the period in that the capacity goes into operation.



The capacity variables are power variables. Technologies can be modelled without capacity
variables. In this case no capacity constraints and no dynamic constraints on construction
can be included in the model. Capacity variables of energy conversion technologies can be
defined as integer variables, if the solution algorithm has a mixed integer option.

Ifa capacity variable is continuous it represents the annual new installations of the
technology in period t, if it is integer it represents either the annual number of installations of
a certain size or the number of installations of 1/At times the unit size (depending on the
definition; At is the length of period in years).

The capacity is defined in relation to the main output of the technology.


2.2 Constraints
----------------

The rows used to model energy conversion technologies limit,
- the utilization of a technology in relation to the capacity actually installed (capacity

constraint) and

- the activity or construction of a technology in a period in relation to the same variable
in the previous period (dynamic constraints).



2.2.1 Capacity Constraints
----------------------------


C_{s,v,d,l,t}

is the identifier for capacity constraints,

identifies the level on that the main energy output of the technology is defined,
is the identifier of the main energy input of the technology,

additional identifier of the conversion technology,

is the identifier of the main energy output of the technology,

identifies the load region, l in {1,2,3,...} or l = ., if the technology is not
modelled with load regions, and

is the period in that the capacity goes into operation.



 

For all conversion technologies modelled with capacity variables the capacity constraints will
be generated automatically. If the activity variables exist for each load region separately
there will be one capacity constraint per load region (see also section 10.4). If the technology
is an end-use technology the sum over the elasticity classes will be included in the capacity
constraint,

Additionally the activity variables of different technologies can be linked to the same capacity
variable, which allows to leave the choice of the activity variable used with a given capacity
to the optimization (see section 10.7).

Technologies without Load Regions

For technologies without load regions (i.e., technologies, where no input or output is modelled
with load regions) the production is related to the total installed capacity by the plant factor.
For these technologies the plant factor has to be given as the fraction they actually operate

per year. All end-use technologies (technologies with main output level "U) are modelled in
this way.


oot X28 -- SS A=) mat fe x Venue < hey % Raed

Technologies with Load Regions and Free Production Pattern

If a technology has at least one input or output with load regions, the activity variables and
capacity constraints will per default be generated separately for each load region. This can
be changed by defining the production pattern over the load regions. If the production
pattern remains free, the production in each load region is limited in relation to the installed
capacity separately for each load region, the capacity is determined by the activity in the
load region with the highest requirements. The plant factor has to be given as the fraction
the system operates in peak operation mode (in general this is the availability factor).

7

Maintenance times or minimum operation times could be included by using additional
relations, if required (see section 8).


= x rsuddt-- So A(T 1) & tyag X fe XVesvdt < heyy X Ted -

Technologies with Load Regions and Fixed Production Pattern

If a technology has at least one input or output with load regions and the production pattern
over the load regions is predefined only one activity variable and one capacity constraint is
generated per period. The plant factor has, like for technologies with load regions and free
production pattern, to be given for the load region with the highest capacity utilization (i.e.,
the highest power requirement). The capacity constraint is generated for only this load
region.

feet % Tbe 800) voy --







Technologies with Varying Inputs and Outputs
---------------------------------------------


Many types of energy conversion technologies do not have fixed relations between their inputs
and outputs. MESSAGEhas the option to link several activity variables of conversion,
technologies into one capacity constraint. For the additional activities linked to a capacity
variable a coefficient defines the maximum power available in relation to one power unit of
the main activity.

In the following this constraint is only described for technologies without load regions; the
other types are constructed in analogy (see also section 10.7).

 










The following notation is used in the above equations:
----------------------------------------------------



.. math::
    z_{s,u,d,l,t} & 	ext{ is the activity of conversion technology } v 	ext{ in period } t 	ext{ and, if defined so, load region } l 	ext{ (see section 2.1.1),} \
    Y_{s,u,d,t} & 	ext{ is the capacity variable of conversion technology } v 	ext{ (see section 2.1.2).} \
    \eta_{s,u,d} & 	ext{ is the efficiency of technology } v 	ext{ in converting the main energy input, } s, 	ext{ into the main energy output, } d, \
    	au_{s,u,d} & 	ext{ is the "plant factor" of technology } v, 	ext{ having different meaning depending on the type of capacity equation applied,} \
    \Delta t & 	ext{ is the length of period } t 	ext{ in years,} \
    T_{s,u,d} & 	ext{ is the plant life of technology } v 	ext{ in periods,} \
    h_{c,s,u,d} & 	ext{ represents the installations built before the time horizon under consideration,} \
    f_i & 	ext{ is 1 if the capacity variable is continuous, and represents the minimum installed capacity per year (unit size) if the variable is integer,} \
    l_m & 	ext{ is the load region with maximum capacity use if the production pattern over the year is fixed,} \
    \pi_{(l_m,s,u,d)} & 	ext{ is the share of output in the load region with maximum production,} \
    \lambda_{s,u,d} & 	ext{ is the relative capacity of main output of technology (or operation mode) } s,u,d 	ext{ to the capacity of main output of the alternative technology (or operation mode),} \
    \lambda_l & 	ext{ is the length of load region } l 	ext{ as fraction of the year, and} \
    \lambda_{l_m} & 	ext{ is the length of load region } l_m, 	ext{ the load region with maximum capacity requirements, as fraction of the year.}

2.2.2 Upper Dynamic Constraints on Construction Variables

.. math::
    MY_{z,s,u,d,t} & 	ext{ The dynamic capacity constraints relate the amount of annual new installations of a technology in a period to the annual construction during the previous period.} \
    \gamma^{gr}_{s,u,d,t} & 	ext{ is the maximum growth rate per period for the construction of technology } v, \
    \gamma^{in}_{s,u,d,t} & 	ext{ is the initial size (increment) that can be given for the introduction of new technologies,} \
    Y_{s,u,d,t} & 	ext{ is the annual new installation of technology } v 	ext{ in period } t.

2.2.3 Lower Dynamic Constraints on Construction Variables

.. math::
    LY_{z,s,u,d,t} & 	ext{ [The lower dynamic constraints on construction variables] }



.. math::
    Y_{s,u,d,t} - \gamma^{gr}_{s,u,d,t} \times Y_{s,u,d,(t-1)} > -\gamma^{in}_{s,u,d,t},

where
- \gamma^{gr}_{s,u,d,t} is the minimum growth rate per period for the construction of technology v,
- \gamma^{in}_{s,u,d,t} is the "last" size (decrement) allowing technologies to go out of the market, and
- Y_{s,u,d,t} is the annual new installation of technology v in period t.

2.2.4 Upper Dynamic Constraints on Activity Variables

.. math::
    M_{s,u,d,t}

The dynamic production constraints relate the production of a technology in one period to the production in the previous period. If the technology is defined with load regions, the sum over the load regions is included in the constraint.

.. math::
    \sum_{l} \xi_{s,u,d,l} \times [ z_{s,u,d,l,t} - \gamma^{adj}_{s,u,d,t} \times z_{s,u,d,l,(t-1)} ] < \gamma^{cap}_{s,u,d,t},

where
- \gamma^{adj}_{s,u,d,t} and \gamma^{cap}_{s,u,d,t} are the maximum growth rate and increment as described in section 2.2.2 (the increment is to be given in units of main output), and
- z_{s,u,d,l,t} is the activity of technology v in load region L.

If demand elasticities are modelled, the required sums are included for end-use technologies.

2.2.5 Lower Dynamic Constraints on Activity Variables

.. math::
    L_{s,u,d,t}

.. math::
    \sum_{l} \xi_{s,u,d,l} \times [ z_{s,u,d,l,t} - \gamma^{adj}_{s,u,d,t} \times z_{s,u,d,l,(t-1)} ] > -\gamma^{cap}_{s,u,d,t},

where
- \gamma^{adj}_{s,u,d,t} and \gamma^{cap}_{s,u,d,t} are the maximum growth rate and increment as described in section 2.2.3, and
- z_{s,u,d,l,t} is the activity of technology v in load region L.

