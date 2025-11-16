# Query Processing
### High Level Overview:
User: inputs a query, e.g. `select * from R, S where ...`
$\rightarrow$ Query Parser: parses the query, resolves references, syntax errors, etc.
$\rightarrow$ Query Optimizer: finding the *best* way to evaluate the query.
$\rightarrow$ Query Processor: runs the finalized query
$\leftrightarrow$ `R, B+Tree on R.a Scan S (No index on S.a)...`

## Query Planning
- Primarily performed by optimizer

### Cost
- Complicated to compute
- A focus on disk:
	- Number of I/O (sequential reads)
	- Number of seeks
	- etc...
	- $t_T$: time to transfer one block from HDD to memory
	- $t_S$: time for one seek
	- cost for $b$ block transfers plus $S$ seeks:
		- $b*t_T + S*t_S$.
## Select Operation
	SELECT * FROM person WHERE SSN = "123"
- Option 1: sequential scan:
	- Reads relation from start to end and searches for our target (WHERE).
	- Cost: 
		- Let $b_r =$ number of relation blocks, note "page" and "block" are interchangeable terms.
		- With 1 seek and $b_r$ block transfers:
			- $t_S + b_r * t_T$ seconds
- Option 2: binary search:
	- The relation needs to be sorted in some comparable order first.
	- Cost of finding the first tuple that matches:
		- $\lceil log_2(b_r)\rceil * (t_T * t_S)$
		- All I/O are random, so needs a seek for all. Slow because of this.
- Option 3: Using indexes (what we've been learning B+ trees for):
	- An appropriate index must exist for this to work.
	- Steps
		- Retrieve all tuples that match by the following pointers:
			- If primary index, the relation is sorted by the search key and thus the key points directly to the disc. Then read blocks sequentially.
			- If secondary index, must follow all pointers using the index.
### Select with ranges
- Option 1: sequential scan
- Option 2: Using indexes.
### Select with complex conditions
- Option 1: Sequential scan
- Option 2 (conjunctive only): Using appropriate indexes on one of the conditions.
	- I.e. if conditions are A and B, then find all indexes that match for A, then filter with B. Or vice versa.
- Option 3 (conjunctive only): use multi-key index. But not commonly available.
- Option 3: Conjunction or disjunction of record identifiers
	- Use indexes to find all RIDs that match each of the conditions
	- Do an intersection (for conjunction) or a union (for disjunction)
	- Sort records and fetch in one shot.
	- Called Index-ANDing or index-ORing.
## Sorting
- Duplicate elimination, group-by, sort-merge join.
- Three choices:
	- Can read lowest level of B+ tree if the relation is already sorted. However if it's not, there will be too many random accesses.
	- If relation is small, we can load into memory and use qsort() in C.
	- If too large for memory, we do an external sort-merge.
# External sort-merge
- Divide and conquer
- Let $M$ denote the memory size (in blocks).
## Phase 1
- Read first $M$ blocks of relation, sort, and write to disk.
- Read the next $M$ blocks, sort, and write to disk.
- Do this $N$ times.
- Now we have $N$ sorted blocks of $M$ size each.
### Example
- For $B$ relation blocks and $M$ memory blocks, there will be $2\frac{B}{M}$seeks and $2B$ IOs.
	- A seek for placing block to memory, and a seek for removing the block from memory. An IO for reading it in and writing it out.
## Phase 2
- Merge the $N$ runs ($N$-way merge).
- Can do it in one shot if $N<M$.
- If $N>M$, i.e. cannot one shot,
	- We combine blocks and sort them chunks at a time, not dissimilar to phase 1.
	- Then, we have a new $N$, if $N<M$ then we can one shot merge.
	- For example, if $M = 1000$, we can compare $999$ runs.
		- ($4$KB blocks): can sort: $999$ runs, each of $1000$ blocks, each of $4$k bytes $\approx$ $4$GB of data.

