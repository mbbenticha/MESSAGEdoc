Chapter 9
=========

Objective and Cost Counters
---------------------------

9.1 Constraints
---------------

9.1.1 Cost Accounting Rows
--------------------------

The different types of costs (i.e., entries for the objective function) can be accumulated over all technologies in built-in accounting rows. These rows can be generated per period or for the whole horizon and contain the sum of the undiscounted costs. They can also be limited. The implemented types are:

- **CCUR** - fix (related to the installed capacity) and variable (related to the production) operation and maintenance costs,
- **CCAP** - investment costs; if the investments of a technology are distributed over the previous periods, also the entries to this accounting rows are distributed (if the capital costs are levelized, the total payments in a period can be taken from CINV; CCAP shows the share of investments in the according period, then),
- **CRES** - domestic fuel costs,
- **CARI** - costs related to the user-defined relations of type 1 (see section 8),
- **CAR2** - costs related to the user-defined relations of type 2 (see section 8),
- **CRED** - costs for reducing demands due to demand elasticities, only related to technologies supplying the demands directly,
- **CIMP** - import costs,
- **CEXP** - gains for export, and
- **CINV** - total investments (in case of levelized investment costs, see CCAP).

9.1.2 The Objective Function
----------------------------

**FUNC**

In its usual form, the objective function contains the sum of all discounted costs, i.e., all kinds of costs that can be accounted for. All costs related to operation (i.e., resource use, operation, etc.) are included.

The objective function has the following general form:

.. math::
    \sum \left[ \beta_{t} \left( \sum_{s \in S} \sum_{l \in L} z_{svd,l,t} \times \epsilon_{svd} \times \text{ccur}(svd,t) + \sum_{i \in M} r^{\text{mult}}_{rc_{svd}} \times \text{cari}(m,t) \right) + \sum_{s \in S} \epsilon_{svd} \times U_{svd,e,t} \times \left( k_{c} \times (\text{ccur}(svd,t) + \sum_{m \in M} r^{\text{mult}}_{rc_{svd}} \times \text{car2}(m,t)) + \text{cred}(d,e) + \sum_{m \in M} r^{\text{mult}}_{rc_{svd}} \times \text{cari}(m,t) \times \frac{dr(t)}{s_{svd,m}} \right) + \sum_{r \in R} \sum_{g \in G} \sum_{l \in L} \sum_{p \in P} R_{rgp,l,t} \times c_{res}(rgp,l,t) + \sum_{c \in C} \sum_{l \in L} \sum_{p \in P} E_{rcp,l,t} \times c_{imp}(rcp,l,t) - \sum_{c \in C} \sum_{l \in L} \sum_{p \in P} E_{rcp,l,t} \times c_{exp}(rcp,l,t) \right] + \sum_{s \in S} \sum_{d \in D} \sum_{t=t'}^{t'+t_{l}} \Delta(t-1) \times Y_{zsvd,r} \times \left( \text{ccap}(svd,r) \times \frac{k_{s}}{s_{rvd,d}} \right) + \sum_{i \in I} r^{\text{mult}}_{rc_{svd}} \times \text{cari}(m,t) \times \left( \frac{dr(t')}{s_{rvd,m}} \right)

where:
- :math:`\Delta t` is the length of period t in years,
- :math:`\beta_{t}` is a discount factor,
- :math:`dr(t)` is the discount rate in period t in percent,
- :math:`z_{svd,l,t}` is the annual consumption of technology v of fuel s load region l and period t; if v has no load regions, l = ".",
- :math:`\epsilon_{svd}` is the efficiency of technology v in converting s to d,

- **ccur(sud,t)**: the variable operation and maintenance costs of technology :math:`v` (per unit of main output) in period :math:`t`,
- **r^{\text{rel}}_{rc_{svd}}**: is the relative factor per unit of output of technology :math:`v` for relational constraint :math:`m` in period :math:`t`, load region :math:`l`,
- **car1(m,t)** and **car2(m,t)**: the coefficients for the objective function that are related to the user-defined relation :math:`m` in period :math:`t`,
- **car2(ml,t)**: are the same for load region :math:`l`, if relation :math:`m` has load regions,
- **U_{svd,e,t}**: is the annual consumption of fuel :math:`s` of end-use technology :math:`v` in period :math:`t` and elasticity class :math:`e`,
- **k_e**: is the factor giving the relation of total demand for :math:`d` to the demand reduced due to the elasticity to level :math:`e`,
- **r^{\text{rel}}_{rc_{svd}}**: is the relative factor per unit of output of technology :math:`v` for relational constraint :math:`m` in period :math:`t`,
- **cred(d,e)**: is the cost associated with reducing the demand for :math:`d` to elasticity level :math:`e`,
- **Y_{zsvd,t}**: is the annual new built capacity of technology :math:`v` in period :math:`t`,
- **cfix(sud,t)**: are the fix operation and maintenance cost of technology :math:`v` that was built in period :math:`t`,
- **ccap(svd,t)**: is the specific investment cost of technology :math:`v` in period :math:`t` (given per unit of main output),
- **f^{\text{rel}}_{rc_{svd}}**: is the share of this investment that has to be paid :math:`n` periods before the first year of operation,
- **f^{\text{rel}}_{rc_{svd}}**: is the relative factor per unit of new built capacity of technology :math:`v` for user-defined relation :math:`m` in period :math:`t`,
- **f^{\text{share}}_{rc_{m,n}}**: is the share of the relative amount of the user-defined relation :math:`m` that occurs :math:`n` periods before the first year of operation,
- **R_{rgp,l,t}**: is the annual consumption of resource :math:`r`, grade :math:`g`, elasticity class :math:`p` in load region :math:`l` and period :math:`t`,
- **cres(rgp,l,t)**: is the cost of extracting resource :math:`r`, grade :math:`g`, elasticity class :math:`p` in period :math:`t` and load region :math:`l`,
- **lrcp,l,t**: is the annual import of fuel :math:`r` from country :math:`c` in load region :math:`l`, period :math:`t` and elasticity class :math:`p`; if :math:`r` has no load regions :math:`l` = ".",
- **cimp(rcp,l,t)**: is the cost of importing :math:`r` in period :math:`t` from country :math:`c` in load region :math:`l` and elasticity class :math:`p`,
- **E_{rcp,l,t}**: is the annual export of fuel :math:`r` to country :math:`c` in load region :math:`l`, period :math:`t` and elasticity class :math:`p`; if :math:`r` has no load regions :math:`l` = ".",
- **cexp(rcp,l,t)**: is the gain for exporting :math:`r` in period :math:`t` to country :math:`c` in load region :math:`l` and elasticity class :math:`p`.

