- A* is extremely memory hungry, but it is better than BFS.
- 8-puzzle effect branching factor: $ebf^d = ($ total # nodes$)/2$ OR $ebf = log_d(($ total # nodes$)/2)$.
## Time: A Performance Measurement Challenge
- CPU Time
	- How many seconds are used when executing X?
	- Small dependence on other system activities.
- Actual ("Wall Clock") Time
	- How many seconds elapse between the start and completion of X?
- Three measures for tracking time:
	- Wall clock (overestimate of User times + CPU times)
	- User time
	- CPU time
- Usually take minimum time since that is the "purest", most optimal and unaffected by extraneous factors.
## Empirical CS:
- Performance measurements:
	- Memory, # steps, "Big O", time.
	- Energy consumed.
	- Data requirements
- Effectiveness:
	- Optimality, cost, % of optimal.
	- User experience.
	- Goals achieved.
## "Experimental Hygiene" for AI Evaluation
- Design reproducible experiments.
	- How many samples?
		- Rule of thumb: Use the Central Limit Theorem.
## IDA* vs A*
- IDA* is complete, optimal, and usually less costly than A*.
- IDA*, iterative deepening A*, has most of its work happen at the end, in nodes closer to the goal.

## Approximation Algorithms
- Sacrificing effectiveness for performance.
- Needs an exact answer.
## Anytime vs Approximation
- Approximation:
	- Broad class of problems.
	- Runtime is important, real-time computation is not.
- Anytime:
	- Kind of a subset of approximation algorithms.
	- Real-time computation is important.
	- Stable and INTERRUPTABLE.
### Anytime A*
- Puts a bias towards the $h(n)$ heuristic function, applying searches more commonly towards paths closer to the goal.
### Contract Algorithms
- Time is allocated to these algorithms a priori, allowing them to execute with respect to  a time constraint.
- 