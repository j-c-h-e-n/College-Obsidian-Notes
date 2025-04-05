# Entropy
- A measure of optimization where we want to minimize our entropy, which means the most amount of "splitting" per branch in a decision tree.
- S is a sample of training examples.
- $p_1$ is the proportion of positive examples in S.
- $p_2$ is the proportion of negative examples in S.
- Entropy measures the impurity of $S$.
![[Pasted image 20250203141233.png]]
## Conditional Entropy
- Conditional entropy $H(Y|X)$ of a random variable $Y$ conditioned on a random variable $X$.
![[Pasted image 20250203141547.png]]
( SELF STUDY ): writing entropy calculations by hand, having to find before-split-entropy and after-split-entropy.
- Calculate information gain by $IG(X) = H(Y) - H(Y|X)$. In words, calculating the entropy before-split by subtracting the after-split-entropy from it.
- https://cs.stackexchange.com/questions/116301/how-to-calculate-conditional-entropy

# Bootstrapping
- "To make an interference about an estimate of statistic".
- Given:
	- Observations $x_1, ..., x_n$ and estimates $y=\frac{1}{n}\sum_{i=1}^{n}x_i$
- We build our own dataset based off of our own data.
	- A randomization procedure that measures the variance of estimates of y.
- Procedure:
	- Generate B random datasets by sampling with replacement from dataset $x_1,...,x_n$. Denote randomized dataset b as $x_{1b},...,x_{nb}$.
	- Construct estimates from each dataset $y_b=\frac{1}{n}\sum_{i=1}^{n}x_{nb}$.
	- Compute center (mean) and spread (variance) of estimates $y_b$.
- OVERFITTING = HIGH VARIANCE but low bias.
- Less variance means more bias.
- Bootstrapping reduces this variance.
- Bootstrapping vs CLT: https://stats.stackexchange.com/questions/363456/when-is-bootstrapping-helpful-and-used

# Bagging
- An extension of bootstrapping.
- Combines bootstrapping and aggregation.