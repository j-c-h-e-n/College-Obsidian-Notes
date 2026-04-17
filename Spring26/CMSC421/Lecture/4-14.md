Markov Chain Monte Carlo


Variable Elimination Ordering

The Galton Board
- Demonstrates the Central Limit Theorem: with enough samples, the binomial distribution approximations a normal distribution.

Prior Sampling
- $P(X_i|parent(X_i))$

Rejection Sampling
- Filtering out samplings we don't need.
- If we only want P(C | +s), we tally all C except for those that are not S = +s.
- This technique is consistent, reflective of true probability.

Likelihood Weighting (study this and figure out how to solve the probabilities).
- $F \rightarrow A$, we have $P(\pm f)$ and $P(\pm a|\pm f)$. But we want $P(\pm f|+a)$.

Performance Analysis:
- We can analyze the error given from our estimation/sampling by comparing Rejection Sampling and Likelihood Weighting Sampling.

Gibbs Sampling: A special case of M-H
- Sample one variable (with a fixed value) at a time, conditioned on all the rest (i.e. randomly initializing all other variables). We fix the values of the variables we've already sampled for future samples.
![[Pasted image 20260414144938.png|300]]

The Metropolis-Hastings Algorithm
- "most popular example of a Markov chain Monte Carlo (MCMC) method in statistics."

Dynamic Bayesian Networks