# Whole Test Suite Generation
**Article:** *G. Fraser and A. Arcuri.* Whole Test Suite Generation. IEEE Transactions on Software Engineering, vol. 39, iss. 2, pp. 276-291, 2013.

## Research Goal
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

## Background Information
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

## Test Suite Optimization
To execute whole test suite generation, the paper describes the use of a Genetic Algorithm (GA) as search algorithm, on a population of test suites. It also focuses on *branch coverage* as test criterion. 

### Genetic Algorithm
This algorithm starts with a random population of test suites and evolves this population.
It stops when a solution is found which fulfills the coverage criterion or when the set resources (such as time or number of fitness evaluations) have been used up. 
For each generation, the new population is initialised with the best individuals of the last generation. 
The new generation is then filled up with individuals produced by rank selection, crossover, and mutation. 
Either the offspring or the parents are added to the new generation, depending on fitness and length constraints.   

### Representation
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

### Fitness Function
Since this paper focuses on branch coverage, it brings a definition of the latter: a program that contains structures such as if or while statements guarded by logical predicates achieves full branch coverage when each of these predicates evaluate to true and to false in the test suite. 
There are instances where branches can be infeasible, which means that there exists no input that evaluates the predicate such that this particular branch is executed.  
The optimal solution for the test suite is defined as a solution that covers all feasible branches/methods and is minimal in the total number of statements in its test cases. 
In order to get to the optimal solution, it is required to guide the selection of parents for offspring generation. 
For this, this paper speaks of a fitness function that makes a selection based on better coverage. 
If two test suites have the same coverage, the selection is then based on the number of statements; the shorter one is selected.  
The fitness value is measured by executing all tests in a given test suite, keeping track of the set of executed methods as well as the minimal *branch distance* for each branch. 
The branch distance is a heuristic to guide the search for input data to solve constraints in the logical predicates of the branches.
The fitness function estimates how close a test suite is to covering *all* branches of a program, and is expressed as a function of the test suite that takes the total number of methods in the program, subtracts the number of executed methods, and adds the sum of all branch distance results since the first generated test suite. 

### Rank Selection, Crossover, and Mutation
#### Rank Selection
Rank selection is based on the fitness function. In case of ties, better ranks are assigned to smaller test suites. 

#### Crossover
Crossover generates two offsprings from two parent test suites based on a random value *a* from [0, 1]. 
For the first offspring, the *a*th test case from the first parent is combined with the (1 - *a*)th test case from the second parent. 
For the second offspring, the *a*th test case from the second parent is combined with the (1 - *a*)th test case from the first parent. 
Because the test cases are independent, this will always generate valid offspring test suites. 
With this set up, no offspring will have more test cases than the largest of its parents. 

#### Mutation
There are three types of mutations:
- **Remove**, which removes test cases in a test suite with a certain probability.
- **Change**, which changes the value of primitive inputs.
- **Insert**, which inserts a new statement at a random position in a test case. 

Each of these types has probability 1/3 of occurring. 
On average, only one of them is applied, but it could be that all of them are applied. 
When a test suite T is mutated, each of its test cases has equal probability of being mutated 1/|T|. 
On average, only one test case mutates.

## EvoSuite
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
It is not desireable to have the test suite opening up a random network connection or manipulating the filesystem in random ways. 

## Difficulties
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

## Conclusion
Although the technique described in the paper and EvoSuite might not be superior to all testing tools, the paper does show that whole test suite generation is superior to a traditional strategy targeting one goal at a time. 
Whole test suite generation achieves **higher coverage** and produces smaller **test suites**.  
It is also proven in this paper that the technique will always converge and therefore find a solution for the optimal test suite, meaning that there will never be a case where the algorithm runs forever by being stuck in the search space.  
Overall, this technique offers a great way for testing an entire code base, where testing oracles can be added in terms of assertions. 
However, the oracle problem is still a difficult obstacle, because even when keeping the tests small, what guarantees the correctness of these tests and the difficulty to comprehend them?