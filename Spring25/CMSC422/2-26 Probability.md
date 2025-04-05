# Probabilities and Classification
- Assume we know feature vector (x_1, ..., x_n). We want to infer the label y.
- We can think of this as solving: $argmax_y(P(y=Y|x=X))$
- This may reflect something that is really probabilistic (predicting who will win a poker game).
- Or something that is uncertain given our feature vector.

## Joint Distribution
- $P(x=X, x'=x')$ means the probability that both $x=X$ and $x'=X'$.
- $P(x=X,x'=X') = P(x=X)*P(x'=X'|x=X) = p(x'=X')*P(x=X|x'=X')$
	- 'and' probability where $P(A \ and\ B) = P(A) * P(B|A)$
## Independence
- If $P(x,x') = P(x)P(x')$ then $x$ and $x'$ are independent.
	- This makes the joint distribution $P(x=X, x'=X') = P(x=X)*P(x'=X')$
- Thus,  $P(x=X|x'=X') = P(x=X)$ _iif_ $x$ and $x'$ are independent.
- Independence simplifies probabilistic inference.
	- A two dimensional problem $P(x,x')$ is now one-dimensional if they are independent, making it $P(x)*P(x')$.
## Marginalization
- Suppose $x \in \{X_1, ..., X_n\}$
- Note that $P(x=X_1) + ... + P(x=X_n) = 1$.
- $P(y) = \sum_{i=1}^n P(y, x=X_i)$

