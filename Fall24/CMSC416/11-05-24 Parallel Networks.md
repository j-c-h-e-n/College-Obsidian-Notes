# High-Speed Interconnection Networks
- Typically supercomputers and HPC clusters are connected by low latency and high bandwidth networks.
- The connections between nodes form different topologies.
- Popular topologies:
	- Fat-tree: Charles Leiserson in 1985.
	- Mesh and torus networks.
	- Dragonfly networks.
## Network Components
- Network interface controller or card.
- Router or switch.
- Network cables: copper or optical.
##### Definitions
- Network hops/distance: number of links a message must travel between the source and destination switch.
- Network diameter: length
	- Gives an idea of the worse case latency on a network.
- Radix: number of ports on a router.

## N-Dimensional Mesh / Torus Networks
- Each switch has a small number of nodes connected to it (often one or two).
- Each switch has direct links to $2n$ switches where $n$ is the number of dimensions.
- Torus = mesh + wraparound links.
	- ![[Pasted image 20241203184714.png|200]]
- Ex:
	- IBM Blue Gene
	- Cray X* machines
### Network Properties of Mesh/Torus
- Let's say the number of switches is $s$, and number of nodes per switch is a small constant, $c$.
- Mesh Diameter:
	- 1-D: $s-1$
	- n-D: $(\sqrt[n]{s}-1)*d$ 
- Torus Diameter:
	- 1-D: $\lfloor\frac{s}{2}\rfloor$
	- n-D: $\lfloor\frac{\sqrt[n]{s}}{2}\rfloor*n$
- Maximum number of nodes: $c * s$
	- (Nodes per switch times switches)
## Fat-tree Network
- Radix (number of ports on a router): $k$
- Number of nodes per router: $\frac{k}{2}$
- A pod is a group of $\frac{k}{2}$ switches at each level.
- Maximum number of pods = $k$
- Ex:
	- ![[Pasted image 20241203194313.png]]
	- Radix = 8.
	- 4 nodes per router (router is just the grouping)
	- 4 pods (the dotted outline).
	- max pods = 8, there are only 4.
### Network Properties of Fat-tree
- $s$ = number of switches. $k$ = radix.
- Diameters:
	- 2-level: 2
	- 3-level: 4
	- I-level: $(I-1)*2$
- Max number of nodes: $k*\frac{k}{2}*\frac{k}{2}=\frac{k^3}{4}$
## Dragonfly Network
- Two-level hierarchical network using high-radix routers.
- Low network diameter.
	- ![[Pasted image 20241203202450.png]]
### Network Properties of Dragonfly
- Diameters:
	- All-to-all connections within supernode: 3
	- Row-column all-to-all connections within supernode: 5
# Messages
## Life-cycle of a Message
![[Pasted image 20241203202626.png]]
## Congestion Due to Network Sharing
- Sharing refers to network flows of different programs using the same hardware resources: links, switches.
- When multiple programs communicate on the network, they all suffer from congestion on shared links.
![[Pasted image 20241203204557.png]]
## Routing Algorithm
- Decides how a packet is routed between a source and destination switch.
- Static routing: each router is pre-programmed with a routing table.
	- Can change it at boot time.
- Dynamic routing: routing can change at runtime.
- Adaptive routing: adapts to network congestion.
## Performance Variability
![[Pasted image 20241203205426.png]]
## Performance Variability Due to Congestion
- No variability in computation time.
- All of the variability can be attributed to communication performance.
- Factors:
	- Placement of jobs.
	- Contention for network resources.
	- ![[Pasted image 20241203205641.png|500]]
## Different Approaches to Mitigating Congestion
- Network topology aware node allocation.
- Congestion or network flow aware adaptive routing.
- Within a job: network topology aware mapping of processes or chares to allocated nodes.
## Topology-aware Node Allocation
- Main takeaway: allocate nodes in a manner that prevents sharing of links by multiple jobs. Try to keep it within the "lowest common denominator" of grouping.
## AFAR: Adaptive Flow Aware Routing
- Main takeaway: Dynamically re-route traffic to alleviate hot-spots.
- Given some traffic for each pair of nodes in the system and the current routing:
	1. Calculate current load (network traffic) on all links in system.
	2. Find link with maximum load.
	3. If max > threshold, re-route one flow crossing that link to an under-utilized link.
	4. Repeat from 1. using new routing.
## Topology-Aware Mapping
- Within a job allocation, map processes to nodes intelligently.
- Inputs: application communication graph, machine topology.
- Graph embedding problem (NP-hard).
- Many heuristics to come up with a solution.
- Can be done within a load balancing strategy.

