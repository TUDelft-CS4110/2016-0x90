
# Reformulating branch coverage as a many-objective optimization problem
**Article:**
*. Panichella, Annibale, Fitsum Meshesha Kifetew, and Paolo Tonella .*Reformulating branch coverage as a many-objective optimization problem. Software Testing, Verification and Validation (ICST), 2015 IEEE 8th International Conference on. IEEE, 2015.


## Test Data Generation
Represents a search problem that aims at testing as many program element as possible (to maximize the number of branches).
Whole test suite approach that was previously described as preferable to the single-branch at a time method.
A new branch coverage will be proposed as many-objective optimisation problem alternatively to whole test suite approach that aggregates multiple objectives into a single value.
A highly scalable many-objective genetic algorithm, called MOSA(Many-Objective Sorting Algorithm) is proposed to many-objective branch coverage problem.

## Many-objective Optimisation Problem
The problem itself includes discovering a set of test cases in order number of covered branches to be maximised. 
By using genetic algorithms (search-based testing) in the testing of procedural programs one branch at a time is targeted with the help of fitness function to estimate the distance between each test case trace and the branch.
Single-target strategy encounters several issues (some targets are more difficult to cover) that whole suite approach is aiming to overcome.
Studies on multi-objective optimisation have indicated that it is not effective in some kind of problems, but more efficient than single-objective approach.
In branch reformulation it is suggested different branches as different object to be optimised.
The candidate solution is test case and the optimisation is able through fitness according to all yet uncovered branches.
The MOSA algorithm adapts multi-objective genetic algorithm to alter the selection scheme.
Comparison between whole suite approach and MOSA is realised on 64 Java classes and results show the preference of MOSA in terms of higher (or the same) coverage and faster convergence.

## Problem Formulation
Analysis considering the differences and characteristics of whole suite approach (based on single-objective formulation) and many-objective approach.

### Single Objective Formulation
A single test suite must be discovered that provides coverage (sum of the individual branch distances) of all branches and minimises the fitness (full branch coverage) functions.
The fitness function determines the coverage of a certain test to all branches of the program.
The fitness function consists of all branches at the same time and all distances by summarising all contributors from the individual branch distances.
In such way whole suite approach transforms multiple search goals into single search goal.

### Many-Objective Formulation
Using this approach, the objectives to be optimised consist of all branch distances of all branches in the class under test and minimises fitness functions for all branches.
Formulated like this, the fitness is a vector (not a single score) and candidate solutions (test cases, not suites) are described using Pareto dominance and Pareto optimality to evaluate the test case.
Single-objective optimisation problems provide only one solution, while  multi-objective is possible to prompt a set of Pareto-optimal test cases corresponding to trade-offs in the objective space.
The goal of presented approach is only the test cases that maximises the total coverage by covering previously uncovered branches to be discovered.


## Algorithm
An analysis of previous methods will be presented including limitations and main characteristics, the new proposed method will be summarised in the second part.

### Existing Many Objective Algorithms
The previous studies of multi-objective evolutionary algorithms show that they are successfully applied but not efficient in solving multi-objective optimisation problems(software refactoring, test case prioritisation, etc.).
To reduce the limitations, new algorithms, that modify the Pareto dominance relation, have been suggested.
All proposed multi-objective algorithms have been applied for numerical optimisation problem with no more than 15 objectives.

### MOSA: a New Many Objective Sorting Algorithm
Previous studies on that many-objective reveal that the proportion of non-dominated solutions increase exponentially with the number of objectives.
According to that assigning a preference for selection is not  possible, thus the search process is transformed to random one.
As a consequence, the search is focused to the neighbouring test cases of uncovered branches of the program.
To overcome these limitations (changing the preference), a new method is proposed.
By definition: Given a branch bi, a test case x is preferred over another test case y if and only if the values of objective function fro bi satisfy the following condition: fi(x) < fi(y), where fi(x)denotes the objective score of test case x for branch b. 
The proposed algorithm (MOSA) provides reference ordering during the selection process.
The search with MOSA is targeted at uncovered branches, it collects the shortest covering test cases in external data structure in order to form the final test suite.

###Graphical Interpolation of Preference Criterion
A comparison between traditional algorithm with non-dominated ranks and the proposed preference based many-objective genetic algorithm.

##Empirical Evaluation
It aims at analysis of performance and productivity of MOSA in terms of coverage and convergence in comparison with single-objective whole suite approach.

###Prototype tool
The experiment was realised with prototype tool, called EvoSuite test data generation framework.

###Subjects
For testing purposes, randomly were selected 64 Java classes.

###Metrics
As metrics technique of effectiveness is used number of branch coverage divided by the total number of the branches in that class. 
Efficiency is measured through consumption of search budget and the number of executed statements.

###Experimental Protocol
The experiment is limited by time - either by reaching full branch coverage, or search budget is complete, or total assigned time is elapsed.
Each test is run 100 times for each search strategy.
Default parameter values, used in EvoSuite, were applied in order performance of the algorithms to be evaluated.

###Results
Based on the results of the experiment, it can be concluded that whole suite approach was better that MOSA in 9 of the cases, while on the other hand MOSA performed better in 42, and in 13 no considerable difference was detected.
In conclusion, MOSA managed to produce better branch coverage than whole suite approach, and when the same coverage is achieved, MOSA shows better convergence.

###Qualitative Analysis
As stated earlier, MOSA solutions are represented by test case (not suite) within the current population of each branch that is yet not covered.
While the whole suite fitness is applicable in increasing the global number of covered goals, but the single-objective coverage of a single test may remain unused.

###Threats to Validity
Threats to Validity provide correlation between theory and experimental results.
The measure values that were used to estimate the efficiency and effectiveness - branch coverage and number of statements executed are commonly used in other experiments.
The experiment for both methods was carried out 100 times with default values in order to avoid fault results .

##Related Work
The experiment and studies of search algorithms for test data generations are motivated by their application and practical representation.
It should be outlined that all previous studies of multi-objective approaches for evolutionary test data generation aim at one branch a time, but the number of objectives remains small.
On the contrary, the proposed approach targets many-objective branch coverage problem while minimising simultaneously the distances between the test cases and uncovered branches in the class under test.

##Conclusions and Future Work
MOSA introduces a new approach towards multi-objective branch coverage by reducing scalability practical problems with working with great number of branches.
That resulted in statistically better results that whole suite approach.
Regarding the future work, it will be expanded to include more java classes.
New experiments will include new adequacy testing criteria - statement coverage, mutation testing, etc. in order to test whether multi-objective problem to be formulated and solved through MOSA.
In addition, a combination with other non-coverage criteria (execution time, memory consumption) will be investigated.