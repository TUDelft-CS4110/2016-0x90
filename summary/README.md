# Automated Test Case Generation


- [Automated Test Case Generation](#automated-test-case-generation)
 - [Introduction](#introduction)
 - [Search-Based Software Test Data Generation](#search-based-software-test-data-generation)
   - [Metaheursitic Search Applications](#metaheursitic-search-applications)
   - [Metaheursitic Search Techniques](#metaheursitic-search-techniques)
   - [Structural (White-Box) Testing](#structural-white-box-testing)
   - [Functional (Black-Box) Testing](#functional-black-box-testing)
   - [Grey-box Testing](#grey-box-testing)
   - [Non-Functional Testing](#non-functional-testing)
   - [Conclusions](#conclusions)
 - [A Multi-Objective Genetic Algorithm to Test Data Generation](#a-multi-objective-genetic-algorithm-to-test-data-generation)
   - [Introduction](#introduction-1)
   - [Test Data Generation by a Multi-objective Approach](#test-data-generation-by-a-multi-objective-approach)
   - [Evaluation Studies](#evaluation-studies)
   - [Conclusions](#conclusions-1)
 - [Evolutionary Testing of Classes](#evolutionary-testing-of-classes)
   - [Automated Test Case Generation](#automated-test-case-generation-1)
   - [Genetic Algorithms for the Unit Testing of Classes](#genetic-algorithms-for-the-unit-testing-of-classes)
   - [The Tool eToc](#the-tool-etoc)
   - [Experimental Results](#experimental-results)
 - [Whole Test Suite Generation](#whole-test-suite-generation)
   - [Research Goal](#research-goal)
   - [Background Information](#background-information)
   - [Test Suite Optimization](#test-suite-optimization)
   - [EvoSuite](#evosuite)
   - [Difficulties](#difficulties)
   - [Conclusion](#conclusion)
 - [Effectiveness of Whole Test Suite Generation](#effectiveness-of-whole-test-suite-generation)
   - [Research Goal](#research-goal-1)
   - [Empirical Study](#empirical-study)
   - [Conclusion](#conclusion-1)
 - [Reformulating Branch Coverage As A Many-Objective Optimization Problem](#reformulating-branch-coverage-as-a-many-objective-optimization-problem)
   - [Test Data Generation](#test-data-generation)
   - [Many-objective Optimisation Problem](#many-objective-optimisation-problem)
   - [Problem Formulation](#problem-formulation)
   - [Algorithm](#algorithm)
   - [Empirical Evaluation](#empirical-evaluation)
   - [Related Work](#related-work)
   - [Conclusions and Future Work](#conclusions-and-future-work-1)
 - [References](#references)
 
 
## Introduction
To automatically generate test cases, there are two things we needs to do: 
- Search through the problem space to generate a random population of test cases.
- Apply a genetic algorithm to evolve the randomly generated test case population according to a fitness function.

There are different approaches to which this method can be applied: single test generation (target one goal at a time), or whole test suite generation.  
After executing our own literature survey of six papers treating the matter, we have created a clear overview of what search algorithms [[1](#1)] exist to find a suitable test case population. 
We have obtained a deeper understanding of the genetic algorithm applied [[2](#2)][[3](#3)], and we have learnt the mechanisms behind whole test suite generation [[4](#4)]. 
We have also studied the effectiveness of whole test suite generation [[5](#5)], and how to optimize the branch coverage criterion [[6](#6)], which is one criterion for a coverage goal.  
We describe the findings in the aforementioned order. 


## Search-Based Software Test Data Generation

### Metaheursitic Search Applications
**Goals:**
 - Coverage of the code.
 - Exercise a specific feature of the program.
 - Disprove grey-box properties.
 - Verify non-functional properties.

### Metaheursitic Search Techniques
Solutions should be encoded such that they can be manipulated by the search and *neighbouring* solutions can be compared. 
Using an *objective function*, *good* solutions are distinguished from *bad* solutions.
The objective function provides guidance to the search for solutions.

#### Hill Climbing
This is a local search algorithm that starts off at a random starting point and tries to look at neighbouring points in the search space to improve the solution.
It is simple and fast. However, it often reaches sub-optimal solutions when there are many local optimal.
This can be slightly avoided by starting at multiple random starting points.

#### Simulated Annealing
Simulated Annealing is similar to Hill Climbing, but probabilistically accepts poorer solutions.
The technique does not always choose the optimal solution, but has a given probability that chooses worse solutions. 
This allows us to find solutions that are "deeper" in the search path.
It starts off with lots of freedom in movement (high probability to choose worse solutions), but limits its movement (low probability to choose worse solutions) as the search progresses.

#### Evolutionary Algorithms
##### Evolution Strategies
Evolution Strategies use mutation: a process of randomly modifying solutions. 

##### Genetic Algorithms
In Genetic Algorithms the search is driven by *recombination*, a mechanism of exchange of information between solutions to create new ones.
Genetic Algorithms keep track of a population of solutions, not only one solution. 
This allows for the sampling of a larger search space.
The population is combined and mutated to evolve successive populations.
Favour 'fitter' solutions to be combined and form even better solutions.
Choosing 'fitter' solutions too heavily can have a negative effect and reduce diversity and can cause *premature convergence*.

**Fitness-proprotionate selection:**
The number of times an individual is selected is proportional to its relative fitness.
Difficulties: maintaining a constant *selective pressure* (grade of selecting different solutions).

**Linear Ranking:**
Individuals are sorted by fitness and a selective bias of *Z* is applied to each of the individuals.
For Z holds: 1 < Z ≤ 2. The linear ranking allocates a selective bias of Z to the top individual, a bias of 1.0 to the median individual, and 2 − Z to the bottom individual.
This differs from fitness-proportionate selection in the sense that a constant bias is applied and thus the selective pressure is more constant and controlled.

**Tournament Selection:** 
In Tournament Selection two individuals are chosen at random. A random number is then chosen and compared to the probability of the better individual being selected.
Afterwards the winner is the parent of the other individual.
Once all the parents have been selected, recombination forms the next generation.
Optionally random crossover is applied, sending children into a new population.
After the recombination, mutation is applied flipping random bits of binary strings with a certain probability.

##### Advanced Encodings and Operators
Problem in encoding: solutions can be close together, but their encoding can be very different.
Real-valued representations tend to outperform binary encodings and are used most often in Evolution Strategies.

### Structural (White-Box) Testing
White-Box Testing is the process of deriving tests from the internal structure of the software under test.

#### Static Structural Test Data Generation
Data is generated by analysis of the internal without actually executing the program.

##### Symbolic Execution
Uses the process of assigning expressions to program variables as a path through the code structure to derive a constraint system in terms of the input variables which describe the conditions necessary for the traversal of a given path.

Backwards path traversal has an advantage over forward traversal in the sense that no storage is required for intermediate symbolic expressions of variables. Forward traversal however allows for early detection of infeasible paths.

Constraint satisfaction problems are NP-complete. However, combinations of heuristics and linear programming techniques can be applied to lighten the process.

Loops are executed *K* times, where *K* is specified by the tester or system.

Procedure calls can be inlined as a solution, however the number of paths can grow rapidly with this approach

##### Domain Reduction
*Constraint-based Testing* builds up a constraint system which describes the test goal. 
*Reachability constraints* withing the constraint system describe the conditions required for the reachability of statements.
*Necessity constraints* describe the conditions that will kill a mutant.

By using Symbolic Execution we can develop constraints in terms of input variables and perform *Domain Reduction*  to attempt a solution to the constraints. 
> Large search space ---constraints---> Reduction of search space

##### Dynamic Domain Reduction
Dynamic Domain Reduction is a *static analysis technique* that starts with domains of the input variables and reduce these dynamically during the symbolic execution stage in contrast to normal Domain Reduction. 

*This technique still suffers from difficulties with computed storage locations, loops and non-ordinal variable types(e.g. Enums).*

#### Dynamic Structural Test Data Generation
*Dynamic methods* use input to execute a program and instrumentation to observe the results.
Many of the issues present in Symbolic Execution are resolved by directly resolving e.g. pointer values and array subscripts.

##### Random Testing
Executes the program with random inputs and observes executed program structures.
When using this technique, code that has low probability of being executed is often not covered, in such cases more directed search techniques are required.

###### Applying Local Search
In the Local Search technique, the tester selects a path through the program and produces a straight-line version of that path.
Path constraints are constructed with a constant for each constraint estimating the satisfiability of the constraint.
A function *f* is constructed out of all of these constraints.
*f* provides an estimate of the satisfiability of all constraints.
Using numerical maximisation techniques, the input values are maximized to satisfy as many constraints as possible.
One major drawback of this is that run-time errors can occur, since the dependency between constraints are not taken into account, which could create sequences that in practice are impossible to execute.

Improvements to this technique emerged to resolve this problem.
The search was now targeted with the satisfaction of each constraint instead of all the constraints. 
If during the execution an undesired branch is taken, a local search is started that is targeted to minimize branch distance of the desired branch, also known as the *branch distance*.
Creating these inputs is done using the *alternating variable* method, that starts with an *exploratory phase* which probes neighbouring constraints by increasing or decreasing only that specific variable.
If the move leads to an improved objective value, a *pattern phase* is entered.
In this phase a larger move is made in the direction of the improvement, this is repeated until a minimum for that objective function is found for that variable.
After this is complete, other variable are explored.

These techniques suffer however from one main problem: the final solution is highly dependent on the starting solution, which can be very wasteful if unfavorable values are chosen.
A solution to this is the creation of an *influences graph*, that is created via dynamic data flow analysis and is used to detect which input variable influence the outcome at a branching node.
Additionally a risk analysis on the input variables can be performed to decided if they could violate the already successful sub-path.

###### The Goal-Oriented Approach
A Goal-Oriented Approach is a technique with a set goals(e.g. statement coverage).
Branches are prioritized by analyzing the control flow graph.

A branch is *critical*, when if chosen, the control flow won't reach the target node.
The objective function is then associated with the alternative branch.
The alternating variable search method is used, if the required inputs cannot be found, the process terminates with the target unexecuted.

A branch is *semi-critical* if it leads to a target node via the back edge of a loop. The alternative branch will lead directly to the target node.
Inputs for this are sought, but when they are not found, the alternative branch is iterated next.

A branch is *non-essential*, when it is neither *critical* nor *semi-critical*.
These branches do not affect the reachability of target node and are thus are allowed to be executed.

The Goal-Oriented Approach suffers from similar problems as the Local Search approach. The removal of the requirement to select a path introduces new ways in which the test data search can fail.
Instead of performing local searches, this technique can also use global search with Genetic Algorithms, but even these have their problems.

Main problem: data dependencies are not taken into account. An attempt to solve this solution is made in the Chaining Approach

###### The Chaining Approach
Chaining uses an *event sequence* as a intermediate means of deciding the type of path required to reach the target node. 
An *event sequence* is a succession of nodes that should be executed.
Initial sequence consists of *start* and *target* nodes.
This sequence is filled in once the test data search encounters difficulties.

**Event Sequence:** `<e_1, e_2, ... e_k>`, where `e_i = (n_i, C_i)` where `n_i in N` is a program node and `C_i` is a set of variables referred to as a constraint set.

Every two adjacent events should not have variables in the constraint set modified, this ensures that a *definition-clear path* must be taken from one node to the other node.

A branch is *critical* if there does not exist a definition-clear path between the two program nodes, even though such a path does exist via another branch.
A branch is *semi-critical* if it is not-critical, the target node is control dependent on path p and there does not exist an acyclic definition-clear path.
A branch is *non-essential* if it is none of the above.

When inputs cannot be found to change the flow of control such that a critical branch is avoided, the starting node is declared as being a problem node.
The technique then searches for alternative event sequences.

The Chaining Approach organises generated event sequences in a tree form. 
In this tree, the root node represents the initial event sequence and subsequent nodes are the resulting event sequences of all the corresponding problem nodes.
Backtracking techniques of depth *n* can be used in more complicated instances to look for last definition statements of variables.
The tree is explored using a depth-first strategy with a specified depth limit.
This technique covers a larger set of programs than the Goal-Oriented Approach, but as search times increase, local search can become trapped in difficult search spaces.

##### Applying Simulated Annealing
The "neighbourliness" structure of the integer and real variables is defined as the range of values around each individual value.
Boolean, enumerated types and all other order insignificant variables are considered as neighbours.
The objective function is the branch distance of the required branch when control flow diverges from the intended path. 
To reduce search becoming stuck in a local optima, the restriction that a solution must conform to an already existing sub-path is lifted.

##### Applying Evolutionary Algorithms
###### Coverage-Oriented Approaches
Coverage-Oriented Approaches work by rewarding on the basis of covered program structures.
Search tends to reward long paths through the test subject.
When using this technique, generally there is a lack of guidance provided for structures, which are only executed with values from a small portion of the overall input domain.
Therefore, it is difficult to expect full coverage of these techniques for any non-trivial programs.

###### Structure-Oriented Approaches
Structure-Oriented Approaches take a *divide and conquer* approach to obtain full coverage. In this approach, a separate search is done for each uncovered element.

**Branch-Distance-Oriented Approaches**: 
Branch-Distance-Oriented Approaches use branch predicates in combination with Random Search or Genetic Algorithms for the more difficult cases.
A path is chosen and the relevant branch predicates are extracted. A Genetic Algorithm is then used to find input data that satisfies all branch predicates at once.
Since this technique requires rigid constraints, the chance of getting stuck in local optima is high.
It would be better if more feedback could be provided via the objective function.
This is where control oriented approaches come into play.

**Control-Oriented Approaches**:
In this technique, the objective function considers branching nodes that need to be executed to cover a desired structure.
This technique needs the control dependence graph of the test subject to identify each of these branching nodes.
The problem with the Control-Oriented Approach is that the objective function gives no guidance on how to change the flow of execution at control dependent nodes, since no distance information is exploited from branch predicates.

**Combined Approaches**
Combined Approaches make use of both branch distance and control information for the objective function.
The technique suffers a bit from local optima, but this can be resolved by using a form of *approximation level*.
The result looks quite a bit like the Control-Oriented approach, but by using branch distance calculations the objective function is greatly improved.

##### Objective Functions for Different Structural Coverage Criteria
Structural criteria are divided into four categories, where the general objective function is defined as: `approach_level + m_branch_dist`.
These two variables depend on the coverage type in question.

**Node-Oriented:**
Aim to cover specific nodes of the control flow graph.

**Path-Oriented:**
Require the execution of specific paths through the control flow graph.

**Node-Path-Oriented:**
Include branch coverage and LCSAJ (linear code sequence and jump) coverage.

**Node-Node-Oriented:**
Aim to execute a certain sequence of nodes through the control flow graph, without the specification of a concrete path between each node.

##### Control-Related Problems for Objective Functions
A major problem is covering nested structures within loops that require many iterations.
Some approaches try to resolve this by considering branches that miss the target in iterations of the loop, as critical branches.
However, this leads to penalisation of individuals in the first iteration of the loop.

A second problem is the assignment of approach levels for some classes of programs with unstructured control flows.
A solution to this is to use *optimistic* or *pessimistic* approach level allocation strategies.
In an optimistic strategy, a control dependent branching node is allocated its approach level on the basis of the shortest control dependent path from itself to the target node.
In a pessimistic strategy, a branching node is allocated its approach level on the basis of the longest control dependent path to the target node.
Both strategies have different effects on the progress of the search, but it still remains an open problem which strategy works the best in general.

##### Branch-Distance-Related Problems for Objective Functions
The global search techniques still have some problems in hostile search landscapes containing large plateaux or several local optima.
Plateaux are easily created by a simple "flag" variable.
With these types of flags, the evolutionary search performs no better than random search.

A solution is removing a flag from the branch predicate by performing program transformation.
This is however not always possible.

Alternatively a sequence of nodes to be executed prior to the branch predicate containing the flag can be identified.
However, the approach has problems avoiding optional assignments to flags within loop bodies.

A second problem is that there exists the possibility that the branch distance calculation deceives the search.

A third problem can occur with nested branch predicates where a solution for subsequent conditions must be found without violating any of the earlier conditions.

##### Applying Variable Dependency Analysis
Variable dependency analysis allows the determination of the subset of input variables that cannot affect the outcome at a branch predicate.
This allows for reduction of the search space.

##### Generating Input Sequences
Another problem for structural test data generation are test objects with internal states.
In these situations an input sequence is required to cover certain structures.

A further problems with state-based systems is their tendency to make use of flag and enumeration variables to control the current state.

##### Use of Evolutionary Algorithms: Encodings and Operators
Early work used binary encoding, but it is common that variables will often only have valid values within a subset of possible bit patterns at the binary level.
Using evolutionary algorithms on binary encoding can cause corruption with restricted types in the crossover and mutation operators.
This problem can be resolved by using real-valued encodings, which removes the need to encode and decode the input vector into and out of a binary format and prevents the variables from going out of range.

#### Future Directions for Search-based Structural Testing
Problems that need further research:
 - Flag and enumeration variables
 - Unstructured control flow and state behaviour
 - Test data cannot be found
 - Does not support programs involving strings and dynamic data structures such as lists, sets, etc
 - Problems with dynamic types including comparison of pointer locations.
 - Complications in object-oriented system due to internal states and polymorphic types.

Future fields of research: programs using information from files and sockets.

### Functional (Black-Box) Testing
Functional (Black-Box) Testing uses metaheuristic search techniques to test the logical behaviour of a system, as described by some form of specification.

#### Generating Test Data from a Z Specification
Z specification describes the state space of the system in a schema consisting of disjunctions, which contain conjunction of input variables and predicates.
Each disjunct is considered as a *route* through the system. Genetic Algorithms are used to search for test data for each route.
Each conjunct is evaluated using a distance based approach, to the branch distance calculations used in Structural testing.
The overall fitness of the route is the summation of the distances for each of its conjuncts.

#### Testing Specification Conformance
Conformance of the implementation to its specification is checked by executing the test object with the generated test data and then validating the output against the specification.
A failure is found when an input situation is discovered that satisfies the pre-condition of a function, but for which the outputs violate the post-condition.

#### Future Directions for Search-based Functional Testing
Search-based functional testing is less active compare to structural testing.
A few downsides:
 - A mapping needs to be provided from the abstract model of the specification to the concrete form of the implementation.
 - Can consist of an extremely large search space.
 - Test sequences may need to be generated to put the system into some valid state in order for the property of interest to be test.

### Grey-box Testing
Grey-box testing combines both structural and functional information for the purpose of testing.

#### Assertion Testing
Assertions specify constraints that apply to some state of a computation. When an assertion evaluates to false, an error has been found in the program.
The assertion code is formed into a function with the original assertion comment region and replaced with a call to the corresponding function.
The goal is then to execute a false assignment to the assertion variable statement within the function and thereafter avoiding all true assignment to the variable.

#### Exception Condition Testing
Similar idea as Assertion Testing, but this technique tries to trigger exceptions.
Problem here was that the Test Data was often not possible when using the software and thus the violations were false positives.

#### Future Directions for Search-based Grey-Box Testing
 - Component-reuse Testing: searching for test data that causes a component to be called where its usage assumptions are broken.
 - Black-Box assertions used as test oracles, offering further automation.

### Non-Functional Testing
In this area there is a large concentration on best and worst-case execution times of real-time systems.

#### Execution Time Testing
Execution time testing involves attempts to find the worst-case execution time (WCET) or the best-case execution time (BCET) of a system in order to determine whether it is compliant with its timing constraints.

##### Static Analysis
Static analysis can be used to derive WCET and BCET. This is done by examining the possible execution paths and modelling timing behaviour at the hardware level.
The primary step needs assistance from the programmer, since information is required regarding the infeasible paths, and the maximum number of iterations for each loop appearing in the code.
Major problems with this technique are the possibility of simulation errors and the need for human involvement.

##### Search-based Execution Time Testing
The objective function is the execution time of the system as executed with some input. The search attempts to maximize (WCET) or minimize (BCET).
Search-based techniques cannot guarantee the actual WCET or BCET, but the best found result can be used to form an interval with the time obtained from static analysis within which the actual extreme execution time most probably lies.

##### Future Directions for Search-based Execution Time Testing
It is important to look into using a combination of static analysis and search-based techniques.
Search-based techniques could be used to verify path feasibility for static analysis.

Further it is possible to combine the objective function with those used by structural test data generation to ensure that timing behavior involving all branches is explored.
Especially procedural code has been researched. It would be interesting extending these techniques to object-oriented software.

#### Future Directions for Search-based Non-Functional Testing
Future research areas include:
 - Resource usage: memory, storage requirements.
 - Memory leak detection
 - Stress testing, security testing, etc.

### Conclusions
*Symbolic Exectuion* evaluates program code in order to build up a system of constraints describing the test goal.
Becomes problematic when using loop and in cases where computer storage location need to be determined.
Alternatively *dynamic approaches* execute the program with some input and examining the effects via some form of instrumentation.
Helps to resolve problems we saw with symbolic execution such as pointer locations, which are known at run-time.

Metaheursitic Techniques allow a definition of an objective function, which is used to guide test data generation.
The two main types of objectives are *Coverage-Oriented* (objective function: number of program structures executed) and *Structure-Oriented* (objective function: guides search to cover each individual structural element).

*Search-based test data generation approaches*  to *functional testing* have focused on finding inputs such that the program does not meet the specifications. 

*Grey-box* test data generation approaches combine methods used in generating structural and functional testing.
Structure-oriented white-box testing techniques can be used to attempt to induce violations of assertions.


## A Multi-Objective Genetic Algorithm to Test Data Generation

### Introduction
Search based algorithms such as Genetic Algorithms (GA) are used for Test Data Generation.
These algorithms require an objective function to guide the search.
Using a single objective function to guide the search is a limitation, since the there are multiple factors that can be considered to guide the generation task. Examples of such factors are: the ability to reveal certain kind of faults, induced execution time and memory consumption.

The paper presents a framework that uses a Non-dominated Sorting Genetic Algorithm (*NSGA-II*).
The *NSGA-II* algorithms is an algorithm that evaluates the solutions according to Pareto dominance concepts and allows for the comparison of multiple objectives.

### Test Data Generation by a Multi-objective Approach
The framework consist of three modules:

 1. *configModule:* receives the parameters provided by the tester.
 2. *brainModule:* implements NSGA-II and is responsible for the evolution process.
 3. *testingModule:* executes the test cases and reports information about the testing process back to the brainModule.

The representation of the population and the choice of the fitness function are very important to the evolution process and are presented below.

#### Representation of the population
In this work, the individual represents a set of test data to be evolved.
Two possible representations can be used to represent the data in each individual:

 - *Procedural:* the input consists of a sequence of value to execute the program.
 - *Object Oriented:* the input consists of sequences of invocations to constructors and methods, including the correspondent parameters.

#### Genetic operators
Genetic operators transform the population through successive generations and encourage diversity and the adaption characteristics obtained by previous operations.

 - *Procedural:* the operators are applied by considering the type of each provided input.
 - *Object Oriented:* the mutation operator changes, inserts or removes inputs, constructors and methods invocations. The crossover operator selects points that can be in the sequence of method invocations or in the part that contains the parameters.

#### Fitness Evaluation
In this framework there are three fitness objective functions available:
 
 - *Coverage* 
 - *Execution time*
 - *Ability to reveal faults* (Using Proteum it is possible to measure the capability of the test data to kill the mutants generated by a mutation operator, we consider that this test data is capable to reveal the fault described by the corresponding operator).

#### Evolution Process
The Evolution Process consists of the following steps:
1. Test data is generated and the fitness of each of the individuals is evaluated
2. Dominated and non-dominated individuals are identified
3. The criterion given by multiple objectives is applied and selected individuals are used in the evolution of test data.
4. The set of the best solutions are added to the new population and return by the algorithm.
5. The evolution continues(go to step 1) until a stop criterion is reached.
6. Individuals are analyzed and non-dominated solutions are added to the set of non-dominated solutions.

### Evaluation Studies
The described approach was evaluated in three case studies.
The solutions found by NSGA-II dominate all the solutions found by a random strategy (RS). 
It was observed that the NSGA-II cost is almost three times greater than the RS cost and thus suggests that the strategies can be used in a complementary way.
Furthermore, it was observed that the coverage of the criterion and ability to reveal faults seems to be dependent. Future work should look more into this.
A combination that makes use of a multi-objective approach more suitable is one that includes execution time since, improving coverage implies to reveal more faults.

### Conclusions
The paper presented a framework to perform multi-objective test data generation with two strategies: *procedural* and *object oriented*.
The results of three case studies show that the multi-objective algorithm gets better solutions and improvements for all the objectives.
The solutions of the NSGA-II algorithm always dominated the random strategy.
Future work can be done in the evaluation of other objectives, as well as other meta-heuristic algorithms.


## Evolutionary Testing of Classes

### Automated Test Case Generation
Automated Test Case Generation consists of creating an object, changing the state of that object and proper method calls with matching corresponding values. 
Unit tests of classes should include all possible constructors and method invocations in order to analyze all cases.
Genetic algorithms use chromosome encoding ensuring that test cases for hard-to-reach code fragments are implemented by creating sequence of program calls.

### Genetic Algorithms for the Unit Testing of Classes
Genetic algorithms are broadly applied in the testing of procedural programs.

#### Unit testing of classes
Unit testing of classes considers that each class being tested consists of constructor(s), method invocation within a certain set of input parameters

#### Evolutionary testing
Evolutionary testing requires certain steps to be implemented in the algorithm, including a definition of parameters, maximum execution time, targets and automatically generated tests.
The algorithms result in generating test cases that are properly defined for at least one target (e.g., branches). 

#### Chromosomes
Chromosomes include a number of input values to be used as parameters of methods and constructors that belong to a certain class.
The specification of chromosomes thus must include the specification of the methods and the proper sequence of parameters.

#### Mutation operators
By randomly selecting mutation operators, chromosomes are being changed by the genetic algorithm.
##### Mutation of input value
Randomly generated value replaces another with the same type.
##### Constructor change
One of the constructors is randomly replaced.
##### Insertion of method invocation
New methods are generated with generated input values
##### Removal of method invocation
Randomly chosen methods are being removed.
##### One-point crossover
During a union of chromosomes at a randomly selected line, the confliction variables are being renamed, constructors are added or removed, methods are added or removed.

#### Input generators
Input generators are defined in the declaration of method signatures. 
Default input generator are used if only type names are declared.
The following types for generated rules are applied:
- Integer and real numbers (only in the interval from 0 to 100)
- Booleans (50% probability for true, and 50% for false)
- Strings (a-zA-Z0-9) 

### The Tool eToc
The Evolutionary Testing of Classes tool (eToc) uses the genetic algorithm for test case generation in the terms of the Java language.
It consists of the following components:

#### Branch instrumentor
Branch instrumentor generates instrumented code where each control flow branch is individually described and traced in the process of execution. 

#### Chromosome former
Produces new chromosomes and modifies existing chromosomes, according to method signatures. 

#### Test case generator 
Within the inner loop of the genetic algorithm every test case, identified by a chromosome is implemented.

##### Test case executor
After execution of test cases, a summary containing information of successfully passed test cases, as well as failed ones and not proper assertions, is being generated.

### Experimental Results
Experimental results have been generated by using eToc tool.
The unit testing have been applied to classes existing in the standard Java library.

#### The procedure
The Classes Under Test have been implemented using *Branch instrumentor*.
After that *Test case generator* generates JUnit test cases for each Class Under Test. 

#### Classes Under Test
The classes that have been tested with the given tool belong to standard Java library and SDK version 1.4.0.

#### Results
The outcome of the generated tests that use chromosomes repeat the theoretical assumption that the branch coverage conducted by the experiment equals to 100%.
On the other hand, the effectiveness of automatically generated tests to detect faults is to some extent high but not ideal due to data flow of attributes or change of the state of the object. 

#### Conclusions and Future Work
The usage of genetic algorithm for the purpose of unit testing is considered as quite impressive.
The generated test suites are completed in acceptable computation time.
Future work will be dedicated to discover various criteria for evaluation of effectiveness of the algorithm as it concerns data flow, fault exposing capability.
Moreover, future work is recommended to include multi-class testing.


## Whole Test Suite Generation

### Research Goal
Software testing is an essential component of software development. 
Over the years, smart programmers have designed techniques that automatically produce test cases, and we can now generate entire test suites with high coverage. 
However, one problem remains, known as the *oracle problem*. 
Automatic oracles that assert what the outcome of a test should be are possible when it comes to explicit events such as 'the program should not crash'. 
But when dealing with more complex outcomes, human intervention is necessary to specify oracles.
For this to be feasible, test generation must aim not only at high code coverage, but also at small test suites that make manual oracle generation as easy as possible. 
The developer who needs to specify an oracle does not have to go through thousands of lines of code to manually add assertions, but through a set limit for the generation. 
The goal of this paper is therefore to make such technology possible by achieving the following:
- High code coverage
- Small yet representative test sets
- Whole test suite generation

This technology was made available through the EvoSuite framework. 

### Background Information
To generate effective test cases, the guiding medium used most commonly is the code coverage criterion, which represents a finite set of coverage goals (e.g. branch coverage, path coverage, etc). 
Many techniques (such as the one mentioned in the previous papers) are already being used by software engineers, but they all have certain drawbacks.  
One example of such drawback is that when applying the work mentioned in Paper 2, handling the growth/reduction of generated test cases requires quite a bit of effort. 
Additionally, there is description of a technique that also takes into account collateral coverage, which represents the amount of collateral targets that are accidentally covered. 
However, no particular heuristic exists to help cover these targets.  
All approaches mentioned so far target a single test goal at a time, which is the predominant method. 
But there is one that uses a single sequence of function calls to maximise the number of covered branches while watching out for the length of such a test case. 
A drawback of such approach is that there can be conflicting testing goals and it might be impossible to cover all of them with a single test sequence, regardless of its length.  
Another method proposed optimises entire test suites with a search algorithm with respect to mutation analysis. 
However, the strong limitation is having to manually choose and fix the length of the test cases, which does not evolve during the search.  
To deal with these drawbacks, the paper therefore approaches a new method with the intent to satisfy the chosen coverage criterion with the smallest possible test suite, which makes use of a search algorithm applied on a population of test suites. 

### Test Suite Optimization
To execute whole test suite generation, the paper describes the use of a Genetic Algorithm (GA) as search algorithm, on a population of test suites. It also focuses on *branch coverage* as test criterion. 

#### Genetic Algorithm
This algorithm starts with a random population of test suites and evolves this population.
It stops when a solution is found which fulfills the coverage criterion or when the set resources (such as time or number of fitness evaluations) have been used up. 
For each generation, the new population is initialised with the best individuals of the last generation. 
The new generation is then filled up with individuals produced by rank selection, crossover, and mutation. 
Either the offspring or the parents are added to the new generation, depending on fitness and length constraints.   

#### Representation
To apply this search algorithm, the problem needs to be defined and a solution for that problem needs to be represented accurately. 
The paper declares the solution to be a *test suite*, which is represented as a set of test cases. 
A test case is represented by a sequence of statements.
The length of the test suite is defined as the sum of the lengths of its test cases. 
Test cases contain statements and each statement has a value and a type. This paper identifies five types of statements:
- **Primitive** statements, which represent Boolean, String, and enumeration variables, but also arrays of any type.
- **Constructor** statements, which generate new instances of any given class.
- **Field** statements, which access public member variables of objects. 
- **Method** statements, which invoke methods on objects or call static methods.
- **Assignment** statements, which assign values to array indices or to public member variables of objects, but do not define new values.

The *test cluster* for a given system under test defines the set of available classes, their public constructors, methods, and fields. 
This chosen representation has a variable size for the test suite for one good reason: when a new software should be tested, the optimal number of test cases and their respective optimal length are not known.
They need to be searched for. 

#### Fitness Function
Since this paper focuses on branch coverage, it brings a definition of the latter: a program that contains structures such as if or while statements guarded by logical predicates achieves full branch coverage when each of these predicates evaluate to true and to false in the test suite. 
There are instances where branches can be infeasible, which means that there exists no input that evaluates the predicate such that this particular branch is executed.  
The optimal solution for the test suite is defined as a solution that covers all feasible branches/methods and is minimal in the total number of statements in its test cases. 
In order to get to the optimal solution, it is required to guide the selection of parents for offspring generation. 
For this, this paper speaks of a fitness function that makes a selection based on better coverage. 
If two test suites have the same coverage, the selection is then based on the number of statements; the shorter one is selected.  
The fitness value is measured by executing all tests in a given test suite, keeping track of the set of executed methods as well as the minimal *branch distance* for each branch. 
The branch distance is a heuristic to guide the search for input data to solve constraints in the logical predicates of the branches.
The fitness function estimates how close a test suite is to covering *all* branches of a program, and is expressed as a function of the test suite that takes the total number of methods in the program, subtracts the number of executed methods, and adds the sum of all branch distance results since the first generated test suite. 

#### Rank Selection, Crossover, and Mutation
##### Rank Selection
Rank selection is based on the fitness function. In case of ties, better ranks are assigned to smaller test suites. 

##### Crossover
Crossover generates two offsprings from two parent test suites based on a random value *a* from [0, 1]. 
For the first offspring, the *a*th test case from the first parent is combined with the (1 - *a*)th test case from the second parent. 
For the second offspring, the *a*th test case from the second parent is combined with the (1 - *a*)th test case from the first parent. 
Because the test cases are independent, this will always generate valid offspring test suites. 
With this set up, no offspring will have more test cases than the largest of its parents. 

##### Mutation
There are three types of mutations:
- **Remove**, which removes test cases in a test suite with a certain probability.
- **Change**, which changes the value of primitive inputs.
- **Insert**, which inserts a new statement at a random position in a test case. 

Each of these types has probability 1/3 of occurring. 
On average, only one of them is applied, but it could be that all of them are applied. 
When a test suite T is mutated, each of its test cases has equal probability of being mutated 1/|T|. 
On average, only one test case mutates.

### EvoSuite
The EvoSuite tool implements the approach presented in this paper, for developing JUnit test suites for Java code. 
This tool works at the bytecode level and collects all information for the test cluster from the byte-code via Java Reflection.
This means that EvoSuite could also be used for other programming languages that compile to Java byte-code (such as Scala or Groovy). 
Furthermore, EvoSuite treats cases from a switch.case construct like an individual if-condition, making the number of branches at byte-code level large than at the source code level.  
During test suite generation, to produce test cases that are compatible with JUnit, EvoSuite accesses only the public interfaces for test generation; any subclasses are also considered part of the unit under test, to allow testing of abstract classes. 
Before returning a test suite, the tool applies a simple minimization algorithm that removes each statement one at a time until all remaining statements contribute to the coverage. 
This contributes to reducing the amount and length of test cases.  
There are a few language-related problems that are encountered by the EvoSuite tool. 
In particular, classes using Java Generics are problematic, as type erasure removes much of the useful information during compilation and all generic parameters are considered as Object. 
To overcome this problem, EvoSuite always inserts an Integer object into container classes, in order to cast returned Object instances back to Integer. 
However, container classes must be identified manually, but the EvoSuite team is working on it.  
Additionally, there are security measures that need to be undertaken during test execution.
There is a security manager that controls what permissions are granted. 
This is useful when the program requires a network connection or filesystem access for some purpose. 
It is not desirable to have the test suite opening up a random network connection or manipulating the filesystem in random ways. 

### Difficulties
As previously mentioned, the EvoSuite tools has difficulties dealing with Java Generics, and security concerns affect the coverage goal the tool can fulfill.  
Additionally, there are a few threats to validity that must be considered when applying the technique described in this paper:
- Construct validity
- Internal validity
- External validity

Construct validity is compromised because the approach of "entire test suite" has some drawbacks over "one target at a time".
Priority is given to coverage and length of the test suite becomes a secondary goal. 
But if a much larger test suite would lead to only slightly better coverage, then this test suite will be qualified as "more optimal", while this is clearly not the case. 
Moreover, this performance measure does not take into account how difficult it would be for a developer to manually evaluate the test cases for writing assertions.  
Internal validity is affected by the fact that this approach makes use of randomized algorithms, which are affected by chance.
Moreover, the entire EvoSuite framework was carefully tested, but testing alone cannot prove the absence of defects.
Experiments were rigorously designed to test EvoSuite for internal validity, but these experiments made use of configurations that were suitable for that sort of experiment. 
EvoSuite claims to outperform other methods in these configurations, but it could well be that other parameter settings actually decrease the performance of the tool.  
The threat to external validity exists because only certain types of software have been tested with EvoSuite and it might not work on others. 
Also, EvoSuite makes use of a GA for optimizing entire test suites, but the superiority of EvoSuite may not hold when dealing with other search algorithms. 

### Conclusion
Although the technique described in the paper and EvoSuite might not be superior to all testing tools, the paper does show that whole test suite generation is superior to a traditional strategy targeting one goal at a time. 
Whole test suite generation achieves **higher coverage** and produces smaller **test suites**.  
It is also proven in this paper that the technique will always converge and therefore find a solution for the optimal test suite, meaning that there will never be a case where the algorithm runs forever by being stuck in the search space.  
Overall, this technique offers a great way for testing an entire code base, where testing oracles can be added in terms of assertions. 
However, the oracle problem is still a difficult obstacle, because even when keeping the tests small, what guarantees the correctness of these tests and the difficulty to comprehend them?


## Effectiveness of Whole Test Suite Generation

### Research Goal
Before the whole test suite generation technique, the traditional method was to carry out a search on each individual coverage goal (e.g. a branch, a statement, etc). 
There are a couple of issues with such an approach:
- *Search budget distribution:* Wasting time on infeasible coverage. Determining whether a goal is feasible or not is an undecidable problem.
- *Coverage goal ordering:* Search for each coverage goal is independent, and potentially useful information is not shared between individual searches. 

The whole test suite generation technique aimed at resolving these issues by targeting multiple coverage goals at the same time. 
The advantage of this new approach is that both the questions of how to distribute the search budget between individual coverage goals at the same time and in which order to target these goals disappear. 
Large improvements have been reported for both branch coverage and mutation testing when using this method. 
However, despite the evidence of higher overall coverage, it is unsure as to how this whole test suite generation technique influences individual coverage goals. 
Is the higher coverage due to more *easy* goals being covered? 
Is the coverage of difficult goals just avoided? 
This paper provides us with an empirical study on how the whole test suite generation compares to the traditional approach. 
It aims as studying whether the traditional approach is better at targeting more specific goals than the new whole test suite approach. 
To perform this study, both approaches are performed on 100 Java classes and the results are compared. 

### Empirical Study
This empirical study aims at answering the following three questions:
- **Q1:** Are there coverage goals for which the traditional *OneBranch* approach performs better?
- **Q2:** How many coverage goals found by the *Whole* approach get missed by *OneBranch*?
- **Q3:** Which factors influence the relative performance between *Whole* and *OneBranch*?

100 Java classes were randomly chosen out of 100 randomly selected projects. 

#### Q1
After performing the empirical study, it was found that *OneBranch* performed better for over 50 coverage goals. 
Three of these coverage goals were never covered by *Whole*. 
However, there are more than half cases for which *Whole* performed much better. 
In other words, even with three difficult goals that *OneBranch* can cover, many more (82 times) difficult branches were covered by *Whole* and ignored by *OneBranch*.

#### Q2
As mentioned before, *Whole* is able to handle 82 times more difficult branches than *OneBranch*. 
It is then assessed that the *Whole* approach leads to higher coverage, even for difficult branches. 

#### Q3
The researchers quantified the odds that *Whole* performs better to cover a certain goal compared to *OneBranch*. 
After some research and high usage of statistical techniques, there was no correlation that could be found between quantification of performance and other factors. 
This means that there is no evidence of a factor that strongly influences the relative performance between *Whole* and *OneBranch*. 

### Conclusion
Existing research has shown that the whole test suite approach lead to better coverage results than the traditional one target at a time approach. 
There was reasonable doubt on whether this would be the case when targeting difficult goals. 
This paper performed an in-depth analysis to study if this doubt could be confirmed in practice. 
It was found that indeed, the *Whole* approach had more difficulties when targeting difficult goals. 
However, these cases are very few compared to the cases for which *Whole* performed significantly better. 
This paper therefore provides more support to the validity and usefulness of the whole test suite generation method in the context of software testing.


## Reformulating Branch Coverage As A Many-Objective Optimization Problem

### Test Data Generation
Represents a search problem that aims at testing as many program element as possible (to maximize the number of branches).
Whole test suite approach that was previously described as preferable to the single-branch at a time method.
A new branch coverage will be proposed as many-objective optimisation problem alternatively to whole test suite approach that aggregates multiple objectives into a single value.
A highly scalable many-objective genetic algorithm, called MOSA(Many-Objective Sorting Algorithm) is proposed to many-objective branch coverage problem.

### Many-objective Optimisation Problem
The problem itself includes discovering a set of test cases in order number of covered branches to be maximised. 
By using genetic algorithms (search-based testing) in the testing of procedural programs one branch at a time is targeted with the help of fitness function to estimate the distance between each test case trace and the branch.
Single-target strategy encounters several issues (some targets are more difficult to cover) that whole suite approach is aiming to overcome.
Studies on multi-objective optimisation have indicated that it is not effective in some kind of problems, but more efficient than single-objective approach.
In branch reformulation it is suggested different branches as different object to be optimised.
The candidate solution is test case and the optimisation is able through fitness according to all yet uncovered branches.
The MOSA algorithm adapts multi-objective genetic algorithm to alter the selection scheme.
Comparison between whole suite approach and MOSA is realised on 64 Java classes and results show the preference of MOSA in terms of higher (or the same) coverage and faster convergence.

### Problem Formulation
Analysis considering the differences and characteristics of whole suite approach (based on single-objective formulation) and many-objective approach.

#### Single Objective Formulation
A single test suite must be discovered that provides coverage (sum of the individual branch distances) of all branches and minimises the fitness (full branch coverage) functions.
The fitness function determines the coverage of a certain test to all branches of the program.
The fitness function consists of all branches at the same time and all distances by summarising all contributors from the individual branch distances.
In such way whole suite approach transforms multiple search goals into single search goal.

#### Many-Objective Formulation
Using this approach, the objectives to be optimised consist of all branch distances of all branches in the class under test and minimises fitness functions for all branches.
Formulated like this, the fitness is a vector (not a single score) and candidate solutions (test cases, not suites) are described using Pareto dominance and Pareto optimality to evaluate the test case.
Single-objective optimisation problems provide only one solution, while  multi-objective is possible to prompt a set of Pareto-optimal test cases corresponding to trade-offs in the objective space.
The goal of presented approach is only the test cases that maximises the total coverage by covering previously uncovered branches to be discovered.

### Algorithm
An analysis of previous methods will be presented including limitations and main characteristics, the new proposed method will be summarised in the second part.

#### Existing Many Objective Algorithms
The previous studies of multi-objective evolutionary algorithms show that they are successfully applied but not efficient in solving multi-objective optimisation problems(software refactoring, test case prioritisation, etc.).
To reduce the limitations, new algorithms, that modify the Pareto dominance relation, have been suggested.
All proposed multi-objective algorithms have been applied for numerical optimisation problem with no more than 15 objectives.

#### MOSA: a New Many Objective Sorting Algorithm
Previous studies on that many-objective reveal that the proportion of non-dominated solutions increase exponentially with the number of objectives.
According to that assigning a preference for selection is not  possible, thus the search process is transformed to random one.
As a consequence, the search is focused to the neighbouring test cases of uncovered branches of the program.
To overcome these limitations (changing the preference), a new method is proposed.
By definition: Given a branch bi, a test case x is preferred over another test case y if and only if the values of objective function fro bi satisfy the following condition: fi(x) < fi(y), where fi(x)denotes the objective score of test case x for branch b. 
The proposed algorithm (MOSA) provides reference ordering during the selection process.
The search with MOSA is targeted at uncovered branches, it collects the shortest covering test cases in external data structure in order to form the final test suite.

#### Graphical Interpolation of Preference Criterion
A comparison between traditional algorithm with non-dominated ranks and the proposed preference based many-objective genetic algorithm.

### Empirical Evaluation
It aims at analysis of performance and productivity of MOSA in terms of coverage and convergence in comparison with single-objective whole suite approach.

#### Prototype tool
The experiment was realised with prototype tool, called EvoSuite test data generation framework.

#### Subjects
For testing purposes, randomly were selected 64 Java classes.

#### Metrics
As metrics technique of effectiveness is used number of branch coverage divided by the total number of the branches in that class. 
Efficiency is measured through consumption of search budget and the number of executed statements.

#### Experimental Protocol
The experiment is limited by time - either by reaching full branch coverage, or search budget is complete, or total assigned time is elapsed.
Each test is run 100 times for each search strategy.
Default parameter values, used in EvoSuite, were applied in order performance of the algorithms to be evaluated.

#### Results
Based on the results of the experiment, it can be concluded that whole suite approach was better that MOSA in 9 of the cases, while on the other hand MOSA performed better in 42, and in 13 no considerable difference was detected.
In conclusion, MOSA managed to produce better branch coverage than whole suite approach, and when the same coverage is achieved, MOSA shows better convergence.

#### Qualitative Analysis
As stated earlier, MOSA solutions are represented by test case (not suite) within the current population of each branch that is yet not covered.
While the whole suite fitness is applicable in increasing the global number of covered goals, but the single-objective coverage of a single test may remain unused.

#### Threats to Validity
Threats to Validity provide correlation between theory and experimental results.
The measure values that were used to estimate the efficiency and effectiveness - branch coverage and number of statements executed are commonly used in other experiments.
The experiment for both methods was carried out 100 times with default values in order to avoid fault results .

### Related Work
The experiment and studies of search algorithms for test data generations are motivated by their application and practical representation.
It should be outlined that all previous studies of multi-objective approaches for evolutionary test data generation aim at one branch a time, but the number of objectives remains small.
On the contrary, the proposed approach targets many-objective branch coverage problem while minimising simultaneously the distances between the test cases and uncovered branches in the class under test.

### Conclusions and Future Work
MOSA introduces a new approach towards multi-objective branch coverage by reducing scalability practical problems with working with great number of branches.
That resulted in statistically better results that whole suite approach.
Regarding the future work, it will be expanded to include more java classes.
New experiments will include new adequacy testing criteria - statement coverage, mutation testing, etc. in order to test whether multi-objective problem to be formulated and solved through MOSA.
In addition, a combination with other non-coverage criteria (execution time, memory consumption) will be investigated.


## References
1. <div id="1"/>*Phil McMinn*. Search-Based Software Test Data Generation: A Survey. Software Testing, Verification and Reliability, vol. 14, no. 2, pp. 105–156, 2004.
2. <div id="2"/>*Gustavo  H.L. Pinto, Silvia R. VErgilio*. A Multi-Objective Genetic Algorithm to Test Data Generation. 22nd IEEE International Conference on Tools with Artificial Intelligence (ICTAI),  vol. 1, pp. 129–134, 2010.
3. <div id="3"/>*Paolo Tonella*. Evolutionary testing of classes. In Proceedings of the 2004 ACM SIGSOFT international symposium on Software testing and analysis (ISSTA '04). ACM, New York, NY, USA, 119-128.
4. <div id="4"/>*Gordon Fraser and Andrea Arcuri*. Whole Test Suite Generation. IEEE Transactions on Software Engineering, vol. 39, iss. 2, pp. 276-291, 2013.
5. <div id="5"/>*Gordon Fraser and Andrea Arcuri*. On the Effectiveness of Whole Test Suite Generation. Search-Based Software Engineering, vol. 8636, pp. 1-15, 2014.
6. <div id="6"/>*Annibale Panichella, Fitsum Meshesha Kifetew, and Paolo Tonella*. Reformulating branch coverage as a many-objective optimization problem. Software Testing, Verification and Validation (ICST), 2015 IEEE 8th International Conference on. IEEE, 2015.