
Chapter 5
=========

Imports and Exports
-------------------

Variables
^^^^^^^^^
Imports and exports are modelled by variables that represent the quantity imported per year
in a period. A subdivision into countries and further into elasticity classes can be modelled.

Import Variables
~~~~~~~~~~~~~~~~
.. math::

    I_{s,c,p,l,t},

where

- :math:`I` identifies import variables,
- :math:`z` is the level on that the imported energy form is defined (usually primary energy and secondary energy),
- :math:`s` identifies the imported energy carrier,
- :math:`c` is the identifier of the country or region the imports come from,
- :math:`p` is the class of supply elasticity, which is defined for the energy carrier and country, or "..", if no elasticity is defined for this energy carrier and country,
- :math:`l` is the load region identifier if :math:`s` is modelled with load regions, otherwise "..", and
- :math:`t` identifies the period.

The import variables are energy flow variables and represent the annual import of the
identified energy carrier from the country or region given. If supply elasticities are defined for
the import of this energy carrier and country one variable per elasticity class (identifier :math:`p` in
position 5) is generated.

Export Variables
~~~~~~~~~~~~~~~~
.. math::

    E_{z,c,p,l,t},

.. list-table::
   :widths: 10 30
   :header-rows: 1

   * - Symbol
     - Meaning
   * - :math:`E`
     - is the identifier for export variables,
   * - :math:`z`
     - is the level on that the exported energy form is defined (usually primary energy and secondary energy),
   * - :math:`s`
     - identifies the exported energy carrier,
   * - :math:`c`
     - is the identifier of the country or region the exports go to,
   * - :math:`p`
     - is the class of supply elasticity, which is defined for the energy carrier and country, or "..", if no elasticity is defined for this energy carrier and country,
   * - :math:`l`
     - is the load region identifier if :math:`s` is modelled with load regions, otherwise "..",
   * - :math:`t`
     - identifies the period.

The export variables are energy flow variables and represent the annual export of the
identified energy carrier to the country or region given. If supply elasticities are defined for
the export of this energy carrier and country one variable per elasticity class (identifier :math:`p` in
position 5) is generated.

Constraints
^^^^^^^^^^^
Imports per Country
~~~~~~~~~~~~~~~~~~~
.. math::

    I_{zrcg..}

Limits the imports of a fuel from a specific country :math:`c` over the whole horizon.

.. math::
    \sum_{p} \sum_{t} \Delta t 	imes I_{zrcp,t} \leq I_{rc},

where

.. list-table::
   :widths: 10 30
   :header-rows: 1

   * - Symbol
     - Meaning
   * - :math:`I_{rc}`
     - is the total import limit for :math:`r` from country :math:`c`,
   * - :math:`I_{zrcp,t}`
     - is the annual import of :math:`r` from country :math:`c`, elasticity class :math:`p` in period :math:`t`,
   * - :math:`\Delta t`
     - is the length of period :math:`t` in years.

Maximum Annual Imports
~~~~~~~~~~~~~~~~~~~~~~
.. math::

    I_{zr...t}

Limits the annual imports of a fuel from all countries per period.

.. math::
    \sum_{c} \sum_{p} I_{zrcp,t} \leq I_{rt},

where

.. list-table::
   :widths: 10 30
   :header-rows: 1

   * - Symbol
     - Meaning
   * - :math:`I_{rt}`
     - is the annual import limit for :math:`r` in period :math:`t`,
   * - :math:`I_{zrcp,t}`
     - is the annual import of :math:`r` from country :math:`c`, elasticity class :math:`p` in period :math:`t`.

Maximum Annual Imports per Country
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
.. math::

    I_{zrc,a,t}

Limits the imports from one country per year.

.. math::
    \sum_{p} I_{zrcp,t} \leq I_{rct},

where

.. list-table::
   :widths: 10 30
   :header-rows: 1

   * - Symbol
     - Meaning
   * - :math:`I_{rct}`
     - is the limit on the annual imports from country :math:`c`, period :math:`t` of fuel :math:`r`,
   * - :math:`I_{zrcp,t}`
     - is the annual import of :math:`r` from country :math:`c`, elasticity class :math:`p` in period :math:`t`.

Upper Dynamic Import Constraints
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
.. math::

    M_{I_{zr...t}}

The annual import level of a fuel in a period can, like the resource extraction, be related to
the previous one by a growth parameter and an increment resulting in upper dynamic
constraints.

.. math::
    \sum_{c,p} I_{zrcp,t} - \gamma_{rt} \sum_{c,p} I_{zrcp,(t-1)} \leq \sigma_{rt},

where

.. list-table::
   :widths: 10 30
   :header-rows: 1

   * - Symbol
     - Meaning
   * - :math:`I_{zrcp,t}`
     - is the annual import of :math:`r` from country :math:`c`, elasticity class :math:`p` in period :math:`t`,
   * - :math:`\gamma_{rt}`
     - is the maximum increase of import of :math:`r` between period :math:`t-1` and :math:`t`,
   * - :math:`\sigma_{rt}`
     - is the initial size (increment) of import of :math:`r` in period :math:`t`.

Lower Dynamic Import Constraints
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
.. math::

    L_{I_{zr...t}}

The annual import level of a fuel in a period can also be related to the previous one by a
decrease parameter and a decrement resulting in lower dynamic import constraints.

.. math::
    \sum_{c,p} I_{zrcp,t} - \eta_{rt} \sum_{c,p} I_{zrcp,(t-1)} \geq - g_{rt},

where

.. list-table::
   :widths: 10 30
   :header-rows: 1

   * - Symbol
     - Meaning
   * - :math:`I_{zrcp,t}`
     - is the annual import of :math:`r` from country :math:`c`, elasticity class :math:`p` in period :math:`t`,
   * - :math:`\eta_{rt}`
     - is the maximum decrease of import of :math:`r` between period :math:`t-1` and :math:`t`,
   * - :math:`g_{rt}`
     - is the “last” size (decrement) of import of :math:`r` in period :math:`t`.

Dynamic Import Constraints per Country
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
.. math::
    M_{I_{zrc,t}} \quad 	ext{and} \quad L_{I_{zrc,t}}

The same kind of relations can be defined per country from that the fuel is imported.

Constraints on Exports
^^^^^^^^^^^^^^^^^^^^^^
The exports of fuels can principally be limited in the same way as the imports. In the
identifiers of the variables and constraints the "I" is substituted by an "E".
