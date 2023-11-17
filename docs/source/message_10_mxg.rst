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

.. note:: Further details on distributions of investments and the load curve.

*d* denotes the time at that the peak of expenditures occurs in percent of *ct*.

The distribution of these yearly shares of investments is done starting in the first period of operation with a one year's share, the expenditures of the remaining *ct - 1* years are distributed to the previous periods.

The coefficients of the capacity variables of a technology in a relational constraint can be distributed like the investments.

10.3 The Load Curve
-------------------

The years representing a period can be subdivided into so-called load regions. This can be done by either ordering the whole year according to the power requirements for the most important energy carriers like, e.g., electricity, or by grouping the year into load regions with similar characteristics (hereafter called characteristic loads), like, e.g., winter days and nights and summer days and nights. The first option results in an interpolation of the usual representation of the load curve by a step function (see Figure 10.1), the second one in a step-function where the time is still ordered in a historic way.

Figure 10.1: Example of an ordered (left) and a semi-ordered load curve (right)
(WD stands for winter day, WN for winter night, SD for summer day, and SN for summer night.)
