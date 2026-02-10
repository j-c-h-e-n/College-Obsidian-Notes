- Consistency of Heuristics
	- Basically the Triangle Inequality
	- Estimated heuristics costs $\leq$ actual costs.
	- How A* is optimized - picking the correct estimated cost.

- Local search as in not maintaining the entire graph, only focusing on what's immediately around in the "radius".
	- Also called "Iterative Improvement".
	- These are the "second best way" to solving a problem.
	- But are relatively easy to implement.
	- These algorithms trade optimality for speed.
## Hill Climbing
- Ascends.
- Finds a local max but can get stuck at an non-optimal max.
- We can impose random restarts to try to solve the non-optimal issues.
	- Requires a hyperparameter to randomly sample hill climbs from the data.
- Requires no memory! (Due to no backtracking).
- Move-set design is critical (available actions at every step).
## Gradient Descent
- Descends.
- Finds a local min and can get stuck.
- Same concepts as hill climbing, gradient descent is just the other direction.
## Simulated Annealing
- Hill climbing and gradient descent are deterministic.
- Simulated annealing provides a non-deterministic approach.
	- Annealing refers to heating and cooling process of sword forging.
- Is basically hill climbing/gradient descent except the algorithm allows random (bad) moves at the beginning, then it settles down as the iterations grow.
	- "Chaos at the start, calms down towards the end".
- Advantages of getting unstuck and out of local minima/maxima.
- Issues:
	- Move-set design is critical.
	- Evaluation function design is critical.
	- Annealing schedule is often critical.
## Genetic Algorithms
- Setup:
	- Imagine a MASSIVE search space.
	- Difficult to define a heuristic.
	- Difficult to define even an evaluation function, but could maybe figure out relative solution quality (fitness).
	- It is not obvious of how to figure out what exactly a good solution is.
- The idea is to evolve a solution of high quality from a population pool of candidate solutions.
	- A (good) fitness function is what guarantees improvement over time.
- A lot of hyperparameter tuning.