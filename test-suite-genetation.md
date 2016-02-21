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
The goal of this paper is to therefore make such technology possible by achieving the following:
- High code coverage
- Small yet representative test sets
- Whole test suite generation

## Background Information
To generate effective test cases, the guiding medium used most commonly is the code coverage criterion, which represents a finite set of coverage goals (e.g. branch coverage, path coverage, etc). 
Many techniques (such as the one mentioned in the previous papers) are already being used by software engineers, but they all have certain drawbacks.  
One example of such drawback is when applying the work mentioned in Paper 2, handling the growth/reduction of generated test cases requires quite a bit of effort. 
Additionally, there is mention of a technique that also takes into account collateral coverage, which is represent the amount of collateral targets that are accidentally covered. 
However, no particular heuristic exists to help covering these targets.  
The approaches mentioned so far all target a single test goal at a time, which is the predominent method. 
But there is one that uses a single sequence of function calls to maximize the number of covered branches while watching out for the length of such a test case. 
A drawback of such approach is that there can be conflicting testing goals and it might be impossible to cover all of them with a signle test sequence, regardless of its length.  
Another method proposed optimizes entire test suites with a search algorithm with respect to mutation analysis. 
However, the strong limitation is having to manually choose and fix the length of the test cases, which does not evolve during the search.  
To deal with these drawbacks, the paper therefore approaches a new method with the intent to satisfy the chosen coverage criterion with the smallest possible test suite, which makes use of a search algorithm applied on a population of test suites. 
## Test Suite Optimization
To execute whole test suite generation, the paper describes the use of Genetic Algorithm (GA) as search algorithm, on a population of test suites. It also focuses on *branch coverage* as test criterion. 

### Genetic Algorithm
This algorithm starts with a random population of test suites, and evolves this population until a solution is found that fulfills the coverage criterion, or the set resources such as time or number of fitness evaluations have been used up. 
For each generation, the new population is initialised with the best individuals of the last generation. 
The new generation is then filled up with individuals produced by rank selection, crossover, and mutation. 
Either the offspring or the parents are added to the new generation, depending on fitness and length constraints.   

### Representation
To apply this search algorithm, the problem needs to be defined, and a solution for that problem needs to be represented accurately. 
The paper declares the solution to be a *test suite*, which is represented as a set of test cases. 
A test case is represented by a sequence of statements, and the length of the test suite is defined as the sum of the lengths of its test cases. 
Test cases contain statements, and each statement has a value and a type. This paper identifies five types of statements:
- **Primitive** statements, which represent Boolean, String, and enumeration variables, but also arrays of any type.
- **Constructor** statements, which generate new instances of any given class.
- **Field** statements, which access public member variables of objects. 
- **Method** statements, which invoke methods on objects or call static methods.
- **Assignment** statements, which assign values to array indices or to public member variables of objects, but do not define new values.

The *test cluster* for a given system under test (SUT) defines the set of available classes, their public constructors, methods, and fields. 
This chosen representation has a variable size for the test suite for one good reason: for a new software to test, the optimal number of test cases and their respective optimal length are not known, and need to be searched for. 

### Fitness Function
Since this paper focuses on branch coverage, it brings a definition of the latter: a program that contains structures such as if or while statements guarded by logical predicates achieves full branch coverage when each of these predicate evaluate to true and to false in the test suite. 
There are instances where branches can be infeasible, which means that there exists no input that evaluates the predicate such that this particular branch is executed.  
The optimal solution for the test suite is defined as a solution that covers all feasible branches/methods and is minimal in the total number of statements in its test cases. 
In order to get to the optimal solution, it is required to guide the selection of parents for offspring generation. 
For this, this paper speaks of a fitness function that selects based on better coverage. 
If two test suites have the same coverage, the selection is then based on the number of statements; the shorter one is selected.  
The fitness value is measured by executing all tests in a given test suite, and keeping track of the set of executed methods as well as the minimal *branch distance* for each branch. 
The branch distance is a heuristic to guide the search for input data to solve constraints in the logical predicates of the branches.
The fitness function estimates how close a test suite is to covering *all* branches of a program, and is expressed as a function of the test suite that takes the total number of methods in the program, subtracts the number of executed methods, and adds the sum of all branch distance results since the first generated test suite. 

## EvoSuite
## Experiments and Results
## Drawbacks and Difficulties
## Conclusion

#### Next Paper
blablabla