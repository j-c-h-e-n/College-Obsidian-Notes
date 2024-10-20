# Designing Parallel Programs
- *Decide* the serial algorithm first.
	- The different models of data computation.[[08-29-24 Intro]]
		- SISD, SIMD, MIMD, SIMT, SPMD
- *Data*: how to distribute data among threads/processes?
	- Data locality: assignment of data to specific processes to minimize data movement.
- *Computation*: how to divide work among threads/processes?
	- How would you structure your actual code to divvy up what code gets run where/when?
- Figure out how often communication is needed.

# Conway's Game of Life
- A Turing complete simulation.
- The gist:
	- 2D grid of cells.
	- Each cell can be either alive or dead.
	- Every cell only interacts with its eight nearest neighbors.
	- In every generation, there are some rules that decide if a cell will continue to live or die or be born.
- "Cellular automata"

# 2D Stencil Computation
- Commonly found kernel in computation codes.
- Heat diffusion, Jacobi method, Gauss-Seidel method.
![[Pasted image 20240903162223.png]]

$$
A[i,j] = \dfrac{A[i,j] + A[i-1,j] + A[i,j - 1] + A[i, j + 1]}{5}
$$
- Computation for a 5-point stencil.
- As these stencils gain more points, parallel computing is necessary for speed and efficiency.
- Serial Code:
```cpp
for(int t=0; t < num_steps; t++) {
	...
	for(i ...)
		for(j ...)
			A_new[i,j] = (A[i,j] + A[i-1,j] + A[i,j - 1] + A[i, j + 1]) 
							* 0.2
		// copy A_new into A... or better yet, play with pointers
}
```

# 2D Stencil Computation in Parallel
- 1D decomposition:
	- Divide rows (or columns) among processes.
	- Each process has to communicate with two neighbors (above and below).
	- C++ is *row major* so it ingests rows one by one, top down into memory.
		- Much easier and faster than ingestion by columns.
- 2D decomposition:
	- Divide both rows and columns (2D blocks) among processes.
	- Each process has to communicate with four neighbors.

# Prefix Sum
- Calculate sums of prefixes (running totals) of elements (numbers) in an array.
- Also called a "scan" sometimes.
```cpp
pSum[0] = A[0]

for (i=1; i<N; i++) {
	pSum[i] = pSum[i-1] + A[i]
}
```

| A    | 1   | 2   | 3   | 4   | 5   | 6   | ... |
| ---- | --- | --- | --- | --- | --- | --- | --- |
| pSum | 1   | 3   | 6   | 10  | 15  | 21  | ... |

## Parallel Prefix Sum
- Illustrated is the prefix sum completed in 3 steps as opposed to 7
![[Pasted image 20240903162151.png]]
#### In Practice
- You have $N$ Numbers and $p$ processes, $N > p$.
- Assign a $N$/$p$ block to each process.
	- Do the serial prefix sum calculation for the blocks owned on each process locally.
- Then do parallel algorithm with partial prefix sums (using teh last element from each local block)
	- Last element from sending process is added to all elements in receiving process' sub-block.

# Load Balance and Grain Size
- Load balance: try to balance the amount of work (computation) assigned to different threads/processes.
	- Bring ratio of maximum to average load as close to 1.0 as possible.
	- Secondary consideration: also load balance amount of communication.
- Grain size: ratio of computation-to-communication.
	- Coarse-grained (more computation) vs. fine-grained (more communication).