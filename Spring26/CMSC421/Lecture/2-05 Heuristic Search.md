- General search
- Depth first search
- Breadth first search
- Cost sensitive search

> Arc is just "directed edge". Specialized term.
### Uniform cost search
- Strategy:
	- Expand the cheapest node first.
	- Fringe is a priority queue.
		- All discovered nodes are added to the priority queue. That is, they are ordered in the queue based on the cost it takes to get to that node.
	- (Basically, at the root, pick the cheapest edge to go along. Repeat this until exhausted, then go back up to the most recent split and repeat.)
- Will find the cheapest/optimal path to the goal.
- STOP CONDITION:
	- Only when the goal node is popped from the priority queue.
		- This means the algo will continue to expand all other cheaper nodes.
		- Ex: If your optimal path is a cost of 50, UCS will process all nodes/paths that cost less than 50. Every other more expensive path will be ignored.
- Effective depth: $C^{*}/\epsilon$, for C* being the cost of the optimal solution, and $\epsilon$ being arc cost (total directed edge cost).
- Time complexity: $O(b^{\frac{C^{*}}{\epsilon}})$ 
- Fringe storage: $O(b^{\frac{C^{*}}{\epsilon}})$ 
- Complete if best solution has finite cost and minimum arc cost (directed edge) is positive.
- Optimal.

## Uninformed Search vs Informed Search
- UCS, BFS, DFS vs Heuristics, Greedy, A*.
### Heuristics
- A function that estimates how close a state is to a goal.
	- Possible candidates are manhattan distance, euclidean distance path planning.
### Greedy Search
- Strategy: Expand a node (on the fringe) that you think is closest to a goal state.
### A* Search
- Combines UCS and Greedy Search principles.
- Has a fringe that sorts nodes based on their $f(n)$ value.
	- $f(n) = g(n) + h(n)$ 
		- $g(n)$ is the cost it took to get from the start to the current node n.
		- $h(n)$ is the estimated cost to get from node n to the Goal.
- The combination of $g(n)$ and $h(n)$ helps A* keep track of the cheapest path to get to the current node, and the cheapest path to get to the end. At the minimum, it avoids the highest cost paths of either.
- To guarantee A* achieving the optimal solution, the heuristic $h(n)$ has to be admissible.
	- This means, the estimate $h(n)$ must NEVER overestimate the true optimal cost to the goal.
		- ex: if the true distance is 10, $h(n)$ must never estimate anything over 10, else A* becomes nonoptimal.
		- However if $h(n)$ severely underestimates the true cost/distance, then A* will become slow as it will attempt to exhaust every possible path reflecting the low underestimated cost.
	- Developing a proper estimate is the main challenge with A*.