# Statistical Tests
- A statistical hypothesis test is a method of statistical inference used to decide whether the data at hand sufficiently supports a particular hypothesis.
	- There are 100 different tests that have been developed.
- One sample t-tests and z-tests are one of many.
## Vocab
- Null hypothesis:
	- The null hypothesis (denoted by $H_0$) is a statement that the value of a population parameter (such as proportion, mean, or standard deviation) is equal to some claimed value.
	- "What you're seeing doesn't deviate from the population."
- Alternative hypothesis: 
	- "What you're seeing is DIFFERENT than the population."
- p-value: the probability that the null hypothesis is correct.
## Standard normal distribution table
![[Pasted image 20240917094452.png]]
- One standard deviation is comprised of 68.2% of the total population.
- GIVEN:
	- A sample, S, from our population.
	- A possible mean, M.
- Higher variances will result in a larger sample space to get a proper representation of the population.
## One Sample T-Test
- Needs a normal distribution to work best.
- Takes in a sample, tells you how likely it is the sample mean is the true mean.
```python
import numpy as np
from scipy import stats

stats.ttest_1samp(your_data, popmean=0.5)

>>>TtestResult(statistic=..., pvalue=..., df=...)
```
- Example:
	- We abduct 100 people and record their heights, which have a mean of 5.5. We can compute the probability that these 100 people came from a population with a true mean of 5.5, So, for example, we might get a 90% chance a true mean of 5.5 would produce our sample.
## Two Sample T-Test
- If we notice two distinct categories in our data, could we take the average of the two samples?
- Ex:
	- $H_0$: Men and women are the same height.
	- $H_1$: Men and women are different heights.
	- p-value: The probability that we see these observations under the null hypothesis.
	- We want a low p-value in this case since we want to observe that men and women are different heights.
## Paired T-Test
- Tests before and after cases.
- Ex:
	- The aliens monitor a bunch of humans, test them for intelligence, and then run them through a machine to make them smarter. Afterwards, they want to know if their machine worked.
	- $H_0$: The machine did nothing (Intelligence did not change and therefore the machine did not work).
	- $H_1$: The machine caused a different distribution (Intelligence did change and therefore the machine did work).
## Tailed T-Test
- Allows us to specify a direction:
	- We can say: What is the probability that we would see these results if the brain machine made the humans smarter
![[Pasted image 20240917101325.png]]
### One-Tailed
- Looks for one difference (direction).
### Two-Tailed
- Two tailed tests look for *any* difference in population.
## Chi-Squared Tests
- A test for checking if two sets of categorical variables come from the same distribution.
- Ex: Sampling birds and testing of they come from different locations.
	- $H_0$: The birds come from the same distribution (location).
	- $H_1$: The birds come from different locations.
## One Way Anova Test
- Used when we have a bunch of different groups and when we want to make comparisons between each group.
- $H_0$: There is no difference between any of the groups.
- $H_1$: There is a difference between at least one of the groups.
## Sampling Errors
- Screwed! Results of the t-test will be invalid.
- Ex:
	- Abducting people from only one country/continent is not representative of the total world population.
## Type 1 and Type 2 Errors
- Hypothesis testing involves the risk of making errors.
	- Type 1 (False Positive): Occurs when you **incorrectly reject a true null hypothesis**.
	- Type 2 (False Negative): Occurs when you **fail to reject a false null hypothesis**.
# Choosing a Statistical Test
## Considerations
- What are we curious about?
	- Mean? Std Dev? Frequency?
- Is your data categorical or continuous?
- Do you have one or two samples?
- Is your data normally distributed?
	- If it is, you would use a parametric test.