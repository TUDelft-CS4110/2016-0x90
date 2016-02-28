# Effectiveness of Whole Test Suite Generation
**Article:** *G. Fraser and A. Arcuri.* On the Effectiveness of Whole Test Suite Generation. Search-Based Software Engineering, vol. 8636, pp. 1-15, 2014.

## Research Goal
Before the whole test suite generation technique, the traditional method was to carry out a search on each individual coverage goal (e.g. a branch, a statement, etc). There are a couple of issues with such an approach:
- *Search budget distribution:* Wasting time on infeasible coverage. Determining whether a goal is feasible or not is an undecidable problem.
- *Coverage goal ordering:* Search for each coverage goal is independent, and potentially useful information is not shared between individual searches. 

The whole test suite generation technique aimed at resolving these issues by targetting multiple coverage goals at the same time. The advantage of this new approach is that both the questions of how to distrubute the search budget between individual coverage goals at the same time and in which order to target these goals disappear. Large improvements have been reported for both branch coverage and mutation testing when using this method. However, despite the evidence of higher overall coverage, it is unsure as to how this whole test suite generation technique influences individual coverage goals. Is the higher coverage due to more *easy* goals being covered? Is the coverage of difficult goals just avoided? This paper provides us with an empirical study on how the whole test suite generation compares to the traditional approach. It aims as studying whether the traditional approach is better at targeting more specific goals than the new whole test suite approach. To perform this study, both approaches are performed on 100 Java classes and the results are compared. 

## Empirical Study
This empirical study aims at answering the following three questions:
- **Q1:** Are there coverage goals for which the traditional *OneBranch* approach performs better?
- **Q2:** How many coverage goals found by the *Whole* approach get missed by *OneBranch*?
- **Q3:** Which factors influence the relative performance between *Whole* and *OneBranch*?

100 Java classes were randomly chosen out of 100 randomly selected projects. 

### Q1
After performing the empirical study, it was found that *OneBranch* performed better for over 50 coverage goals. Three of these coverage goals were never covered by *Whole*. However, there are more than half cases for which *Whole* performed much better. In other words, even with three difficult goals that *OneBranch* can cover, many more (82 times) difficult branches were covered by *Whole* and ignored by *OneBranch*.

### Q2
As mentioned before, *Whole* is able to handle 82 times more difficult branches than *OneBranch*. It is then assessed that the *Whole* approach leads to higher coverage, even for difficult branches. 

### Q3
The researchers quantified the odds that *Whole* performs better to cover a certain goal compared to *OneBranch*. After some research and high usage of statistical techniques, there was no correlation that could be found between quantification of performance and other factors. This means that there is no evidence of a factor that strongly influences the relative performance between *Whole* and *OneBranch*. 

## Conclusion
Existing research has shown that the whole test suite approach lead to better coverage results than the traditional one target at a time approach. There was reasonable doubt on whether this would be the case when targetting difficult goals. This paper performed an in-depth analysis to study if this doubt could be confirmed in practice. It was found that indeed, the *Whole* approach had more difficulties when targetting difficult goals. However, these cases are very few compared to the cases for which *Whole* performed significantly better. This paper therefore provides more support to the validity and usefulness of the whole test suite generation method in the context of software testing.
