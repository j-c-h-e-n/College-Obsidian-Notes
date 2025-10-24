We've traditionally defined $X_i$ to be a Bernoulli r.v. that is, 1 if success, 0 otherwise.

In order of introduction through classes:
1. Expected Value
	1. $E[X] = \sum_aa*Pr[X=a]$ 
2. Linearity of Expectations
	1. $E[\sum^n_{i=0}X_i] =\sum^n_{i=0}E[X_i]$
	2. Solve the inside $E[X_i]$ and it's just $n*E[X_i]$.
3. Hashing
	1. An idealized hashing function $h: u \rightarrow T$ where $u$ is the initial universe, and elements are transformed to fit universe $T$.
	2. Often need to find $Pr[$chance of collision$]$ for elements being placed in the same bucket.
4. Markov's Inequality  
	1. $Pr[X \geq v] \leq \frac{\mu}{v}$ where $\mu = E[X]$
5. Two level hashing
6. Union Bound
	1. $Pr[X_1 | X_2 | ... | X_n] \leq \sum^{n}_{i=1}Pr[X_i]$
7. Chebyshev's Inequality
	1. $Pr[|X-\mu| \geq v] = Pr[(X-\mu)^2 \geq v^2] \leq \frac{E[(X-\mu)^2]}{v^2} = \frac{Var[X]^2}{v^2}$
	2. Invokes Markov in the middle steps.
8. Random Sampling and relation to independence
9. Bernstein-Chernoff-Hoeffding Bounds
	1. $Pr[X \geq n*q] = Pr[X \leq n*q] \leq e^{-n*KL(q, p)}$ where $KL = q*ln(\frac{q}{p}) + (1-q)*ln(\frac{1-q}{1-p})$
10. Chernoff-Hoeffding Upper Bound (Upper Tail)
	1.   $Pr[X \geq \mu(1+\alpha)] \leq e^{\mu*\frac{\alpha^2}{3}}$
11. Chernoff-Hoeffding Lower Bound (Lower Tail)
	1. $Pr[X\leq\mu(1-\alpha)] \leq e^{-mu\frac{\alpha^2}{2}}$
12. Bloom Filters
13. Min-Hash
14. Locality Sensitive/Similarity Preserving Hashing
	1. HyperLogLog
15. Duplicate Detection
16. 