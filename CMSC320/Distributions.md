Distributions are...
- Statistical function that gives the probability of a given outcome from an experiment.
- Continuous, but large enough sample size converges to the right thing.

## Uniform Distribution
- No particular insight as to how it is distributed.
- All even/the same throughout the entire range.
## Normal Distribution (Gaussian)
- Extremely common.
- Data usually tends to trend towards a normal distribution.
## Poisson Distribution
- A graph with the left compression, and a long right tail.
- They have a parameter called `lambda` which represents the average rate.
- ![[Pasted image 20240912094529.png]]
## Zero-Inflated Poisson Distribution
- Oftentimes, poisson distributions can have a spike at 0.
- Any distribution can be zero inflated.
- Will need to dealt with differently.
- Ex: Graph of those who check out of the ER (0 i.e. deceased).
- ![[Pasted image 20240912095227.png]]
## Bernoulli Trial
- Literally boring.
- ![[Pasted image 20240912095158.png]]
## Binomial Distribution
- Shows us the probability of getting a specific set out of outcomes from a set of Bernoulli Trials.
- ![[Pasted image 20240912095347.png]]

# The Central Limit Theorem
- A fundamental theorem of statistics.
- If we sample a distribution a bunch of times, the set of *sample means* is normally distributed.
## Use 
- Comparing two non-normal distributions is hard.
	- If you sample each distribution a bunch of times, we can look at the means of the sample. This allows a look into the change over time of the original non-normal distribution.
## Example
- Say I take 5 samples from a uniform distribution (dice).
- Most of the time, the mean will be 3.5.
	- What is the rarest mean?
		- We have highest chances of getting an average of 3.5, but what if a dice gives us 1 1 1 1 1, or 6 6 6 6 6?
- We can graph a distribution of the frequency of its means.
	- "The distribution of an average of a bunch of averages."

# Summary Statistics
- They communicate something about the dataset without needing to understand the whole thing.
- Very useful for quickly understanding what's going on.
- Ex: Mean, median, mode.
- However they can be misleading if you use them by themselves.
## Measures of Central Tendency
- These tell you about the center of your distribution.
- Pythagorean means:
	- Arithmetic Mean: your typical average.
	- Geometric Mean: a measure of central tendency less sensitive to outliers.
	- Harmonic Mean: primarily used for rates. 
- Median
	- The value in the dataset that has an equal number of items greater than and less than it.
- Mode
	- The most common item in your dataset.
## Measures of Variance
- Descriptors for how broad/spread out the distribution is.
## Other Descriptors
- Skew (left (negative) or right (positive) tailed)
	- Skew measure whether the distribution is shifted to the left or right.
- Modality
	- A distribution is multimodal (bimodal, trimodal, etc) depending on how the modes are distributed.