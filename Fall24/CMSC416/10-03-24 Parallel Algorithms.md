# Parallel Algorithms
## Matrix Multiplication
```cpp
for (i = 0; i < M; i++)
	for (j = 0; i < N; j++)
		for (k = 0; k < L; k++)
			C[i][j] += A[i][k] * B[k][j]
```
This will run into performance issues as the matrices get larger and larger. $O(n^3)$

### Blocking to Improve Cache Performance
- Create smaller blocks within a giant matrix that fit in cache: leads to cache reuse.
- $C_{12} = A_{10} * B_{02} + A_{11} * B_{12} + A_{12} * B_{22} + A_{13} * B_{32}$
![[Pasted image 20241003202312.png]]
```cpp
for (ii = 0; ii < n; ii+=B)
	for (jj = 0; jj < n; jj+=B)
		for (kk = 0; kk < n; kk+=B)
			for (i = ii; i < ii+B; i++)
				for (j = jj; j < jj+B; j++)
					for (k = jj; j < jj+B; k++)
						C[i][j] += A[i][k] * B[k][j];
```
# Parallel Matrix Multiply
- Store A and B in a distributed manner.
- Communication between processes to get the right sub-matrices to each process.
- Each process computes a portion of C.
## Cannon's 2D Matrix Multiply
- Arrange processes in a 2D virtual grid.
- Assign sub-blocks of A and B to each process.
- Each process responsible for computing a sub-block of C.
- Requires other processes in its row and column to send A and B blocks so it can compute the final values of its sub-block.
### Pros/Cons
- Memory efficient.
- Suitable for 2D architectures.
- Constant storage requirements.
- But requires a square number of processes ($\sqrt{p}$)
- Difficult to extend to heterogeneous 2D grids.
###### Ex:
$C_{12} = A_{10} * B_{02} + A_{11} * B_{12} + A_{12} * B_{22} + A_{13} * B_{32}$
![[Pasted image 20241003205150.png]]
Alignment:
In A: Displace every block in row i by i spaces.
- So row every element in row 1 is moved to the left by 1. Elements to the left most are wrapped around to the left. Row 2 moved by 2, row 3 moved by 3, etc.
In B: Displace every block in column j by j spaces.
- Shift upward, wrapping around to the bottom.
![[Pasted image 20241003205347.png]]
Now perform local block calculations, adding the result to the C submatrix.
Then:
In A: shift by left 1 in every row.
In B: shift by up 1 in every column.
![[Pasted image 20241003210244.png]]
Repeat the multiplication, then shift by 1 again. These steps are repeated $\sqrt{p}$ times.

## Agarwal's 3D Matrix Multiply
- Arrange processes in a 3D virtual grid.
- Assign sub-blocks of A and B to each process.
	- There are multiple copies of A and B (one in each plane).
- Each process computes a *partial* sub-block of C (what is C?).
- Data movement is done only once before computation and once after computation.
###### Ex
Copy A to all i-k planes and B to j-k planes.
![[Pasted image 20241014132557.png]]
![[Pasted image 20241014132651.png]]
Perform a single matrix multiply to calculate partial C.
All reduce along i-j planes to calculate final result.
![[Pasted image 20241014132727.png]]
# Communication Algorithms
Reduction and All-To-All
## Reduction
- Scalar reduction: every process contributes one number.
	- Perform some commutative associate operation.
- Vector reduction: every process contributes an array of numbers.
### Parallelizing
- Several different types of parallelization algorithms for reduction:
	- Naive algorithm: every process sends to the root.
	- Spanning tree: organize processes in a n-ary tree.
		- Starts at leaves and sends to parents.
		- Intermediary nodes wait to receive data from their children.
		- $log_kp$ phases.
## All-To-All
Another collective call like reduction.
- Each process sends a distinct message to every other process.
- Naive algorithm: every process sends the data pair-wise to all other processes.
![[Pasted image 20241014134224.png]]
(Looks like making the transpose of a matrix)
# Virtual Topology
## 2D Mesh
Alternative algorithm: sends messages along the rows and columns of a 2D mesh.
- Phase 1: every process sends to its row neighbors.
- Barrier: waits for phase 1 to complete.
- Phase 2: every process sends to its column neighbors.
![[Pasted image 20241014134839.png]]
## Hypercube
An n-dimensional analog of a square (n=2) and cube (n=3).
- Special case of a n-ary d-dimensional mesh:
![[Pasted image 20241014134936.png]]
