Ex: "If I leave for the airport 60 minutes before my flight, will I arrive on time?"
Potential sources of uncertainty:
- Partial observability: road conditions, other drivers' plans, etc.
- Noisy sensors: radio traffic reports, google maps estimates.
- Immense complexity: traffic modelling, security lines, parking, gate changes, etc.
- Unknown dynamics: flat tire, sudden detours, unexpected delays.

Maximizing expected utility:
$$a^* = argmax_a (\sum_s P(s|a) * U(s)) $$
- s (state/outcome)


## Basic Laws of Probability
- Begin with a set $\Omega$ - the sample space (possible worlds).
	- Ex: {1, 2, 3, 4, 5, 6} for possible dice rolls.
- $\omega \in \Omega$ is a sample point (atomic event/possible world) which represents a single outcome of a random experiment.
- A probability space or probability model assigns a number $P(\omega)$ to each world $\omega$.
	- Ex: $P(1) = P(2) = P(3) = P(4) = P(5) = P(6) = \frac{1}{6}$
- An event is a set of sample points - a subset of $\Omega$.
	- Ex: "Roll < 4" is the set {1, 2, 3}
- The probability of an event is the sum of probabilities over its worlds:
	- $P(A) = \sum_{\omega \in A} P(\omega)$

## Prior Probability
- The proportion of times this event occurs out of all possible occasions before taking into account any specific evidence.
	- ex: P(Cavity = true) = 0.1
	- P(Weather = sunny) = 0.72
	- P(T = hot) = 0.5

## Conditional Probability
$$P(A|B) = \frac{P(A \land B)}{P(B)}$$
## Chain Rule
$$P(x_1, x_2, ..., x_n) = \prod_{i=1}^{n} P(x_i | x_1, ..., x_{i-1})$$