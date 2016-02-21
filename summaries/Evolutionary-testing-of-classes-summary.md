# Evolutionary Testing of Classes
**Article:**
*Tonella, Paolo*. Evolutionary testing of classes. In Proceedings of the 2004 ACM SIGSOFT international symposium on Software testing and analysis (ISSTA '04). ACM, New York, NY, USA, 119-128.


## Automated Test Case Generation
Consists of creating an object, changing the state of that object and method call with proper input values. 
Unit tests of classes should include all possible constructor and method invocation in order all cases to be analyzed.
Generic algorithms use chromosome encoding ensuring that for hard to reach code fragments test cases are implemented by creating sequence of program calls.

## Generic Algorithms for the Unit Testing of Classes
Generic algorithms are broadly applied in testing procedural programs.

### Unit testing of classes
Unit testing of classes considers that each class being tested consists of constructor(s), method invocation within sertain set of input parameters

### Evolutionary testing
Evolutionary testing requires certain steps to be implemented in the algorithm including definition of parameters, maximum execution time, targets, automatically generated tests.
The algorithms resuts in gererating test cases that are properly defined for at lest one target. 

### Chromosomes
Chromosomes includes number of input values to be used as parameters of methods and constructors that belong to a ceartain class.
The specification of chromosomes thus must include the specification of the methods and the proper sequence of parameters.

### Mutation operators
By randomly selectiong mutation operators, chromosomes are being changed by the generic algorithm.
#### Mutation of input value
Randomly generated value replaces another with the same type.
#### Constructor change
One of the constructors is randomly replaced.
#### Insertion of method invocation
New methods are generated with generated input values
#### Removal of method invocation
Randomly choosen methods are being removed.
#### One-point crossover
During a union of chromosomes at a randomly selected line, the confliction variables are being renamed, constructors are added or removed, methods are added or removed.

### Input generators
Input generators are defined in the declaration of method signatures. 
Default input generator are use if only type names are declared.
The following types for generated rules are applied:
- Integer and real numbers (only in the interval from 0 to 100)
- Booleans (50% probability for true, and 50% for false)
- Strings (a-zA-Z0-9) 

##The Tool ETOC
eToc tool(evolutionary Testing of classes) gives realisation of generic algorith for test case generation.
It consists of the following components:
 
###Branch instrumentor
Branch instrumentor generates instrumented code where each control flow branch is individually described and traced in the process of execution. 

### Chromosome former
Prodeuces new chromosomes and modifies existing chromosomes according to method signatures. 

### Test case generator 
Withinh the inner loop of the generic algorithm every test case identified by a chromosome is implemented.

#### Test case executor

