# Effectiveness of Whole Test Suite Generation
**Article:** *G. Fraser and A. Arcuri.* On the Effectiveness of Whole Test Suite Generation. Search-Based Software Engineering, vol. 8636, pp. 1-15, 2014.

## Research Goal
Before the whole test suite generation technique, the traditional method was to carry out a search on each individual coverage goal (e.g. a branch, a statement, etc). There are a couple of issues with such an approach:
- *Search budget distribution:* Wasting time on infeasible coverage. Determining whether a goal is feasible or not is an undecidable problem.
- *Coverage goal ordering:* Search for each coverage goal is independent, and potentially useful information is not shared between individual searches. 
The whole test suite generation technique aimed at resolving these issues by targetting multiple coverage goals at the same time. The advantage of this new approach is that both the questions of how to distrubute the search budget between individual coverage goals at the same time and in which order to target these goals disappear. Large improvements have been reported for both branch coverage and mutation testing when using this method. However, despite the evidence of higher overall coverage, it is unsure as to how this whole test suite generation technique influences individual coverage goals. Is the higher coverage due to more *easy* goals being covered? Is the coverage of difficult goals just avoided? This paper provides us with an empirical study on how the whole test suite generation compares to the traditional approach. It aims as studying whether the traditional approach is better at targeting more specific goals than the new whole test suite approach. To perform this study, both approaches are performed on 100 Java classes and the results are compared. 

## Empirical Study

## Conclusion
