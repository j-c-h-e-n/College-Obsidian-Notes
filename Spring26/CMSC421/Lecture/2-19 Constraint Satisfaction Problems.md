## Intro to CSP
- CSPs are a specialized class of identification problems
	- SAT, Knapsack, Sudoku, Graph Coloring, etc.
	- The final assignment (solution) matters, not the path it took to get there.
- A standard search problems treats each state as atomic.
	- CSPs use a factored state, allowing them to see the internals of each state.
- Solutions for CSPs just need to satisfy all constraints.

- A Binary CSP shows each constraint relates to at most two variables.
	- Nodes are variables, arcs are constraints.
	- General-purpose CSP algos use the graph structure to speed up search.
	- If a node is not part of the main body of the graph, they are considered independent subproblems.

- Example:
	- Sudoku:
		- Variables: Each unpopulated square.
		- Domains: {1, 2, ..., 9} (The possible choices for each tile/square)
		- Constraints: 9-way all-diff for each column, row, and region. (or some pairwise inequality constraints).
	- 3CNF-SAT
		- Variables: X, Y, and Z.
		- Domains: True/False, 1/0
		- Constraints: The 3-variable clauses.

- CSPs with:
	- Discrete Variables:
		- Finite Domains:
			- n variables, domain size of d (d options).
			- Boolean CSPs
		- Infinite Domains:
			- Integers, strings, etc.
			- Job scheduling
			- Linear constraints are solvable, nonlinear are undecidable.
	- Continuous Variables:
		- Start/end times for Hubble Space Telescope observations.
		- Linear constraints solvable in polynomial time by linear programming.
- Different types of constraints:
	- Unary -> Single variable: X != Green.
	- Binary -> Pairs of variables: X != Y
	- >= 3 -> Cryptarithmetic column constraints.
	- Soft constraints -> "preferences" or biases, such as we prefer a variable to be Red instead of Green.

## Solving CSPs
- States of a CSP:
	- Initial state: empty assignment {}
	- Successor function: assigns values to unassigned variables.
	- Goal test: completion and constraint satisfaction check.
### Backtracking Search
- The basic uninformed search for CSPs.
	- Essentially DFS, but abandons a path the moment it determines it will not lead to a solution.
- However it is inefficient for large scale problems and can pointlessly explore many paths.
- Improvements can be made by combining:
	- Forward checking: keeping track of remaining legal values for unassigned variables.
	- Constraint propagation: (Arc consistency) where we maintain consistency between pairs X and Y. 
		- We can increase the degrees of consistency, but doing so increases compute requirements.
		- ![[Pasted image 20260219145447.png|400]]
		- **Strong K-consistency** means all values < K are consistent. Regular K-consistency does not mean that all values < K are consistent as well.
	- Heuristics: (Ordering). 
		- "Minimum Remaining Values/Most Constraining Value" heuristic is a fail-fast approach where we choose the variable with the fewest legal possible values so we can prune branches quick.
		- "Least Constraining Value" heuristic chooses the value for a variable that rules out the fewest values in subsequent remaining variables. (Maximizing our choices in later variables).

Forward passes are able to find a valid solution if a tail-to-head backtrack has no 