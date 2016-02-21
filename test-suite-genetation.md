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
## EvoSuite
## Experiments and Results
## Drawbacks and Difficulties
## Conclusion

#### Next Paper
blablabla