Chapter 6
==========

Energy Flows
------------

6.1 Constraints
^^^^^^^^^^^^^^^

Energy flows are modelled solely by linking the activity variables of the different conversion technologies and the resource extraction, import and export variables in balance constraints. These constraints ensure that only the amounts of energy available are consumed. There are no further variables required to model energy flows.

Energy demands are also modelled as part of a balance constraint: it is the right hand side and defines the amount to be supplied by the technologies in this constraint.

The following description of the energy flow constraints in MESSAGE is given for the following set of level identifiers:

- U Useful energy (demand level),
- F Final energy (after distribution),
- T Final energy (after transmission),
- X Secondary energy,
- A Primary energy, and
- R Energy resources.

The identifier of the demand level (U) which gives it a special meaning (see section 2.1.1) and imports and exports are given for primary energy. Clearly any other combination of technologies is also possible.

6.1.1 Demand Constraints
^^^^^^^^^^^^^^^^^^^^^^^

Ud...t

Out of the predefined levels each one can be chosen as demand level. However, level "U” has a special feature. This is related to the fact that useful energy is usually produced on-site, e.g., space heat is produced by a central heating system, and the load variations over the year are all covered by this one system. Thus, an allocation of production technologies to the different areas of the load curve, like the model would set it up according to the relation between investment and operating costs would ignore the fact that these systems are not located in the same place and are not connected to each other. MESSAGE represents the end-use technologies by one variable per period that produces the required useful energy in the load pattern needed and requires the inputs in the same pattern. For special technologies like, e.g., night storage heating systems, this pattern can be changed to represent the internal storage capability of the system.

This representation of end-use technologies has the advantage of reducing the size of the model, because the demand constraints, the activity variables and the capacity constraints of the end-use technologies do not have to be generated for each load region.

If another level is chosen as demand level or the demand level is not named "U”, all demand constraints for energy carriers that are modelled with load regions are generated for each load region. The general form of the demand constraints is:

.. math::

    \sum_{s\in S} \sum_{e=0}^{e_{d}} \eta_{s}^{d,e} x k_{e} x U_{svd,e,t} + \sum_{s\in S} \sum_{e=0}^{e_{d}} \gamma_{s}^{d,e} x k_{e} x U_{svbue,t} \geq U_{d,t},

where:

- :math:`U_{d,t}` is the annual demand for :math:`d` in period :math:`t`,
- :math:`U_{svd,e,t}` is the activity of end-use technology :math:`v` in period :math:`t`, elasticity class :math:`e` and period :math:`t` (see section 2.1.1),
- :math:`\eta_{s}^{d,e}` is the efficiency of end-use technology :math:`v` in converting :math:`s` to :math:`d`,
- :math:`\gamma_{s}^{d,e}` is the efficiency of end-use technology :math:`v` in producing by-product :math:`d` from :math:`s` (:math:`\delta` is the main output of the technology),
- :math:`e_{d}` is the number of steps of demand reduction modelled for own-price elasticities of demand :math:`d`, and
- :math:`k_{e}` is the factor giving the relation of total demand for :math:`d` to the demand reduced to level :math:`e` due to the demand elasticity (:math:`k_{e} x U_{svd,e,t} = U_{svd,0,t}, k_{0} = 1`, :math:`k_{e}` is increasing monotonously.)

6.1.2 Distribution Balance
^^^^^^^^^^^^^^^^^^^^^^^^^

Fs...lt

                                                                                                                                                       This constraint, the final energy balance, matches the use of final energy needed in the end-use technologies and the deliveries of the distribution systems. It is generated for each load region, if energy form s is modelled with load regions.

.. math::

    \sum_{s \in S} \epsilon_{vs} \times F_{svs,l,t} - \sum_{d} \eta_{vd} \times e_{d} \sum_{c=0}^{\alpha} U_{svd,e,t} \geq 0,

where:

- :math:`F_{svs,l,t}` is the activity of the distribution technology in load region :math:`l` and period :math:`t` (see section 2.1.1),
- :math:`\epsilon_{vs}` is the efficiency of technology :math:`v` in distributing :math:`s`,
- :math:`U_{svd,e,t}` is the activity of end-use technology :math:`v` in period :math:`t` and elasticity class :math:`e`,
- :math:`\beta_{s}` is the use of fuel :math:`s` relative to fuel :math:`o` (the main input) by technology :math:`v`, and
- :math:`\eta_{l,d}` is the fraction of demand for :math:`d` occurring in load region :math:`l`.

6.1.3 Transmission or Transportation Balance
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Ts...lt

This constraint gives the simplest form of an energy balance equation of MESSAGE. It matches the output of transmission to the requirements of distribution systems. The difference to other levels (F, X, A) is not built-in, but emerges from the simplicity of energy transportation (i.e., transportation technologies do usually not have by-products and only one input). Also big industrial consumers that are directly connected to the transmission system would have to be included in this constraint. Like level F it does usually exist for all load regions if they are defined for the fuel.

.. math::

    \sum_{s \in S} \epsilon_{ws} \times T_{sws,l,t} - \sum_{s \in S} F_{svs,l,t} \geq 0,

where:

- :math:`T_{sws,l,t}` is the activity of the transportation technology :math:`w` (see section 2.1.1), and all the other entries to the equation are the same as in section 6.1.2.
                                                                                                                                                       
6.1.4 Central Conversion Balance
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Xs...lt

In principle, the secondary energy balance is built up in the same way as the two previous ones (sections 6.1.2 and 6.1.3). It matches the production of central conversion technologies to the requirements of the transmission systems. Secondary energy imports and exports of secondary energy are usually assigned to level X.

.. math::

    \sum_{r \in R} \epsilon_{vr} \times X_{vros,l,t} + \sum_{r \in R} \beta_{vr} \times X_{vroo,l,t} - \sum_{s \in S} T_{sros,l,t} +
    \sum_{c \in C_p} IX_{scpl,t} - \sum_{c \in C_p} EX_{scpl,t} \geq 0

where:

- :math:`X_{vros,l,t}` is the activity of central conversion technology :math:`v` in load region :math:`l` and period :math:`t` (see section 2.1.1); if the secondary energy form :math:`s` is not defined with load regions (i.e. :math:`l = "."`) and the activity of technology :math:`v` exists for each load region, this equation will contain the sum of the activity variables of technology :math:`v` over the load regions.
- :math:`\epsilon_{vr}` is the efficiency of technology :math:`v` in converting energy carrier :math:`r` into secondary energy form :math:`s`,
- :math:`\beta_{vr}` is the efficiency of technology :math:`v` in converting energy carrier :math:`r` into the by-product :math:`s` of technology :math:`v`,
- :math:`T_{sros,l,t}` is explained in section 6.1.3, and
- :math:`IX_{scpl,t}` and :math:`EX_{scpl,t}` are the import and export variables explained in sections 5.1.1 and 5.1.2, respectively.

6.1.5 Resource Extraction, Export and Import Balance
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Ar...t

This equation matches production and import of primary energy to the requirements of central conversion, transport and for export. In the general case primary energy does not have load regions. Some technologies, like, e.g., nuclear reactors need inventories of primary energy and also leave a last core that is available at the end of the lifetime. It may be necessary to model by-products of extraction technologies, for instance the availability of associated gas at oil production sites.

.. math::

    \sum_{r \in R} \epsilon_{vr} \times Ar_{vr,t} - \sum_{l} \sum_{r \in R} X_{vros,l,t} + \sum_{p \in P} \beta_{vp} \times X_{vp,l,t} + \sum_{c \in C_p} IX_{rcp,t} - \sum_{c \in C_p} EX_{rcp,t} \geq 0

                                                                                                                                                       .. math::

    \sum_{c_p} EA_{rcp,t} + \sum_{f_{us}} \left( \frac{\Delta(t - T_{fus})}{\Delta t} \times p(f_{us,r}) \times YX_{fus,t-(T_{fus})} - \frac{\Delta(t + 1)}{\Delta t} \times \ell(f_{us,r}) \times YX_{fus,t+(1)} \right) \geq 0,

where:

- :math:`A_{rwt...}` is the activity of technology :math:`w` extracting resource :math:`r`,
- :math:`\epsilon_{wr}` is the efficiency of technology :math:`w` in extracting fuel :math:`r` (this is usually 1),
- :math:`\beta_{w,p}` is the efficiency of technology :math:`w` in producing secondary energy form :math:`s` from the by-input :math:`p`,
- :math:`EA_{rcp,t}` and :math:`IA_{rcp,t}` are the import and export variables described in sections 5.1.1 and 5.1.2, respectively,
- :math:`T_{fus}` is the plant life of technology :math:`v` in periods (depending on the lengths of the periods covered),
- :math:`YX_{fus,t}` is the annual new installation of technology :math:`v` in period :math:`t` (see section 2.1.2),
- :math:`\phi(f_{us,r})` is the amount of fuel :math:`r` that is needed when technology :math:`v` goes into operation (usually this is the first core of a reactor). It has to be available in the period before technology :math:`v` goes into operation, the normal unit is kWyr/kW,
- :math:`p(f_{us,r})` is the amount of fuel :math:`r` that becomes available after technology :math:`v` goes out of operation (for a reactor this is the last core that goes to reprocessing). The unit is the same as for :math:`\phi(f_{us,r})`, and
- :math:`\Delta t` is the length of period :math:`t` in years.

6.1.6 Resource Consumption
^^^^^^^^^^^^^^^^^^^^^^^^^^

Rr...t

The resources produced by the extraction technologies in a period can come from different cost categories (also called grades), which can, e.g., represent the different effort to reach certain resources. Short-term variations in price due to steeply increasing demand can be represented by an elasticity approach (see section 10.11).

.. math::

    \sum_{g,p} RR_{rgp,t} - \sum_{r,v} A_{rvr,t} \geq 0,

where:

- :math:`RR_{rgp,t}` is the annual extraction of resource :math:`r`, cost category (grade) :math:`g` and elasticity class :math:`p` in period :math:`t`, and
- :math:`A_{rvr,t}` is the activity of extraction technology :math:`v` in period :math:`t` (as described in section 2.1.1).
