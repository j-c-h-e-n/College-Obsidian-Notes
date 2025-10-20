In order of introduction through classes:
1. Expected Value
2. Linearity of Expectations
3. Hashing
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
10. Chernoff-Hoeffding Upper Bound
	1.   $Pr[X \geq \mu(1+\alpha)] \leq e^{\mu*\frac{\alpha^2}{3}}$
11. Bloom Filters
12. Min-Hash
13. Locality Sensitive/Similarity Preserving Hashing
	1. HyperLogLog
14. Duplicate Detection
15. 