# B+ Trees
$log_d(n)$ of runtime, d = depth, n = # of references per node.
## Tuple Insertion
- Find leaf node where search key needs to go.
	- If already present, insert the record into the file. Update the bucket if necessary
	- If not present, insert record into the file. Adjust the index, add a new ($K_i$, $P_i$) pair to the leaf node.
	- If no space...
### Tuple Insertion with no space
- I.e. the bottom most layer has no more space for an additional element.
	- Create a temporary node (the bottom layer) which can store everything, including the element we could not fit.
	- Then this temporary oversized node needs to be divided into two nodes, which can then be placed into our B+ tree.
	- The left node gets the extra element should the split result in an uneven distribution (the left node is the "original" node that retains its connection to the parent).
	- Then, look in the right node (new node), get the smallest key, get a pointer to it, and insert the pointer into parent so we can access it. Reconfigure the tree from leaf to root.
- This algorithm is not aggressive with merging nodes to prevent redundant keys.
- **Handling of the split in leaves vs the nodes are different**.
	- Values in the leaves stay and are not removed. However, smallest values in nodes that are NOT IN THE leaf layer are removed due to redundancy.
	- Key idea for this reason: The leaf layer is a dense index. We need to maintain an explicit reference to every entry.
## Tuple Deletion
- KEY FUNCTION: Minimum keys per node: $ceil(\frac{n-1}{2})$ where $n$ is the number of pointers in a maxed node, i.e. $\# keys + 1$. <-- this is important.
- **Not confident with this process... definitely watch videos/read textbook for review**
- Find record in database, delete it.
- Then remove it from the tree:
	- *maybe* remove the (search-key, pointer) pair from leaf node.
	- But if the resulting leaf node now has too few entries:
		- Merge if possible (can only merge with siblings).
			- May result merging all the way to the roof, may reduce height of tree by one (big performance boost).
		- Borrow a tuple from an adjacent sibling and then modify parent as needed.
	- "Merge-right" (remove-right) and "merge-left" (remove-left) doesn't impact the leaf level. Rather, it impacts the parent node. 
	- **A merge must result in a full node**, this determines if we do a "merge right" or "merge left".
	- If a parent doesn't have enough keys from a deletion, then we pull a key from the grandparent to place with the parent, reducing the height of the tree.
- Can also have rotations when merging. The parent will take the root key, then the root key will take a key from the parent's sibling.
	- What's the condition for this?
- 