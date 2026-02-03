![[Pasted image 20260203140858.png]]
- Possible states of the 8-world game. Actions would be anything that permutes the figure from one state to another.
![[Pasted image 20260203140949.png]]
- This example here shows a real-world possibility of a state-action relationship.

- There are discrete and continuous space planning. We will focus on discrete space planning in 421.
![[Pasted image 20260203141244.png]]
- We have two worlds when we look at configuration spaces.
	- The actual and parameter world.
	- The actual world is the real world representation of our objects and environment.
	- The parameter world is the space of allowable parameterizations of the robot in the real world, shown by the graph representation of all the possible combinations of the relevant variables in the actual world.

## SAT: Boolean satisfiability
![[Pasted image 20260203141746.png]]
- We can go through every possible combination of x, y, and z to solve this in a brute force fashion.
- This results in a search space of $2^n$ (0/False, 1/True), n for the number of variables to consider.

## Atomic States
![[Pasted image 20260203141935.png|400]]
- A single, indivisible snapshot of the world, represented as a unique identifier or hashable object without revealing internal details.
	- Basically abstracted for search algorithms so they don't get bogged down by the specific state components.
## Factored States
![[Pasted image 20260203142151.png|400]]
- A vector of values representing a set of variables, each with a value.
- For example, SAT Assignments (X=True, Y=False, Z=True).
## Structured States
![[Pasted image 20260203142241.png|400]]
- Represents the world as a set of objects connected via various relations.
![[Pasted image 20260203142313.png|200]]
- Basically models the entire world as objects and relationships.
- Helpful for first-order logic and object-oriented planning.

# Agents that Plan Ahead
- Planning: finding a sequence of actions that leads from an initial state to a goal state.
	- Complete planning: guaranteed to find a solution if one exists.
		- Solution: A sequence of actions (plan) which transforms the start state to a goal state.
	- Optimal planning: guaranteed to fine the best solution if one exists.
	- Important feature of planning agents: they consider future potential states of the world.
- Search (in planning): Systematically exploring states of actions to find a sequence of steps (plan) that leads to a goal.
	- Search problems consist of a state space and a successor function with actions and costs, a start state, and a goal test.
## State Space Graphs
- A mathematical representation of a search problem, with every possible outcome/sequence represented.
	- Each state occurs only once.
	- These graphs can become far too big.
  ![[Pasted image 20260203143304.png|200]]![[Pasted image 20260203143318.png|250]]
## Search Trees
![[Pasted image 20260203143435.png]]
- The root is an initial state. The children correspond to possible subsequent states through a successor function.
- Like the state space graph, these are often too big to build.
- Unlike a state space graph, repeated states can happen.
	- These repeated states can result in an infinitely tall tree.
![[Pasted image 20260203143501.png]]
## Searching with Trees
- Search process:
	- Expand out potential plans (tree nodes).
	- Maintain a fringe of partial plans under consideration.
		- fringe (the data structure containing all currently generated but not yet expanded nodes in a search tree/graph. The boundary between explored and unexplored areas).
	- Try to expand as few tree nodes as possible.
![[Pasted image 20260203144816.png|300]]
- There will be a number of $b^m$ nodes in the tree, resulting in an average time complexity of $O(b^m)$. 
	- b is branching factor, m is height.
### DFS
- Expand a deepest node first.
	- Usually the left of the tree is expanded first.
	- Takes $O(b^m)$ to search the tree.
	- Takes $O(bm)$ space.
- Implementation: Fringe is a LIFO stack (last in, first out)
- Bad for trees with infinite height since it will infinitely search down the infinite stack.
- Provides a complete solution, given there are no infinite branches.
- Does not find the optimal solution, finds the "leftmost" solution.
### BFS
- Expands the shallowest nodes first.
- The fringe is a FIFO queue (first in, first out)
	- Search takes $O(b^s)$ where $s$ is the depth of the shallowest solution.
	- The fringe takes $O(b^s)$.
- If $s$ is finite then there exists a solution, so it is complete.
- It is optimal if all action costs are 1.
### Iterative Deepening Search
- Use DFS's space advantage ($bm$ vs $b^s$)