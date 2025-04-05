# Loop Scheduling
- Assignment of loop iterations to different worker threads.
- Default schedule tries to balance iterations among threads.
- User-specified schedules are also available.
## User-specific Scheduling
- Schedule clause: `schedule(type, chunk)
	- `type` specifications: 
		- `static`: iterations divided as evenly as possible (# iterations/# threads)
			- chunk $<$ (`#iterations` $/$ `#threads`) can be used to interleave threads.
		- `dynamic`: assign a chunk size block to each thread.
			- When a thread is finished, it retrieves the next block of `#chunk` iterations from an internal work queue.
			- Default chunk size = 1.
		- `guided`: similar to dynamic but start with a large block and gradually shrinks to size `#chunk` for handling load imbalance between iterations.
		- `runtime`: use the `OMP_SCHEDULE` environment variable.
### Mathematical Example
Calculating the value of 
$$
pi = \int_0^1(\frac{4}{1+x^2})dx
$$
```cpp
int main(int argc, char *argc[]) {
	...
	n = 10000;
	h = 1.0/(double)n;
	sum = 0.0;

	#pragma omp parallel for private(x) reduction(+: sum)
	for (i = 1; i <= n; i++) {
		x = h * ((double)i - 0.5);
		sum += (4.0 / (1.0 + x * x));
	}
	pi = h * sum;
	...
}
```
# Parallel Region
- All threads execute the structured block:
```
#pragma omp parallel [clause [clause] ... ]
	structured block
```
- Structured block: a block of one or more statements with one point of entry at the top and one point of exit at the bottom.
- Number of threads can be specified just like the parallel for directive.
# Synchronization
- Concurrent access to shared data may result in inconsistencies.
- Use mutual exclusion to avoid that.
- Some types:
	- Critical directive.
	- Atomic directive.
	- Library lock routines.
## Critical Directive
- Specifies that the code is only to be executed by one thread at a time:
```
#pragma omp critical [(name)]
	structured block
```
## Atomic Directive
- Specifies that a memory location should be updated atomically:
```
#pragma omp atomic
	expression
```

# OMP on GPUs
- GPGPU: General Purpose Graphical Processing Unit.
- Many slower cores.
![[Pasted image 20240918181125.png]]
- `target`: run on accelerator/device.
```cpp
#pragma omp target teams distribute parallel for
for (int i = 0; i < n; i++) {
	z[i] = a * x[i] + y[i];
}
```
- `teams distribute`: creates a team of worker threads and distributes work amongst them.


TO SLIDE 13 IN "MESSAGE PASSING AND MPI"
# Distributed Memory Programming Models
- Each process only has access to its own local memory/address space.
- When it needs data from remote processes, it has to send/receive messages.
# Message Passing
- Each process runs in its own address space.
	- Access to only their own memory (no shared data).
	- This is a restatement of the above.
- They use special routines to exchange data among each other.
- A parallel message passing program consists of independent processes.
	- Processes created by a launch/run script.
- Each process runs the same executable, but potentially different parts of the program, and on different data.
- Often used for SPMD (Single Program Multiple Data) style of programming.
	- Thus great for MIMD parallelization.
# Message Passing History
- PVM (Parallel Virtual Machine) was developed in 1989-1993.
- MPI forum was formed in 1992 to standardize message passing models and MPI 1.0 was released in 1994.
	- v2.0 - 1997
	- v3.0 - 2012
	- v4.0 - 2021

# Message Passing Interface (MPI)
- It is an interface standard - defines the operations/routines needed for message passing.
- Implemented by vendors and academics for different platforms.
	- Meant to be "portable": ability to run the same cod eon different platforms without modifications.
- Some popular open-source implementations are MPICH, MVAPICH, OpenMPI.
	- Vendors often implement their own versions optimized for their hardware: Cray/HPE, Intel.
