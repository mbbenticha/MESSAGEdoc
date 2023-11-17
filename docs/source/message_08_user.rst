Chapter 8
=========

User-defined Relations
----------------------

8.1 Constraints
---------------

The user-defined relations allow the user to construct constraints that are not included in the basic set of constraints. For each technology, the user can specify coefficients with that either the production variables (see section 2.1.1), the annual new installation variables (see section 2.1.2), or the total capacity in a year (like it is used in the capacity constraints, see section 2.2.1) can be included in the relation. The relations can be defined with and without load regions, have a lower, upper or fixed right hand side or remain free (non-binding) and be related to an entry in the objective function, i.e., all entries to this relation are also entered to the objective function with the appropriate discount factor. There are two types of user-defined constraints, for which the entries to the objective function—without discounting—are summed up under the cost accounting rows CARI and CAR2 (see chapter 9).

The formulation of the user-defined relations is given for relations, that are related to the main output of the technologies. It is also possible (e.g., for emissions) to relate the constraint to the main input of the technology, i.e., the amount of fuel used. In this case the efficiencies (e) would be omitted from the formulation.

8.1.1 Relation without Load Regions
-----------------------------------

Nm...t or Pm...t

Relations without load regions just sum up the activities (multiplied with the given coefficients) of all variables defined to be in this constraint. If a technology has load regions, the activity variables for all load regions of this technology are included. If the total capacity of a technology is included, all new capacities from previous periods still operating are included, if new capacities are included, the annual new installation of the current period is taken.

.. math::

    \sum_{s \in S} r_{om,s} \times U_{s,v,d,e,t} \times c_{e,s} + \sum_{s \in S} r_{e,s} \times Y_{U,s,v,d,r}

.. math::

    \sum_{r \in R} r^{\text{mult}}_{rc_{svd}} \times z_{rus,l,t} \times \epsilon_{svd} + r^{\text{mult}}_{rc_{svd}} \times z_{rus...} \times \epsilon_{svd} + \sum_{t'=t-ip}^{t} r^{\text{mult}}_{rc_{svd}} \times Y_{z, rus...} = 
    \begin{cases} 
    \text{free} \\
    \geq rhs_{m} \\
    = rhs_{m} \\
    \leq rhs_{m}
    \end{cases}

- :math:`U_{svd,e,t}` and :math:`YU_{svd...}` are the activity and capacity variables of the end-use technologies,
- :math:`z_{rus,l,t}` and :math:`Yz_{rus...}` are the activity variables of technologies with and without load regions and the capacity variables of the technologies,
- :math:`\epsilon_{svd}` are the efficiencies of the technologies; they are included by the code,
- :math:`r^{\text{mult}}_{rc_{svd}}` is the relative factor per unit of output of technology :math:`v` (coefficient) for relational constraint :math:`m`,
- :math:`r^{\text{mult}}_{rc_{svd}}` is the same per unit of new built capacity,
- :math:`r^{\text{mult}}_{rc_{wrs}}` is the relative factor per unit of output of technology :math:`w` (coefficient) for relational constraint :math:`m`, load region :math:`l`,
- :math:`r^{\text{mult}}_{rc_{svd}}` is the same per unit of new built capacity,
- :math:`tl` is 1 for relations to construction and :math:`\Delta r` for relations to total capacity,
- :math:`ip` is 1 for accounting during construction and the plant life on periods for accounting of total capacity, and
- :math:`rhs_{m}` is the right hand side of the constraint.

8.1.2 Relation with Load Regions
--------------------------------

Nm...lt or Pm...lt

The user-defined relations can be defined with load regions. Then all entries of activities of technologies with load regions are divided by the length of the according load region resulting in a representation of the utilized power.

.. math::

    \sum_{s \in S} r^{\text{mult}}_{rc_{svd}} \times U_{svd,e,t} \times \epsilon_{svd} + \sum_{t=ip}^{t} r^{\text{mult}}_{rc_{svd}} \times YU_{svd...} +

    \sum_{r \in R} r^{\text{mult}}_{rc_{wrs}} \times z_{wrs,l,t} \times \epsilon_{wrs} + r^{\text{mult}}_{rc_{svd}} \times z_{wrs...} \times \epsilon_{wrs} + \ldots

.. math::

    \sum_{\tau=t-ip}^{t} r^{\text{mult}}_{rc_{svd}} \times tl \times Y_{z, rus,\tau} =
    \begin{cases} 
    \text{free} \\
    \geq rhs_{m} \\
    = rhs_{m} \\
    \leq rhs_{m}
    \end{cases}


- :math:`U_{svd,e,t}` and :math:`YU_{svd,t}` are the activity and capacity variables of the end-use technologies,
- :math:`z_{rus,l,t}`, :math:`z_{rvs,t}` and :math:`Yz_{rvs,c}` are the activity variables of technologies with and without load regions and the capacity variables of the technologies,
- :math:`\epsilon_{svd}` and :math:`\epsilon_{rvd}` are the efficiencies of the technologies; they are included by the code,
- :math:`r^{\text{rel}}_{rc_{svd}}` is the relative factor per unit of utilized capacity of technology :math:`v` (coefficient) for relational constraint :math:`m` in load region :math:`l`, period :math:`t` (this constraint is adapted to represent the utilized power, as stated above),
- :math:`r^{\text{rel}}_{rc_{svd}}` is the same per unit of new built or installed capacity,
- :math:`r^{\text{rel}}_{rc_{wrs}}` is the relative factor per unit of output of technology :math:`v` (coefficient) for relational constraint :math:`m`, load region :math:`l`,
- :math:`r^{\text{rel}}_{rc_{wrs}}` is the same per unit of new built capacity,
- :math:`tl` is 1 for relations to construction and :math:`\Delta r` for relations to total capacity,
- :math:`ip` is 1 for accounting during construction and the plant life on periods for accounting of total capacity, and
- :math:`rhs_{m}` is the right hand side of the constraint.

8.1.3 Construction of Relations between Periods
-----------------------------------------------

Nm...t or Pm...t

The change of activities over time can either be limited or included in the objective by constructing relations between periods. The relations express the difference between the annual activity in a period and the following period. This difference can either be limited or included in the objective function.

.. math::

    \sum_{e=0}^{e_d} r^{\text{rel}}_{rc_{svd}} \times U_{svd,e,t} \times \epsilon_{svd} - r^{\text{rel}}_{rc_{svd}} \times U_{svd,e,(t-1)} \times \epsilon_{svd} + \ldots

.. math::
    z_{rus,(t-1)} \times \epsilon_{rus} + \sum_{r \in R} \left( r^{\text{mult}}_{rc_{rus}} \times z_{rus,l,t} \times \epsilon_{rus} - r^{\text{mult}}_{rc_{rus}}(t-1) \times z_{rus...(t-1)} \times \epsilon_{rus} \right) = 
    \begin{cases} 
    \text{free} \\
    \geq rhs_{m} \\
    = rhs_{m} \\
    \leq rhs_{m}
    \end{cases}

where:
- :math:`U_{svd,e,t}` is the activity variable of the end-use technologies,
- :math:`z_{rus,l,t}` and :math:`z_{rvs,t}` are the activity variables of technologies with and without load regions,
- :math:`\epsilon_{rus}` and :math:`\epsilon_{rvs}` are the efficiencies of the technologies; they are included by the code,
- :math:`r^{\text{mult}}_{rc_{rus}}` is the relative factor per unit of output of technology :math:`v` (coefficient) for relational constraint :math:`m`, period :math:`t`,
- :math:`r^{\text{mult}}_{rc_{rus}}` is the relative factor per unit of output of technology :math:`v` (coefficient) for relational constraint :math:`m`, load region :math:`l`,
- :math:`rhs_{m}` is the right hand side of the constraint.

For this type of constraints only the ro-coefficients have to be supplied by the user, the rest is included by the model. It can be defined with and without load regions.

8.1.4 Special Handling of Demand Elasticities
---------------------------------------------

Pm...t

The second type of user-defined relations differs from the first one in the fact that the activity of the end-use technologies is multiplied by :math:`k_e`, and therefore represents the production without reduction by demand elasticities.

Thus, this constraint can be applied to force a certain reduction level due to the elasticities reached in one period to be also reached in the following period, allowing the interpretation of the reduction as investments in saving. The coefficient of the technologies supplying a demand have to be the inverse of this demand in the current period, then. This constraint has the following form:

.. math::
    \sum_{s \in S} U_{svd,e,t} \times \epsilon_{svd} \times k_e \leq 
    \sum_{s \in S} U_{svd,e,(t-1)} \times \epsilon_{svd} \times U_{d,t} \times k_e \leq 0,

where the coefficients are supplied by MESSAGE. The user can additionally define multiplicative factors for these coefficients.
