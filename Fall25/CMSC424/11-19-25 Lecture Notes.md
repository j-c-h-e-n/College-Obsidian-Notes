# Query Processing Cont.

## Block Nested Loops Join
### Variables
- $M$ = the number of blocks for the memory buffer. BUT there is an implicit block to store results. Thus, the visualizations only show $M-1$ blocks.
- $b_r$ = tuples per block in the R relation.
- $b_s$ = tuples per block in the S relation.
- $n_r$ = total number of tuples in the R relation.
- $n_s$ = total number of tuples in the S relation.
### Calculations for Block Nested Loop Join
- If $M = 3$ (minimum memory required to perform join):
	- blocks transferred = $b_s * b_r + b_r$
	- seeks = $2*b_r$
- If $R$ completely fits in memory:
	- blocks transferred = $b_s + b_r$
	- seeks = $2$
- If we can only take in $S$ partially:
	- blocks transferred = $b_s * \lceil\frac{b_r}{M-2}\rceil + b_r$
	- seeks = $2 * \lceil\frac{b_r}{M-2}\rceil$
### Assigning R and S
- Nested loop join will have the smaller relation to be the "inner" (S).
- Block nested loops will want the smaller relation to be the "outer" (R).

		block nested loop join is always better than nested loop join.
## Index Nested Loops Join
- Mainly the same as block nested.
- Instead of having to check for tuple s in S, we assume S is indexes. Then we just extract by index comparisons.
- Cost: $b_r(t_T + t_S) + n_r * c$
- Con: since we're matching indexes, it's really seek dependent in a traditional large scale join.
	- Really good if the outer table is super small, i.e. for queries.
## Merge Join
- NOTE: Both relations R and S are sorted.
	- We do comparisons from a tuple at a time. 
	- If tuple at index i in R is less than tuple at index j in S we move i forward, and vice versa.
- Seek heavy.
- block transferred = $b_r + b_s$.
- Performance typically comparable to hash join even if we need to sort first.
- The result will also have to be sorted based on the same sort key earlier.

## Hash Join
- Phase 1:
	- We perform a simple hash on the outer (R) relation and populate the resulting relation with the indexes we get from the simple hash.
		- (element of R) mod (size of output relation)
		- Each index of the output relation becomes buckets to hold the values of R.
		- Save this result to the side.
	- Now we do the exact same thing for the inner (S) relation.
		- (element of S) mod (size of output relation).
		- We have created new buckets for S.
- Phase 2:
	- Now to find matches between S and R all we do is use the buckets (these ideally fit in memory).
	- We bring in the buckets for S into memory, one index of values at a time. Then we perform a join match with the results of R (buckets of R) against S, both from the same index from their respective buckets.
	- This is usually linear time if the values of the bucket indexes all fit in memory (hashing provides great compression).
- First phase requires $\sqrt{(b_r)}$ blocks of memory
- Every block of bigger table S gets read, written, then read: $3*b_s$
- Every block of smaller table R gets read, written, then read: $3*b_r$
- Seeks are hard to calculate. The worst case is $2*(b_s + b_r)$.	
# Summary:
- Nested Loops Join
	- Simple, easy, but slow.
- Block Nested Loops Join
	- Always be applied and better than nested loops join.
- Index Nested Loops Join
	- Only applies if an appropriate index exists.
- (Sort) Merge Join
	- Common especially if indexes are sorted.
- Hash Join
	- Good if one relation is significantly larger than the other (needing compression).

