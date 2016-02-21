# Whole Test Suite Generation
**Article:** *G. Fraser and A. Arcuri.* Whole Test Suite Generation. IEEE Transactions on Software Engineering, vol. 39, iss. 2, pp. 276-291, 2013.

## Research Goal
Software testing is an essential component of software development. 
Over the years, smart programmers have designed techniques that automatically produce test cases, and we can now generate entire test suites with high coverage. 
However, one problem remains, known as the *oracle problem*. 
Automatic oracles that assert what the outcome of a test should be are possible when it comes to explicit events such as 'the program should not crash'. 
But when dealing with more complex outcomes, human intervention is necessary to specify oracles.
For this to be feasible, test generation must aim not only at high code coverage, but also at small test suites that make manual oracle generation as easy as possible. 
The developer who needs to specify an oracle does not have to go through thousands of lines of code to manually add assertions, but through his or her own set limit for the generation. 
The goal of this paper is to therefore make such technology possible by achieving the following:
- High code coverage
- Small yet representative test sets
- Whole test suite generation
