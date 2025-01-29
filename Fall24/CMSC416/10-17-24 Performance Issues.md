## Metrics
- Time to solution.
- Time per step/iteration.
- Science progress (figure of merit per unit time).
- Floating point operations per second (flop/s).
- When comparing multiple data points:
	- Speedup.
	- Efficiency.

## Best Performance
We achieve best performance when the following are maxed out:
- Flop/s
- Memory bandwidth
- Network bandwidth

## Things Happening in a Program
- Integer operations.
- Floating point operations.
- Conditional instructions.
- Loads/stores
- Data movement across the network (messages + I/O)

# Performance Issues
#### Serial Code Issues:
- Inefficient memory access: data movement in the memory hierarchy.
- Inefficient floating point operations.
#### Load Imbalance:
- Some process doing more work than most (uneven spread of tasks amongst process pool).
#### Communication Issues / Parallel Overhead:
- Spending increasing proportion of time on communication.
#### Algorithmic Overhead / Replicated Work:
- More computation when running in parallel (e.g. prefix sum).
#### Speculative Loss:
- Perform extra computation speculatively but not use all of it for the result. (Unnecessary computations).
#### Critical Paths:
- Dependencies between computations spread across processes/threads.
#### Insufficient Parallelism
#### Bottlenecks:
- Serial Bottlenecks: One process doing some computation while inadvertently blocking.
## Serial Code Performance Issues:
- We can identify issues using performance tools such as Hatchet.
- Solutions:
	- Minimizing data movement (not talking about communication between processes).
	- Maximizing data reuse.
	- Optimizing floating point calculations.
## Communication Performance Issues
- Overhead and grainsize (Lots of tiny messages or a fewer, larger messages).
- No overlap between communication and computation.
- Increasing amounts of communication as we run with more processes/threads.
- Frequent global synchronization.
## Critical Paths
- A long chain of operations with consecutive dependencies across processes.
- We want to identify and avoid having long critical paths.
	- Eliminate completely if possible, or shorten it.
	- Reduce time spent in a path by removing work on the critical path.
![[Pasted image 20241031150006.png]]
## Serial Bottlenecks
- Detect serial bottlenecks: things getting "serialized".
	- One process busy while all others wait.
- Examples of serial bottlenecks:
	- Reduce to one process and then broadcast.
	- One process responsible for input/output.
	- One process responsible for assigning work to others.
- Solutions:
	- Parallelize as much as possible, use hierarchical schemes.
## Performance Variability
- We want consistent run times, having variable performance is a real concern.![[Pasted image 20241031150640.png]]
- Performance variability can lead to several problems:
	- Usage problems:
		- Individual jobs will run slower:
			- More time to complete science simulations.
			- Increased wait time in job queues.
			- Inefficient use of machine time allocation.
		- Overall lower system throughput.
		- Increased energy usage/costs.
	- Development Cycle Problems:
		- Debugging performance issues.
		- Quantifying the effect of various software changes on performance (can't pinpoint benefits).
			- Code changes.
			- System software changes.
		- Estimating time for a batch job or simulation (because it isn't consistent).
### Sources of Performance Variability
- Operation System influences (noise/jitter).
- Contention for shared resources:
	- Network
	- Filesystem

## Operating System Influences
- Nodes on an HPC cluster may have:
	- A "full" Linux kernel or a light-weight kernel.
- The different kernels/flavors of Linux have different services/daemons that run.
	- Impacts performance predictability.
### OS Noise
- Also called 'jitter'
- Impacts computation due to interrupts by OS.
![[Pasted image 20241031152744.png|500]]
- We can measure the noise.
- Fixed Work Quanta (FTW) and Fixed Time Quanta (FTQ)
![[Pasted image 20241031152819.png|600]]![[Pasted image 20241031152926.png|600]]
### Mitigating OS Noise
- Running a light-weight OS (some minimal Linux flavor/distro).
- Turn off unnecessary daemons.
- Reducing the frequency of daemons.
- Dedicated cores for OS daemons.
- User programs can avoid user certain cores.