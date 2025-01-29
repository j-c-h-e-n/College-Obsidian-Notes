GOOD TO KNOWS:
- DO NOT RUN CODE ON THE LOGIN NODE
- DO NOT RUN SUDO ON ZARATAN.
- 
# Shared Memory and OpenMP
## Architectures
- All processors/cores can access all memory as a single address space.
- Incredibly hard to do uniform memory access (complicated).
	- Better to give each CPU their own and use bus interconnects.
		- NUMA - Non-Uniform Memory Access
	![[Pasted image 20240905153846.png]]
### Distributed memory architecture
- Groups of processors/cores have access to their local memory.
- Writes in one group's memory have no effect on another group's memory.
	- Processors in NUMA *CAN* access the memory associated with other processors.
	![[Pasted image 20240905154206.png]]

## Programming Models
- Shared memory model: All threads have access to all of the memory.
	- Runs within nodes.
	- pthreads, OpenMP, CUDA.
- Distributed memory model: Each process has access to its own local memory
	- Also sometimes referred to as message passing.
	- Can run across nodes with tis.
	- MPI, Charm++.
- Hybrid models: use of both shared and distributed memory models together in the same program.
	- Alternative solution when both forms are needed, to lessen complexity.
	- MPI+OpenMP, Charm++ (SMP mode).

### Shared memory programming
- All entities (threads) have access to the entire address space.
- Threads "communicate" or exchange data by directly accessing shared variables.
- Programmer has to manage data conflicts.


# OpenMP
- Overview:
	- OpenMP is an example of a shared memory programming model.
	- Provides within-node parallelization (CANNOT RUN ACROSS NODES)
	- Targeted at some kinds of programs/computational kernels.
		- Specifically ones that use arrays and loops.
	- Potentially easier to implement programs in parallel using OpenMP with small code changes (as opposed to distributed memory programming models, which may require extensive modifications to the serial program).
- OpenMP is a *language extension* (and library) that enables parallelizing C/C++/Fortran code.
- Programmer uses compiler directives and library routines to indicate parallel regions in the code and how to parallelize them.
- Compiler converts code to multi-threaded code.
- OpenMP uses a fork/join model of parallelism.
## Fork-join parallelism
- Single flow of control
- Primary thread spawns worker threads.
	![[Pasted image 20240905160223.png]]
## Race Conditions when Threads Interact
- Unintended sharing of data/variables can lead to race conditions.
- Race condition: programs outcome depends on the scheduling order of threads.
	- More than one thread accesses a memory location and at least one of them writes to it (without proper synchronization).
- We want program outcome to be deterministic and same as serial program.
- How can we prevent data races?
	- Use synchronization, which can be expensive.
	- Change how data is accessed to minimize the need for synchronization.
## OpenMP Pragmas
- Pragma: a compiler directive in C or C++
	- Mechanism to communicate with the compiler.
	- Compiler may ignore pragmas.
	`#pragma omp construct [clause [clause] ...]`
- Just tells it how to parallelize your code.

# Hello World in OpenMP
```c
#include "omp.h"

void main()
{
	#pragma omp parallel
	{
		int thread_id = omp_get_thread_num();
		printf("Hello, world from %d.\n", thread_id);	
	}
}
```
- The `#pragma` and the `{}` denotes where the fork happens and ends.
- Compile command: `gcc -fopenmp hello.c -o hello`
- Setting thread count: `export OMP_NUM_THREADS=2`
## Parallel For
- Directs the compiler that the immediately following for loop should be executed in parallel.
- *Only applies to the immediately following for loop* even if you have nested for loops.
```c
int main(int arc, char **argv)
{
	int a[100000];
	#pragma omp parallel for
	for (int i = 0; i < 100000; i++)
	{
		a[i] = 2 * i;
	}

	return 0;
}
```
## Parallel for execution
- Primary thread creates worker threads
- The OpenMP runtime distributes iterations of the loop to different threads.
	![[Pasted image 20240905163241.png]]
## Number of threads
- You can set it using this environment variable before executing the program:
						`export OMP_NUM_THREADS=X`
- From within the program, you can call this library routine to set the number of OpenMP Threads to be used in parallel regions:
				`void omp_set_num_threads(int num_threads);`
	- This overrides the original setting.
- You can get the number of available hardware cores on the node and can be used to decide the number of threads to create:
						`int omp_get_num_procs(void);`
- 