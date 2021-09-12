# challenge-roadef-2020
This challenge is about solving a Mixed Integer Non Linear problem: MINLP
More details can be found at https://www.roadef.org/challenge/2020/en/


# Methods used to solve the problem

- Soveur Excel 
- Python gekko v
- Heuristique: pour trouver une solution
- Metaheuristique: pour trouver une solution faisable (qui respecte les contrainte)

In this repository, i present the method 2, the one i developped using gekko


# Introduction

Helping the decision maker to find the best decision, which is sometimes complex, requires a large number of alternatives and constraints to be taken into consideration. Thanks to operational research techniques, making the best choice is possible to answer a complex decision problem. Thus, operations research is an important tool in decision support.

In this perspective and within the framework of the Complex Systems Engineering (CSE) course, an optimization project in the form of a challenge has been programmed with the aim of confronting a complex, real-life decision-making problem encountered in the industrial environment, exploring the requirements and difficulties encountered in industrial applications, and applying the scientific knowledge and know-how acquired to a practical subject.

The French Society of Operational Research (OR) and Decision Support (ROADEF) is exceptionally organising, in partnership with the European Society of Operational Research (EURO), the ROADEF / EURO 2020 challenge dedicated to the problem of optimisation of maintenance planning in collaboration with RTE.

This report is organized into three main parts. First, we will introduce the challenge by describing the proposed maintenance planning problem to be studied. Then, we will review the parts performed in the context of this project. Finally, we will propose a qualitative analysis of the results.


# Description of the challenge

Our project is part of the resolution of a maintenance planning optimization problem in collaboration with the electricity transmission network (RTE).

RTE is the French manager of the electricity transmission network which ensures the operation, maintenance, supply and delivery of electricity on the 100,000 km of lines. RTE&#39;s main mission is therefore to guarantee the delivery and supply of electricity. Thus, maintenance operations must be well planned.

RTE&#39;s problem is that some maintenance operations on overhead power lines require power cuts while others are live work. If several nearby lines fail, the network may not be able to handle the demand for electricity properly. In this context, the delivery of electricity must be guaranteed. Therefore, RTE wants to plan outages due to maintenance interventions. Moreover, forecasting maintenance schedules allows RTE to implement efficient network design strategies. With the beginning of the energy transition, new electricity production capacities are appearing everywhere. People&#39;s behaviour and consumption patterns are also changing, bringing new and unpredictable variables into the equation. Maintenance is becoming more complex than ever, and tomorrow&#39;s operations and maintenance strategy will be different from today&#39;s.

To solve this problem, RTE decided, firstly, to calculate risk values corresponding to different future scenarios, secondly, these calculated values have to be evaluated in order to find a good optimal schedule in terms of time. Finally, the resulting schedule has to be validated.

Thus, given the risk values, our job is to find an optimal and feasible planning with respect to a risk-based objective, determining the interventions and their start time with the minimum cost.


## Mathematical model

_ **Problem data:** _

To model our problem, we define the number of steps noted T. The event must take place during one year, for that T can be equal to 365 in case of number of day per year and 53 in case of number of week per year. Thus we define the set H by :

To carry out the different interventions, the workforce needs resources of different sizes. We note C, the set of resources. **The maximum resource** indicates the upper limit that cannot be exceeded. **The minimum resource** indicating the minimum value that should be consumed. The interventions are the tasks that should be carried out during the whole year. They are not equal in terms of time or resources required. The set of interventions is noted I :

We also define **the duration** which represents the duration of the intervention if it starts at . **The resource** represents the work required for the resource time by the task when it starts at start\_i.

In addition to the set of scenarios noted S, this set can be interpreted by the set of possible cases for each simulation made by RTE.

While an intervention is being carried out, the disconnection of some lines may involve a power cut. This entails risks for the RTE, so we introduce the variable **Risk** which illustrates the risk for and and the intervention when i starts at start\_i. In addition, data .

_ **Decision variables :** _

The solution will be expressed through the decision variables :

_ **Objective function:** _

RTE conducted studies using current methods to find out which indicator best represents all the interventions related to the nuances. The average cost appeared to be a first solution, but it was not sufficient because it did not take into account the specific behaviours of the risk distribution. Finally, RTE found another indicator, in combination with the average cost, which is able to illustrate the professional realities of the project.

The cumulative risk is given by :

The average cumulative risk is given by :

Thus the objective function 1 (or the average cost) is given by :

Then, to define the objective function 2 we use the quantile of a set :

Let E be a finite non-empty set, the quantile of E is given by :

Our indicator depends on the value of the quantile with represents the quantile rate, in our case it is 0.9. For each period t, we define the quantile value as follows:

The value of the excess is defined at time by :

And so the objective function 2 is given by :

After that, the objective function that we will minimize is equal to :

With the combination coefficient between 0 and 1.

_ **Constraints:** _

_ **Schedule constraints :** _

_**Schedule constraints (part 2) :**_


At each time, each intervention i consumes resources. These values are computed for each resource c. In this problem, at each time t, each resource c is bounded.

  - The values of the resource are given for each resource, at each time t, for each intervention
  - Each intervention must be completed within the planning period
  - Let i be an intervention, start\_i, its start date
  - The end date of an intervention that starts at time t and has duration of ∆𝑖,𝑡 is equal to start\_i +∆𝑖,𝑡 o The end date Tfinal of the planning is the end date of the last period [T,T+1] so Tfinal=T+1

- 2

  - This constraint incorporates the fact that each intervention must start before a deadline tmax (specified in the json file). We further check that for each intervention i: tmax = (T+1) - max { ∆𝑖,𝑡 , t 𝜖 H}

_ **Resource constraints :** _

At each time, each intervention i consumes resources. These values are computed for each resource c. In this problem, at each time t, each resource c is bounded.

  - The values of the resource are given for each resource, at each time t, for each intervention i taking into account its start date st


- risk value


_ **Exclusion constraints** __ **:** _

_**Mathematical model (summary) :**_

Minimize



- **Python**

The search for methods of solving problems

In the literature, NLP (Non linear Problem) and MIP (Mixed Integrator Problem) are NP-hard problems. There are methods to solve these problems using solvers such as AlphaECP [[GAMS](https://neos-server.org/neos/solvers/minco:AlphaECP/GAMS.html)], ANTIGONE [[GAMS](https://neos-server.org/neos/solvers/minco:ANTIGONE/GAMS.html)], BARON [[AMPL](https://neos-server.org/neos/solvers/minco:BARON/AMPL.html)] [[GAMS](https://neos-server.org/neos/solvers/minco:BARON/GAMS.html)], Bonmin [[AMPL](https://neos-server.org/neos/solvers/minco:Bonmin/AMPL.html)] [[GAMS](https://neos-server.org/neos/solvers/minco:Bonmin/GAMS.html)] and others (# 1). These solvers usually solve NLP problems and then use the brach&amp;bound method to find an integer or binary solution.


# Input files for the optimisation program
input format [here](https://github.com/Hermann-web/challenge-roadef-2020/blob/main/checker/README.md) and some input and output examples can be found at [checker/test_checker](https://github.com/Hermann-web/challenge-roadef-2020/tree/main/checker/test_checker)

# Implementation
In order to facilitate the implementation, we used a library available under pytn. Among the existing libraries, there are Pyomio, Gekko

_ **Approach 1:** _ Test all possible cases. The complexity of this program is exponential, which will not be at all time-optimal if the number of interventions or quarters exceeds 3. It is not necessary to test this method to know that the computation time will be very large. Indeed, there is a non-linear objective, while every possible solution must be tested. It is more efficient to find a method that generates a solution and then use it in a meta-heuristic than to make this enumeration.

_ **Approach 2:** _ Solving the MINLP problem

With the help of the Python solver used, we define the decision variables as binary variables, we write the constraints and the objective in a format adapted to the resolution (see the documentation).

This method does real number solving and uses the Branch&amp;Bound method to find a good solution. Since there are non-linear and non-regular constraints, there are alternative functions to use. These functions are at least of class C1 by performing extensions. But due to the complexity of the second term of the objective, the algorithm reaches a large number of iterations without converging.

_ **Approach 3:** _ Ignore the second term of the objective function

Indeed, this term presents a rather strong non-linearity. Indeed, there are functions such as min, max which are non-linear.

Taking this objective into account, there are functions to regularize. Moreover, there are more intermediate variables, which increases the number of iterations of the solver. Thus, since there are limits on the number of iterations, the method does not always converge. On the other hand, removing an objective modified the problem. But, the second term of the objective function is a standard deviation used to capture the values of too high risks. So, this term is generally small compared to the first objective, which can be verified on the examples presented below. So we can leave the second term provided that the risk values do not contain values too high compared to the average. In practice, on the first two examples, taking into account all the constraints and neglecting the second term of the objective function, the MINLP solver finds the optimal solution. An advantage of this method is that we can quickly find a solution that verifies the constraints. This solution will be an input for our meta-heuristic.

_ **Results obtained :** _

On examples 1,2 and even3 , we obtain solutions that minimize the first term of the objective

![](RackMultipart20210912-4-1lqr2kk_html_d06de3582cf7a8a4.gif) ![](RackMultipart20210912-4-1lqr2kk_html_599be3aeb75e34c2.gif)

&quot;example1.json&quot;

| I | I1 | I2 | I3 |
| --- | --- | --- | --- |
| t | 1 | 1 | 2 |
| Objective1 | 8.33 euros |
| Excess | 0.66 euros |
| Objective | 4.5 euros |

&quot;example2.json&quot;

| I | I1 | I2 | I3 |
| --- | --- | --- | --- |
| t | 1 | 2 | 2 |
| Objective1 | 11 |
| Excess | 0 |
| Objective | 5.5 |

&quot;Example3.json&quot;

| Intervention | Start date |
 |
| --- | --- | --- |
| Intervention\_345 | 6 | **Objective1** |
| Intervention\_309 | 1 |
| Intervention\_552 | 7 |
| Intervention\_259 | 2 | 3701.53 |
| Intervention\_580 | 8 |
| Intervention\_653 | 6 |
| Intervention\_652 | 6 | **Excess** |
| Intervention\_503 | 6 |
| Intervention\_659 | 13 |
| Intervention\_660 | 1 | 0.0050 |
| Intervention\_63 | 1 |
| Intervention\_212 | 10 |
| Intervention\_385 | 7 | **Objective** |
| Intervention\_602 | 6 |
| Intervention\_257 | 17 |
| Intervention\_680 | 9 | 1850.77 |
| Intervention\_31 | 3 |
| Intervention\_90 | 6 |

On the instances of class A, the algorithm reaches a limited number of iterations without finding a feasible solution and therefore stops except for instance n°9 which contains the least number of decision variables.

_ **Approach4:** _ Use an NLP solver: Keep the objective and relax the entire constraint.

As it is not possible to find an equivalent formulation of the problem as a real variable problem without integer constraints, we perform a relaxation of the constraint and solve the nonlinear problem in real numbers. We then add as a constraint, the bern of the decision variables (between 0 and 1).

The advantage of this method is that we find a lower bound to our optimum.




# Analysis of results

_ **Excel solver:** _ The results found by the solver are feasible for both example 1 and 2. All the constraints are respected for the solution of example 1 contrary to example 2 which does not respect the time constraint.

_ **Solver python :** _ The results obtained are satisfactory for the first three examples. We manage to find an optimum. But, for larger instances, we can find a feasible solution that respects at least one constraint. In some cases, we can find a feasible solution, which allows us to use the meta-heuristic to find a better solution.




_ **Team members :** _

- Akram IBN EL AHMER
- Hermann LEIBNITZ KLAUS AGOSSOU
- Reda ASSIOUI
- Olivier KOMBA MBONGO
- Souad EZZOUINE

# Read more
- [1](#sdfootnote1anc)( ) Some MINLP solvers according to [neos-server.org](https://neos-server.org/neos/solvers/)(accessed on 22/04/2021): AlphaECP [[GAMS](https://neos-server.org/neos/solvers/minco:AlphaECP/GAMS.html)], ANTIGONE [[GAMS](https://neos-server.org/neos/solvers/minco:ANTIGONE/GAMS.html)], BARON [[AMPL](https://neos-server.org/neos/solvers/minco:BARON/AMPL.html)] [[GAMS](https://neos-server.org/neos/solvers/minco:BARON/GAMS.html)], Bonmin [[AMPL](https://neos-server.org/neos/solvers/minco:Bonmin/AMPL.html)] [[GAMS](https://neos-server.org/neos/solvers/minco:Bonmin/GAMS.html)], Couenne [[AMPL](https://neos-server.org/neos/solvers/minco:Couenne/AMPL.html)] [[GAMS](https://neos-server.org/neos/solvers/minco:Couenne/GAMS.html)], DICOPT [[GAMS](https://neos-server.org/neos/solvers/minco:DICOPT/GAMS.html)], FilMINT [[AMPL](https://neos-server.org/neos/solvers/minco:FilMINT/AMPL.html)], Knitro [[AMPL](https://neos-server.org/neos/solvers/minco:Knitro/AMPL.html)] [[GAMS](https://neos-server.org/neos/solvers/minco:Knitro/GAMS.html)], LINDOGlobal [[GAMS](https://neos-server.org/neos/solvers/minco:LINDOGlobal/GAMS.html)], MINLP [[AMPL](https://neos-server.org/neos/solvers/minco:MINLP/AMPL.html)], SBB [[GAMS](https://neos-server.org/neos/solvers/minco:SBB/GAMS.html)], scip [[AMPL](https://neos-server.org/neos/solvers/minco:scip/AMPL.html)] [[CPLEX](https://neos-server.org/neos/solvers/minco:scip/CPLEX.html)], [[GAMS](https://neos-server.org/neos/solvers/minco:scip/GAMS.html)] [[MPS](https://neos-server.org/neos/solvers/minco:scip/MPS.html)] [[OSIL](https://neos-server.org/neos/solvers/minco:scip/OSIL.html)] [[Python](https://neos-server.org/neos/solvers/minco:scip/Python.html)] [[ZIMPL](https://neos-server.org/neos/solvers/minco:scip/ZIMPL.html)], SHOT [[GAMS](https://neos-server.org/neos/solvers/minco:SHOT/GAMS.html)]

- Resolution with python
	- NLP
		- http://www.pyopt.org/
		- https://docs.scipy.org/doc/scipy/reference/tutorial/optimize.html#constrained-minimization-of-multivariate-scalar-functions-minimize
	- Pyomo: https://pyomo.readthedocs.io/en/stable/pyomo_overview/simple_examples.html	
	- gekko:
		- https://gekko.readthedocs.io/en/latest/model_methods.html?highlight=array#pre-built-objects
		- https://apmonitor.com/wiki/index.php/Main/GekkoPythonOptimization
		- handle bug: 
			- https://stackoverflow.com/questions/56671888/how-to-retrieve-the-infeasibilities-txt-from-the-gekko
			- https://stackoverflow.com/questions/56942615/how-to-fix-solution-not-found-error-in-python-gekko-optimal-control-code
	- nomad: https://www.gerad.ca/nomad/

- More about solving MINLP problems
	- overview:
		- NLP:
			- https://www.researchgate.net/post/What-types-of-Optimization-Problems-are-supported-by-AMPL-AIMMS-GAMS-Pyomo
		- integer
			- https://scicomp.stackexchange.com/questions/19870/python-solvers-for-mixed-integer-nonlinear-constrained-optimization
			- bonnin:
				https://www.researchgate.net/post/Can-you-suggest-best-solver-for-the-mixed-integer-nonlinear-programming
				https://stackoverflow.com/questions/42391945/solving-minlp-with-pyomo-and-bonmin
		- MINLP: 
			- https://sci-hub.st/https://doi.org/10.1007/978-1-4614-1927-3_1
			- https://link.springer.com/book/10.1007/978-1-4614-1927-3			
	- https://hal.archives-ouvertes.fr/tel-01450981/document



