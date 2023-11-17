
Chapter 4
=========

Domestic Resources
------------------

Variables
^^^^^^^^^
Extraction of domestic resources is modelled by variables that represent the quantity
extracted per year in a period. A subdivision into cost categories (which are called "grades"
in the model) and further into elasticity classes can be modelled.

Resource Extraction Variables
~~~~~~~~~~~~~~~~~~~~~~~~~~~~
.. math::

    R_{ergp,t},

where

.. list-table::
   :widths: 10 30
   :header-rows: 1

   * - Symbol
     - Meaning
   * - :math:`R`
     - identifies resource extraction variables,
   * - :math:`z`
     - is the level on that the resource is defined (usually = R),
   * - :math:`r`
     - is the identifier of the resource being extracted,
   * - :math:`g`
     - is the grade (also called cost category) of resource :math:`r`, :math:`g \in \{a,b,c,\ldots\}`.
   * - :math:`p`
     - is the class of supply elasticity, which is defined for the resource and grade, or "..", if no elasticity is defined for this resource and grade,
   * - :math:`t`
     - identifies the period.

The resource variables are energy flow variables and represent the annual rate of extraction
of resource :math:`r`. If several grades are defined, one variable per grade is generated (identifier :math:`g` in
position 4). Supply elasticities can be defined for resource extraction as described in section
10.11; in this case one variable per elasticity class (identifier :math:`p` in position 5) is generated.

Constraints
^^^^^^^^^^^
The overall availability of a resource is limited in the availability constraint per grade, annual
resource consumption can be constrained per grade (sum of the elasticity classes) and total.
Additionally, resource depletion and dynamic resource extraction constraints can be modelled.

Resource Availability per Grade
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
.. math::
    RR_{rg,g..}

Limits the domestic resource available from one cost category (grade) over the whole time
horizon. Total availability of a resource is defined as the sum over the grades.

.. math::
    \sum_{p} \sum_{t} \Delta t 	imes RR_{rgp,t} \leq RRg - \Delta t_0 RR_{rg,0},

where

.. list-table::
   :widths: 10 30
   :header-rows: 1

   * - Symbol
     - Meaning
   * - :math:`RRg`
     - is the total amount of resource :math:`r`, cost category :math:`g`, that is available for extraction,
   * - :math:`RR_{rgp,t}`
     - is the annual extraction of resource :math:`r`, cost category (grade) :math:`g` and elasticity class :math:`p` in period :math:`t`,
   * - :math:`\Delta t`
     - is the length of period :math:`t`.
   * - :math:`\Delta t_0`
     - is the number of years between the base year and the first model year, and
   * - :math:`RR_{rg,0}`
     - is the extraction of resource :math:`r`, grade :math:`g` in the base year.

Maximum Annual Resource Extraction
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
.. math::
    RR_{r...t}

Limits the domestic resources available annually per period over all cost categories.

.. math::
    \sum_{g} \sum_{p} RR_{rgp,t} \leq RR_{t},

where

.. list-table::
   :widths: 10 30
   :header-rows: 1

   * - Symbol
     - Meaning
   * - :math:`RR_{t}`
     - is the maximum amount of resource :math:`r`, grade :math:`g`, that can be extracted per year of period :math:`t`, and
   * - :math:`RR_{rgp,t}`
     - is the annual extraction of resource :math:`r`, cost category (grade) :math:`g` and elasticity class :math:`p` in period :math:`t`.

Resource Depletion Constraints
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
.. math::
    RR_{rgd,t}

The extraction of a resource in a period can be constrained in relation to the total amount
still existing in that period. For reasons of computerization, these constraints can also be
generated for imports and exports, although they do not have any relevance there (they
could, e.g., be used for specific scenarios in order to stabilize the solution).

.. math::
    \Delta t \sum_{p} RR_{rgp,t} \leq \delta_{rg} \left[ RR_{g} - \Delta t_0 RR_{rg,0} - \sum_{	au=1}^{t-1} \Delta 	au 	imes RR_{rgp,	au} 
ight],

where

.. list-table::
   :widths: 10 30
   :header-rows: 1

   * - Symbol
     - Meaning
   * - :math:`RR_{g}`
     - is the total amount of resource :math:`r`, cost category :math:`g`, that is available for extraction,
   * - :math:`RR_{rgp,t}`
     - is the annual extraction of resource :math:`r`, cost category (grade) :math:`g` and elasticity class :math:`p` in period :math:`t`,
   * - :math:`\delta_{rg}`
     - is the maximum fraction of resource :math:`r`, cost category :math:`g`, that can be extracted in period :math:`t`,
   * - :math:`\Delta t`
     - is the length of period :math:`t` in years,
   * - :math:`\Delta t_0`
     - is the number of years between the base year and the first model year, and
   * - :math:`RR_{rg,0}`
     - is the extraction of resource :math:`r`, grade :math:`g` in the base year.

Maximum Annual Resource Extraction per Grade
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
.. math::
    RR_{rga,t}

Limits the domestic resources available from one cost category per year.

.. math::
    \sum_{p} RR_{rgp,t} \leq RR_{gt},

where

.. list-table::
   :widths: 10 30
   :header-rows: 1

   * - Symbol
     - Meaning
   * - :math:`RR_{gt}`
     - is the total amount of resource :math:`r`, cost category :math:`g`, that is available for extraction, and
   * - :math:`RR_{rgp,t}`
     - is the annual extraction of resource :math:`r`, cost category (grade) :math:`g` and elasticity class :math:`p` in period :math:`t`.

Upper Dynamic Resource Extraction Constraints
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
.. math::
    MRR_{r...t}

The annual extraction level of a resource in a period can be related to the previous one by a
growth parameter and an increment of extraction capacity resulting in upper dynamic
extraction constraints. For the first period the extraction is related to the activity in the
baseyear.

.. math::
    \sum_{g,p} RR_{rgp,t} - \gamma_{rt} \sum_{g,p} RR_{rgp,(t-1)} \leq \sigma_{rt},

where

.. list-table::
   :widths: 10 30
   :header-rows: 1

   * - Symbol
     - Meaning
   * - :math:`\gamma_{rt}`
     - is the maximum growth of extraction of resource :math:`r` between period :math:`t-1` and :math:`t`,
   * - :math:`\sigma_{rt}`
     - is the initial size (increment) of extraction of resource :math:`r` in period :math:`t`,
   * - :math:`RR_{rgp,t}`
     - is the annual extraction of resource :math:`r`, cost category (grade) :math:`g` and elasticity class :math:`p` in period :math:`t`.

Lower Dynamic Resource Extraction Constraints
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
.. math::
    LRR_{r...t}

The annual extraction level of a resource in a period can also be related to the previous one
by a decrease parameter and a decrement resulting in lower dynamic extraction constraints.
For the first period the extraction is related to the activity in the baseyear.

.. math::
    \sum_{g,p} RR_{rgp,t} - \eta_{rt} \sum_{g,p} RR_{rgp,(t-1)} \geq - g_{rt},

where

.. list-table::
   :widths: 10 30
   :header-rows: 1

   * - Symbol
     - Meaning
   * - :math:`\eta_{rt}`
     - is the maximum decrease of extraction of resource :math:`r` between period :math:`t-1` and :math:`t`,
   * - :math:`g_{rt}`
     - is the “last” size (decrement) of extraction of resource :math:`r` in period :math:`t`,
   * - :math:`RR_{rgp,t}`
     - is the annual extraction of resource :math:`r`, cost category (grade) :math:`g` and elasticity class :math:`p` in period :math:`t`.

Dynamic Extraction Constraints per Grade
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
.. math::
    MRR_{rg,t}, and
    LRR_{rg,t}

The same kind of relations as described in sections 4.2.5 and 4.2.6 can be defined per grade of
the resource.
