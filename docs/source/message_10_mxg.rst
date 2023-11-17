Chapter 10
==========

Special Features of the Matrix Generator
---------------------------------------

The mathematical formulation of MESSAGE as presented in the previous sections shows the structure of all constraints as the matrix generator builds them up. The background of the more complicated features is given here for a better understanding.

10.1 The Time Horizon—Discounting the Costs
-------------------------------------------

The whole time horizon of the calculations is divided into periods of optional length. All variables of MESSAGE are represented as average over the period they represent, resulting in a step-function. All entries in the objective function are discounted from the middle of the respective period to the first year, if they relate to energy flow variables and from the beginning of that period if they represent power variables. The function to discount the costs has the following form:

.. math::
    c_t = \frac{C_t}{\prod_{k=1}^{t} \left(1 + \frac{d_{rk}}{100}\right)^{\Delta k}} \times f_i

where:
- :math:`C_t` is the cost figure to be discounted,
- :math:`c_t` is the objective function coefficient in period :math:`t`,
- :math:`f_i` is a factor which is 1 for costs connected to investments and \(\left(1+\frac{d_{rk}}{100}\right)^{-\frac{\Delta t}{2}}\) otherwise,
- :math:`d_{rk}` is the discount rate in period :math:`k`.

10.2 Distributions of Investments
---------------------------------

In order to support short-term applications of MESSAGE, the possibility to distribute the investments for a new built technology over several periods was implemented. The same type of distributions can be applied to entries in user-defined relations if they relate to construction. The distribution of investments can be performed in several ways. There is one common parameter that is needed for all of these possibilities, the construction time of the technology [ct].

The implemented possibilities are:
1. Explicit definition of the different shares of investments for the years of construction. The input are ct figures that will be normalized to 1 internally.
2. The investment distribution is given as a polynomial of 2nd degree, the input consists of the three coefficients:

.. math::
    y = a + bx + cx^2 , \quad x = \frac{1}{ct}

where ct is the construction time. The values of the function are internally normalized to 1, taking into account the construction time.
3. Equal distribution of the investments over the construction period.
4. A distribution function based on a logistic function of the type:

.. math::
    f = \frac{100}{1 + e^{a(x-x_0)}}

where :math:`x_0 = \frac{t}{2}` and :math:`a = \frac{2}{d} \ln \left( \frac{100}{c} - 1 \right)`.

This function is expanded to a normalized distribution function of the following type:

.. math::
    g = \left[ \frac{100}{1 + e^{-\frac{100}{50}(x-x_0)-c}} - c \right] \times \frac{1}{1 - \frac{c}{50}}.

:g: gives the accumulated investment at the time :math:`x`, :math:`x` is given in percent of the construction time. The parameter :math:`c` describes the difference of the investment in the different years. :math:`c` near to 50 results nearly in equal distribution, a :math:`c` close to 0 indicates high concentration of the expenditures in the middle of the construction period.

In order to shift the peak of costs away from the middle of the construction period, the function is transformed by a polynomial:

.. math::
    x = a^2 + bx, \quad 0 < x < 100,

.. math::
    b = \frac{5000 - d^2}{100d - d^2}, \quad 0 < d < 100,

.. math::
    a = \frac{1}{100} - b.


*d* denotes the time at that the peak of expenditures occurs in percent of *ct*.

The distribution of these yearly shares of investments is done starting in the first period of operation with a one year's share, the expenditures of the remaining *ct - 1* years are distributed to the previous periods.

The coefficients of the capacity variables of a technology in a relational constraint can be distributed like the investments.

10.3 The Load Curve
-------------------

The years representing a period can be subdivided into so-called load regions. This can be done by either ordering the whole year according to the power requirements for the most important energy carriers like, e.g., electricity, or by grouping the year into load regions with similar characteristics (hereafter called characteristic loads), like, e.g., winter days and nights and summer days and nights. The first option results in an interpolation of the usual representation of the load curve by a step function (see Figure 10.1), the second one in a step-function where the time is still ordered in a historic way.

Figure 10.1: Example of an ordered (left) and a semi-ordered load curve (right)
(WD stands for winter day, WN for winter night, SD for summer day, and SN for summer night.)

10.4 Consideration of Load Variations in Conversion Technologies
---------------------------------------------------------------

The activity of a conversion technology is generated for each load region, if the main in- or output energy form is defined to have load regions. In this case, the relation of these activities between the load regions is freely chosen by the model. The relations can be fixed by the user to reflect a certain fixed production pattern. In this case, the activity will only be generated once and written to the energy flow balances with coefficients reflecting the chosen pattern. A powerplant operating in baseload mode would for instance have the shares of the load regions in the year as coefficients in the balances of energy forms with load regions.

For end-use technologies (output level "U") the production is assumed to meet the demand pattern, the input of the technology is fixed to reflect the according demand variations. This can also be changed into a different pattern. This would, e.g., model night storage heating systems that meet the heat demand of a household but generate a final electricity demand with a different load distribution, namely at night.

10.5 The Implementation of Energy Storage
-----------------------------------------

MESSAGE contains a quite complex model of energy storage. Section 3 contains the mathematical formulation. In order to allow for different types of storage like daily and seasonal, the distribution of demands over the year has to be depicted in a semi-ordered load curve. The user has to define the load regions in a physical order. Daily storage would for instance need the definition of several parts of the day that are ordered like in an actual day. The model can then store energy in one part of the day and release it in one of the following load regions, keeping track of the storage contents in each load region. This loop of storage is closed for all but seasonal storage, where an appropriate part of the energy stored in the last load region is delivered to the next period.

The length of time that the content of a storage can be held can also be limited to some fraction of the time it is dedicated to. An example would be a daily heat storage that can only keep the heat 80% of the day, after that time it could have too low a temperature to be used. The loss of energy in the case of heat storage can be modeled by a decay function:

.. math::
    c_{t+1} = c_t \times e^{-\delta t},

where:
- :math:`c_t` is the content of storage in load region :math:`l`,
- :math:`\delta` is the decay constant of storage [unit: :math:`\frac{1}{k}`],
- :math:`k` is 1 day for daily storage, 1 year for seasonal storage, etc.,
- :math:`\delta l` is the fraction of :math:`k` that lies between load regions :math:`l` and :math:`l+1` [unit: :math:`k`].

The amount of energy available from storage is reduced over time according to this function.

If several types of load regions are defined, e.g., weekly and seasonal (it should rather be named yearly for reasons of consistency), they are ordered according to the length of the time period they span. The "bigger" one (the seasonal) can then work like the smaller one (weekly), too (see Figure 10.2). The decay of content and limitation on time is only applied to the biggest type of load the storage works in.

Figure 10.2: Flows of energy in daily and seasonal storage

The two basic parts of a storing device, namely the input/output part (for a pumped hydro storage the generator/turbine/pump part) and the real storage (dam and reservoir) can be handled in two different ways. One of them is to link them in size, i.e., to fix the content (in MWyrs or GWyrs) in relation to the generation capacity (in MW or GW), as it is usually the case with batteries.

The other possibility, which could, e.g., be useful for pumped hydropower storage plants, is to keep them separate with their own costs and leave the relation of the two open for the optimization process.

10.6 Lag Times Between Input and Output of a Technology
-------------------------------------------------------

Since MESSAGE can be used for very short time steps, even for steps of 1 year per period, the implementation of lag-times between input and output of a conversion technology seemed to be appropriate. One possible application are the reprocessing units for nuclear fuels, which usually keep the fuels for several years.

The lag time for a technology (keyword lag) is given in years and the period in which the output is available is calculated beginning from the middle of the period when the input is required.

10.7 Variable Inputs and Outputs
--------------------------------

A lot of power plants can use different fuels for electricity generation, the highest variability occurs between oil products and natural gas as fuel. This can be modeled by having two or more inputs.

10.8 The Contribution of Capacities Existing in the Base Year
-----------------------------------------------------------

The possible contribution of an installation that exists in the base year is kept track of over time. There are two possibilities to give the necessary information to MESSAGE.

1. Define the capacities that were built in the years iyr,...,iyr − τ + 1, with iyr = base year and τ = plant life in years explicitly. These capacities are then distributed to historic periods of the length ν.

2. Define the total capacity, c₀, that exists in iyr and the rate at that it grew in the last τ years, γ. This information is then converted to one similar to 1. by using the function:

.. math::
    \text{The function appears here but was not captured accurately by the OCR.}

where:
- :math:`t` is the annual construction in period −t, (0 = base year),
- :math:`γ` is the annual growth of new installations before the base year,
- :math:`c₀` is the total capacity in the base year.
- :math:`\tau` is the plant life, and
- :math:`\nu` is the length of the periods in that the time before the base year is divided.

The right hand sides in the capacity constraints are derived by summing up all the old capacities that still exist in a certain period (according to the plant life). If the life of a technology expires within a period, MESSAGE takes the average production capacity in this period as installed capacity (this represents a linear interpolation between the starting points of this and the following period).

10.9 Capacities which Operate Longer than the Time Horizon
----------------------------------------------------------

If a capacity of a technology is built in one of the last periods its lifetime can exceed the calculation horizon. This fact is taken care of by reducing the investment costs by the following formula:

.. math::
    C_t' = C_t \times \left[ \frac{\prod_{k=1}^{t-\tau} \left( 1 + d_{r_k} \right)}{\prod_{k=1}^{t+\nu-\tau} \left( 1 + d_{r_k} \right)} \right]

where:
- :math:`\nu` is the number of years the technology exists after the end of the calculation horizon,
- :math:`d_{r_t}` is the discount rate for year :math:`t`,
- :math:`\tau` is the plant life in years,
- :math:`C_t` is the investment cost in year :math:`t`, and
- :math:`C_t'` is the reduced investment.

10.10 Own-Price Elasticities of Demand
--------------------------------------

Own-price elasticities of demand can be interpreted either as short-term elasticities resulting in reduced demand due to sharp price increases (they have to relate to a reference price- and demand level and represent renunciation of services) or as long-term elasticities reached by substituting capital for energy. In the latter case, the user has to ensure that the relatively decreased demand level is maintained over the calculation horizon by applying user-defined relations (see section 8). The costs and levels of demand reduction can be derived from the investments and savings that are associated with certain additional installations, like, e.g., three-glass windows to save in space heating.

The form of the own price elasticity function of demand is

.. math::
    \frac{Q}{Q_r} = \left( \frac{P}{P_r} \right)^{\epsilon},

where
- :math:`Q_r` is the reference demand level,
- :math:`P_r` is the reference price level, and
- :math:`\epsilon` is the elasticity, (assumed to be < 0).

It says that the demand will decrease by a factor of :math:`2^{\epsilon}` if the price rises by 2. This function is approximated by a step-function of the following form:

The demands (Q) and prices (P) are normalized to the reference levels:

.. math::
    Q = q \times Q_r,
    P = p \times P_r,

the normalized values follow the function

.. math::
    q = p^{\epsilon},

or

.. math::
    p(q) = q^{-\frac{1}{\epsilon}}.

To reduce the demand to the level :math:`q_i` the supply has to have the cost

.. math::
    c(q_i) = \int_{q_i}^{1} \frac{1}{q} dq = \frac{1}{1 + \epsilon} \left[ 1 - q_i^{1+\epsilon} \right],

a function increasing monotonously with decreasing :math:`q_i` (see also Figure 10.3). In absolute terms this means that the cost would be higher by an absolute value of

.. math::
    R(Q_i) = c\left( \frac{Q_i}{Q_r} \right) \times Q_r \times P_r,

compared to the cost at the reference level.

The step-function is then defined by choosing certain levels of demands and prices (:math:`Q_i`, :math:`P_i`), i=1(1)n with :math:`Q_i < Q_r`, that fulfill the elasticity function. The code can choose which demand level it supplies, but if it supplies a level :math:`Q_i < Q_r` it has to pay additionally :math:`R(Q_i)`, the cost of reducing the demand to level i.

Figure 10.3: Representation of Demand Elasticities

10.11 Supply Elasticities
-------------------------

The reaction of the market prices to changes in demand can be expressed as elasticities:

.. math::
    \frac{P}{P_r} = \left( \frac{S}{S_r} \right)^{\alpha},

where:
- :math:`P_r` is the reference price level,
- :math:`S_r` is the reference supply level, and
- :math:`\alpha` is the elasticity.

The normalized form of this equation is:

.. math::
    c = s^{-\alpha},

where:
- :math:`c = \frac{P}{P_r}`.

.. math::
    s = \frac{S}{S_r}.

The relationship is converted to a step-function with n steps, which is shown in Figure 10.4. :math:`f(s_1)` is the cost of supplying amount :math:`s_1` relative to supplying :math:`s_r`, while :math:`f(s_1) + (s_2)` is the relative cost of supplying the amount :math:`s_2`. The marginal costs are then defined as

.. math::
    \mu(s) = \frac{f(s_i) - f(s_{i-1})}{s_i - s_{i-1}} \cdot \frac{1}{s},

where :math:`s_{i-1} < s \leq s_i`.

Figure 10.4: Representation of Supply Elasticities

According to the normalized function, the total price of buying the amount :math:`s` is then

.. math::
    t(s) = \sum_{j=1}^{i} \mu(s_j) \cdot (s_j - s_{j-1}) + \mu(s_i) \cdot (s - s_{i-1}).

The price of the amount :math:`S`, :math:`S_{i-1} < S \leq S_i`, is defined as

.. math::
    TC(S) = P_r \cdot S \cdot t\left(\frac{S}{S_r}\right).

In the matrix, this function is implemented as n + 1 additive elasticity classes for resources
and imports (:math:`R_0 = S_r`, :math:`R_y = S_i - S_{i-1}`, :math:`i=1(1)n`), which have increasing costs. The code
takes these classes as supply one after the other and has to pay increasing prices, then.

10.12 The Mixed Integer Option
-------------------------------

If the LP-package used to solve a problem formulated by MESSAGE has the capability to
solve mixed integer problems, this can be used to improve the quality of the formulated
problems, especially for applications to small regions.

The improvement consists in a definition of unit sizes for certain technologies that can only
be built in large units. This avoids, for instance, the installation of a 10 kW nuclear reactor in
the model of the energy system of a city or small region (it can only be built in units of e.g.,
700 MW). Additionally, this option allows to take care of the "economies of scale" of certain
technologies.

This option is implemented for a technology by simply defining the unit size chosen for this
technology (keyword `cmix`). The according capacity variable is then generated as an integer in
the matrix, its value is the installation of one powerplant of unit size.

If a problem is formulated as a mixed integer, it can be applied without this option by changing
just one switch in the general definition file (keyword `mixsw`). Then all capacity variables are
generated as real variables.

10.13 Nonlinear Objective Functions
-----------------------------------

In combination with MINOS, MESSAGE can be applied to problems with a partly nonlinear
objective function or with nonlinear constraints. The requirements are that the functions are
differentiable and convex with respect to the solution space.

In order to use a nonlinear objective or nonlinear constraints, the user has to identify the
variables that are to be included with nonlinear coefficients in the input file (keyword `non`);
they will be written to the matrix as the first entries in the columns section—as required by
MINOS—and to supply MINOS with an additional subroutine (Funcon for nonlinear
constraints and Funobj for nonlinear objective gradients), which yields the nonlinear part of
the constraints or objective and the first derivatives as required by MINOS.

In order to start a nonlinear problem, it can be solved as a linear problem at the beginning.
The nonlinear variables can be fixed to user-defined estimates by specifying "initial bounds"
in the bounds section.

The order in which the nonlinear variables appear in the input file is essential because the
same order is used for identifying them in Funcon and in Funobj. MESSAGE generates the
activity variables first, then the capacity variables (both of them in the order in which the
technologies appear in the input file). The loops in producing the columns are nested in the
following order:

.. _load regions:
.. _demand elasticity classes:
.. _time periods:

10.14 Multiobjective Optimization
---------------------------------

MESSAGE is capable of handling two types of multiobjective optimization: It can generate
weights on different types of activities or prepare the MPS-file for using the Reference
Trajectory Optimization Method.

Weights on Activities
---------------------

The common way to optimize several objectives at the same time is to define weights for the
different types of activities in the model. MESSAGE provides an easy way to do this: It is
possible to define costs that are added to the objective gradients of all technologies that have
coefficients in a specific additional relation (see chapter 8), e.g., all technologies emitting SO2
could get some addition to the objective gradient if this addition is defined for the relation
accounting for the emissions of SO2.

Alternatively, additive and multiplicative weights for all activities considered in the "Cost
Accounting Rows” (see section 9.1.1) can be defined. As an example, additional costs (taxes)
put on energy imports could be imposed this way.

The Reference Trajectory Method
-------------------------------

The "Reference Trajectory Optimization Method" is an approach to optimize more than one
objective function for a problem in a way that circumvents the necessity to define weights on
the single objectives. It allows to define reference trajectories for all objectives; the solution
will lie on the "pareto"-optimal border of the feasible region and be as close as possible to all
reference trajectories.

The way in which the pareto-optimal border is approached can either be problem-oriented
with an egalitarian approach between the objectives or aspiration oriented, meaning that
objectives with a reference point that is closer to the overall optimum (the UTOPIA point),
get higher influence on the solution.

The objectives can be grouped to nodes, for each of which a multiobjective approach is
taken. The nodes are just summed up in the objective function. This feature is useful if
interconnected models with separate objectives are depicted in one physical model.

Mathematically the objective functions are summed into a single function that minimizes the
maximum difference between the reference trajectory and the actual value of the function for
each time step. The difference is calculated using the Chebyshev norm of the two points
(reference point and actual value). The time steps are usually handled like the nodes, i.e.,
each point in time has a single objective. Alternatively, the time steps can be included in one
objective, which means that the compromise solution is searched over all objectives and time
steps at once. This algorithm may lead to unrealistic results, since the dynamics of the model
may not be handled adequately.

The aggregated objective function has the following general form:

.. math::
    (The mathematical formula was not captured accurately by the OCR.)

where
- n is the index for the nodes,
- t is the index for the time steps,
- J_n is the set of objectives in node n,
- j is the index for the objectives,
- σ is a scaling factor to improve numerical stability (all objectives should have the same order of magnitude to avoid rounding-off errors),
- y_{jt} is the actual value of objective j in time step t,
- y^*_{jt} is the reference point for objective j in time step t,
- α_{jt} is the scaling factor for objective j in time step t, it represents the way the pareto-optimal border is approached,
- ε is a small number to drive the solution algorithm in the right direction.

If all time steps are to be aggregated into one objective, the sum over t is added to the sums over the variables instead of being outside the maximum.

The scaling factors are generated depending on a criterion regarding the way in which the absolutely optimal (and probably infeasible) point is approached:

Problem-oriented scaling: α_{jt}
Aspiration-oriented scaling: α_{jt}

where
- u_{jt} is the optimal value for objective j in time step t with single-objective optimization. The respective values for all objectives constitute the UTOPIA point.
