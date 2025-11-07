# Multi-Level Indexes
 - B-Trees.
 - Sparse indexing that goes to another level of sparse (or dense) indexes, which then links to another level of more indexes (sparse or dense), etc.
 - Not great when we need to write to the indexes.
## B Trees (Technically we're talking about B+ Trees)
- Balanced (every path from root to leaf/top to bottom is same length)
- The leaf nodes are always sorted from left to right.
- Child nodes to the right of parent are always greater or equal to the parent.
- Child nodes to the left of parent are always less than the parent.
### Searching
- We use pointers to search.
- Logarithmic time, worst case is $log_{\frac{B}{2}}(N)$, B = pointers per block (order).
- Assuming B = 100, if a relation contains 1,000,000 entries and only takes 4 random access, there is a ceiling of $log_{50}(1000000)$.
	- Best case is $log_B$
