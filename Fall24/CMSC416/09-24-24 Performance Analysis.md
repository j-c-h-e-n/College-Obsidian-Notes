# Performance Modeling, Analysis, and Tools

## Weak VS Strong Scaling
- Strong scaling: Fixed total problem size as we run on more processes.
	- Sorting n numbers on 1 process, 2 processes, 4 processes, ...
	- Problem size per process decreases with increase in number of processes.
- Weak scaling: Fixed problem size per process but increasing total problem size as we run on more processes.
	- Sorting n numbers on 1 process.
	- 2n numbers on 2 processes.
	- 4n numbers on 4 processes.
## Amdahl's Law
- Speedup is limited by the serial portion of the code.
	- Often referred to as the serial "bottleneck".
- Lets say only a fraction $f$ of the code can be parallelized on $p$ processes.
$$ Speedup = \frac{1}{(1-f)+f/p}$$
- Speedup calculated from comparing how much faster execution time is in total is: $$Speedup = \frac{t_1}{t_p}$$
	- $t_1$ is the time it takes for 1 process.
	- $t_p$ is the time it takes for $p$ amount of processes.
## Performance Analysis
- Parallel performance of a program might not be what the developer expects.
- How do we find performance bottlenecks?
- Performance analysis is the process of studying the performance of a code.
- Identify why performance might be slow:
	- Serial performance.
	- Serial bottlenecks when running in parallel.
	- Communication overheads.
### Methods
- **Analytical techniques**: use algebraic formulae
	- In terms of data size (n), number of processes (p).
- **Time complexity analysis**: big O notation.
- **Scalability analysis**: Isoefficiency.
- More detailed modeling of various operations such as **communication**.
	- Analytical models: LogP, alpha-beta model.
- **Empirical performance** analysis using profiling tools.

# Analytical Techniques
## Parallel Prefix Sum
![[Pasted image 20240926173000.png]]
Assign $n/p$ elements (block) to each process.
Perform prefix sum on these blocks on each process locally.
- Number of calculations per process: $n/p$
Then do the parallel algorithm using the computed partial prefix sums.
- Number of phases: $log(p)$
- Total number of calculations per process: $log(p)*n/p$
- Communication per process (one message containing one key/number): $log(p)*1*1$

# Communication
## Modeling Communication: LogP Model
![[Pasted image 20240926173309.png]]
Used for modeling communication on the inter-node network.
- $L =$ Latency or delay.
- $o =$ Overhead (processor busy in communication).
- $g =$ Gap (required between successive sends/receives).
- $P =$ Number of processors/processes
## Alpha + N * Beta Model
Another model for communication:
$$T_{comm} = a + n * \beta$$
- $\alpha =$ Latency.
- $n =$ Size of message.
- $1/\beta =$ Bandwidth.
# Isoefficiency
The relationship between problem size and number of processes to maintain a certain level of efficiency.
Answers the question, "At what rate should we increase problem size with respect to number of processes to keep efficiency constant (iso-efficiency)?"
## Speedup and Efficiency
- Speedup: Ratio of execution time on one process to that on $p$ processes.
$$Speedup = \frac{t_1}{t_p}$$
- Efficiency: Speedup per process.
$$Efficiency = \frac{t_1}{t_p * p}$$
## Efficiency in Terms of Overhead
- Total time spent in all processes = (useful) computation + overhead.
	- Overhead can involve extra computation such as communication, idle time, etc.
$$p * t_p = t_1 + t_0$$
$$Efficiency = \frac{t_1}{t_p*p} = \frac{t_1}{t_1 + t_0} = \frac{1}{1+\frac{1_0}{t_1}}$$
- The Isoefficiency function is the third formula.
- Efficiency is constant if $t_0/t_1$ is a constant ($K$):
$$t_0 = K * t_1$$
## Isoefficiency Analysis
![[Pasted image 20240926174836.png]]
- 1D decomposition:
	- Computation: $\sqrt{n}*\frac{\sqrt{n}}{p} = \frac{n}{p}$ (area)
	- Communication: $2*\sqrt{n}$ (2 * width)
	- Efficiency:$$\frac{t_0}{t_1} = \frac{2*\sqrt{n}}{\frac{n}{p}}=\frac{2*p}{\sqrt{n}}$$
![[Pasted image 20240926174850.png]]
- 2D decomposition:
	- Computation: $\frac{\sqrt{n}}{\sqrt{p}}*\frac{\sqrt{n}}{\sqrt{p}} = \frac{n}{p}$
	- Communication: $2*\sqrt{n}$
	- Efficiency: $$\frac{t_0}{t_1} = \frac{4*\frac{\sqrt{n}}{\sqrt{p}}}{\frac{n}{p}} = \frac{4*\sqrt{p}}{\sqrt{n}}$$
## Empirical Performance Analysis
- Two parts to doing this type of analysis:
	- Measurement: gather/collect performance data from a program execuion.
	- Analysis/visualization: analyze the measurements to identify performance issues.
- Simplest tool: adding timers in the code manually and using print statements.
## Using Timers
- Use `MPI_Wtime()` to get current timing on MPI processes.
```cpp
double start, end;
double phase1, phase2, phase3;

start = MPI_Wtime();
... phase1 code ...
end = MPI_Wtime();
phase1 = end - start;

start = MPI_Wtime();
... phase2 code ...
end = MPI_Wtime();
phase2 = end - start;

start = MPI_Wtime();
... phase3 code ...
end = MPI_Wtime();
phase3 = end - start;
```
## Performance Tools
- Tracing tools
	- Captures entire execution trace, typically via instrumentation.
- Profiling tools
	- Provide aggregated information.
	- Typically uses statistical sampling.
- Many tools can do both of these.

- These tools usually keep track of the following metrics:
	- *Counts* of function invocations.
	- *Time spent* in each function/code region.
	- *Number of bytes sent* (in case of MPI messages)
	- Hardware counters such as *floating point operations, cache misses*, etc.
- To fix performance problems, we need to connect metrics to the source code, which we use *tracing tools* to do so.
## Tracing Tools
- Records all the events in the program with enter/leave timestamps.
- Events: user functions, MPI, and other library routines, etc.
![[Pasted image 20241002144802.png]]
- List of tracing tools:
	- VampirTrace
	- Score-P
	- TAU
	- Projections
	- HPCToolkit
## Profiling Tools
- Ignores specific times at which events happen.
- But they provide aggregate information about time spend in different functions/code regions.
- Some profiling tools:
	- gprof
	- ![[Pasted image 20241002145117.png]]
	- perf
	- mpiP
	- MPCToolkit
	- caliper
- Python tools:
	- cprofile
	- pyinstrument
	- scalene
## Calling Contexts, Trees, and Graphs
- <mark style="background: #D2B3FFA6;">Calling Context/Call Path</mark>: Sequence of function invocations leading to the current sample (statement in code).
- <mark style="background: #D2B3FFA6;">Calling Context Tree (CCT)</mark>: Dynamic prefix tree of all call paths in an execution.
- <mark style="background: #D2B3FFA6;">Call Graph</mark>: obtained by merging nodes in a CCT with the same name into a single node but keeping caller-callee relationships as edges. *Condenses a CCT and demonstrates a nice flow chart*.
![[Pasted image 20241002145507.png]]
- Profiling tools may output the following:
	- Flat profiles: Listing of all invoked functions with counts and execution times.
	- Call graph profile: unique node per function.
	- Calling context tree: unique node per calling context.
## Hatchet: Performance Analysis Tool
- We're using hatchet(?)
- Hatchet enables programmatic analysis of parallel profiles.
- Leverages pandas with supports multi-dimensional tabular datasets (DataFrames).
- Create a structured index to enable indexing pandas df by nodes in a graph.
- A set of operators to filter, prune, and/or aggregate structured data (built into pandas).

### GraphFrame
- The main structure within hatchet.
- Consists of a structed index graph object and a pandas dataframe.
- Graph stores caller-callee relationships.
- Dataframe stores all numerical and categorical data for each node in the graph.

# GraphFrame Functions
## gf.filter
- Once df.filter is applied and there is now a variable to hold the squashed version of the df, use `df.squash()` to sync the tree structure with the filtered dataframe.
## gf subtraction
- You can subtract to obtain a difference between two graphframes.
- Useful for comparisons and noting what has changed between different runs.
## Visualizing Small Graphs
- `print(gf.tree(color=True))` will return a wireframing of the call order.
- `gf.to_dot()` will return a tree-type diagram.
	- You can write it to a file using `with open ("file.dot", "w") as dot_file: dot_file.write(gf.to_dot())`
- `gf.to_framegraph()` will return a "flame graph"
	- ![[Pasted image 20241003194711.png]]
	- You can write it to a file using `with open("file.txt", "w") as folded_stack: folded_stack.write(gf.to_flamegraph())`

## Reading Data
```python
import hatchet as ht
import sys

if __name__ == '__main__':
	file = sys.argv[1]
	gf = ht.GraphFrame.from_caliper(file)

	print(gf.tree())
	print(gf.dataframe)
```
