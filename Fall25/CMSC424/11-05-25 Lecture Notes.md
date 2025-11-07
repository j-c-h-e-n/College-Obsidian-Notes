# B+ Trees
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
- 