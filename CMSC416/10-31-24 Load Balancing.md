# Performance Issues
- Sequential performance issues.
- Load imbalance.
- Communication performance issues / parallel overhead.
- Algorithmic overhead / replicated work.
- Speculative loss.
- Critical paths.
- Insufficient parallelism.
- Bottlenecks.

# Load Imbalance
- Def: unequal amounts of "work" assigned to different processes/threads.
	- Work could be computation or communication or both.
- Results in overloaded processes, which means greater potential for an overall slow down of all processes.
$$Load Imbalance = \frac{maxload}{meanload}$$
# Load Balancing
- The process of balancing load across threads, processes, etc.
- Goal: to bring the maximum load as close to the average load as possible.
- Steps:
	- Determine if load balancing is needed.
	- Determine when and how often to load balance.
	- Determine what information to gather/use for load balancing.
	- Choose/design a load balancing algorithm.

## Determining if Load Balancing is Needed
- Need the distribution of load ("work") across processes.
- Collecting empirical information using performance tools.
- Developer knowledge.
- Analytical models of load distribution.
## When/How Often to Load Balance?
- Load balance during:
	- Initial work distribution (program start ups, etc).
	- Partitioning.
	- Static load balancing (load balancing without regards to how processes are at the moment, i.e. predetermined plans to load balance).
- Dynamic load balancing:
	- During program execution.
	- How often depends.
# Information Gathering for Load Balancing
- Centralized load balancing:
	- Gather all load information at one process - global view of data.
- Distributed load balancing:
	- Every process only knows the load of a constant number of "neighbors".
- Hybrid or hierarchical load balancing.
	- ![[Pasted image 20241203174500.png]]
## Information Used for Load Balancing
- Computational load.
- Communication load (number/sizes of message).
- Communication graph.

# Load Balancing Algorithms
- Input: Amount of work ($n_i$) assigned to each process $p_i$.
- Output: New assignments of work units to different processes.
- Goals:
	- Bring maximum load close to average.
	- Minimize the amount of data migration (moving data/communication).
- Secondary goals:
	- Balance (possibly reduce) communication load (volume).
	- Optimize the amount of time it takes for the program to load balance.
## Simple Greedy
- Sort all the processes by their load (centralized process).
- Take some work from the heaviest loaded process and give it to the lightest loaded process. Repeat.
## Work Stealing
- Decentralized strategy where processes steal work from neighbors when they have no work.
- Each process has a queue of work items.
   	- A process looks at other queues when there are no items remaining.
- Implemented in Cilk, Cilk++, Cilk Plus, OpenCilk (general-purpose multithreaded parallel computed based on C).
## Considerations for Strategies
- Communication-aware load balancing:
	- Try to move work to processes that this work communicates with frequently.
- Network topology-aware load balancing:
	- Take into account how the nodes are connected to one another to minimize some metrics (number of hops, average link load, etc).
### Static Load Balancing Examples
- Decomposition of n-D stencil.
- Using orthogonal recursive bisection (ORB), space-filling curves, etc.
- ORB: 
	- ![[Pasted image 20241203180210.png|300]]
- Space-filling curves:
	- ![[Pasted image 20241203180312.png]]

