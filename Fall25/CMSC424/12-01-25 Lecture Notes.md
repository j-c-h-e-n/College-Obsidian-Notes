# Query Optimization
- Databases NEED a good query optimizer.
- The most expensive systems have the best optimizers.
- Cost minimization.
- Step 1: Figuring out solution space.
- Step 2: Finding algorithms/heuristics to search through the solution space.
## Cost Estimation
- Needs information such as:
	- Primary key?
	- Sorted or not, which attribute is it sorted on?
	- How many tuples in relation?
	- How many tuples fulfill the provided condition?
	- Join sizes.
	- etc.
- DBs store some static data (metadata) about relations to make these judgements.
## Using Histograms
- Selection size estimation: the number of unique tuples in the range divided by the range. This assumes a uniform distribution within that specific range.
## Basic Statistics
- $n_r = |r|$ the numbers of tuples in a relation r.
- $b_r$ the number of blocks containing tuples of r.
- $l_r$ the size of a tuple of r
- $f_r$ the blocking factor of r, the number of tuples of r that fit into one blokc.
- $V(A, r)$ 
- and more...

