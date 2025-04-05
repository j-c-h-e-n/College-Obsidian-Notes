# Molecular Dynamics
- Calculate trajectories of atoms and molecules by solving Newton's equations of motions.
- Force calculations:
	- Bonded interactions: bonds, angles, dihedrals.
	- Non-bonded interactions: van der Waal's and electrostatic forces.
- Number of atoms: thousands to millions.
- Simulation step: ~1 femtosecond ($10^{-15} s$)
- Used for drug design, materials design.
## Sequential Algorithm
- At every step, calculate forces on each atom.
	- Calculate bonded and short-range forces every step.
	- Calculate long-range on-bonded forces every few time steps (using PME or P3M etc.).
- Particle mesh Ewald (PME) summation:
	- Calculate long-range interactions in Fourier space.
- Calculate velocities and new positions.
- Repeat...
## Traditional Approaches to Parallelization
1. Atom Decomposition
	- Partition the atoms across processes (permanently assigning atoms to processes).
	- Every process is responsible for calculating forces and updating their own atoms.
	- Cons:
		- Limited scalability due to communication overhead as the number of processors grow.
		- Potential load imbalance from unevenly distributed atoms.
		- Inefficient for situations where atoms are frequently redistributed.
2. Force Decomposition
	- Distribute the force matrix to processes.
	- Matrix is sparse and non-uniform.
	- Cons:
		- More complicated than other approaches.
		- Potential increased memory usages due to redundant storage of atomic data across processors.
		- May require more complex communication patterns.
3. Spatial Decomposition
	- Assign a region of the 3D simulation space to each process.
	- Each process is responsible for the atoms that are under the space which it is assigned to.
	- Cons:
		- Potential load imbalance with non-uniform density.
		- Increased complexity for moments when atoms move across region boundaries.
## The Fourth Approach: Hybrid Parallelization
- A mix of spatial and force decomposition.
- Decouple assignment of data and work to processes.
- Distribute both atoms and the force calculations to different processes.
## Neutral Territory (NT) Methods
- Desmond's mid-point method:
	- ![[Pasted image 20241204161516.png]]
## Particle Mesh Ewald
A Molecular Dynamics technique for efficient long-range electrostatic calculations using Fourier space.
- Replace direct force calculations by:
	- Calculate short-range forces in real space.
	- Calculate long-range forces in Fourier space.
- Create a 3D mesh/grid representing charge densities of atoms.
	- Compute a 3D Fast Fourier Transform (FFT).
- FFT computes the discrete Fourier transform (DFT) or inverse DFT.
	- Reduces the complexity from O($N^2$) to O($N*log(N)$)
## Parallelization of PME
- Bring all data to one process.
- 1D or slab decomposition
	- ![[Pasted image 20241204171150.png]]
- 2D or pencil decomposition
	- ![[Pasted image 20241204171215.png]]
## Example: Parallel Simulation of the Spread of Measles
![[Pasted image 20241204171804.png]]
### Societal Challenge:
- Using computational and mathematical modeling of epidemics to assist governments in responding to outbreaks.
- Hard on computation when:
	- Increased and denser urbanization.
	- Increased local and global travel.
	- Increasingly immuno-compromised population.
#### Approach:
- Agent-based modeling to simulate epidemic diffusion.
- Model agents (people) and interactions between them.
- Idea:
	- People interact when they visit the same location at the same time.
	- These "interactions" between pairs of people are represented as "visits" to locations.
- Use a bi-partite graph of people and locations or a people-people interactivity graph.
#### Serial Algorithm:
- At each timestep
	- Determine which people visit which locations.
	- "Send" agents to those locations.
	- At each location "interactions" happen and transmission happens.
	- Update agent states at the end of the day and continue.
- Interventions (vaccinations, school closures) can be added on certain days to change people's susceptibility, movements, etc.
#### Combination of Network Theory and Discrete Event Simulations
![[Pasted image 20241204173117.png]]
#### Parallel Simulation is Challenging
- Size and scale of the social contact network (6 billion agents...)
	- Unstructured networks and complicated dependencies lead to high communication cost.
- Individuals and their behaviors are not identical.
- Co-evolving epidemics, public policies, and agent behaviors make it impossible to apply standard model reduction techniques (too many variables to account for).
### Solution: Loimos Parallel Implementation.
- All the people and locations are distributed amongst all processes.
- DES computation can be done locally in parallel.
- Communication when sending visit and infection messages.
- Uses Charm++, a message driven model.
	- ![[Pasted image 20241204173502.png|400]]
## Application Software Stack
- Parallel programming model / runtime:
	- MPI, OpenMP, Charn++, CUDA, ...
- Libraries:
	- Data and visualization libraries (mesh management, simulation output).
	- I/O libraries.
	- Math/numerical libraries.
	- Graph partitioning, load balancing, ...
### Significance of Libraries
- No need to reinvent the wheel.
	- Libraries are usually highly optimized and have fewer bugs than if you did it yourself.
- Avoids significant effort to write, optimize, and maintain code if you wrote it yourself.
- Makes code more portable.
### Popular Libraries
- Data/visualization and I/O libraries.
	- I/O: HDF5, pNetCDF, ADIOS
- Numerical libraries:
	- Fast Fourier transforms: FFTW.
	- Dense linear algebra: BLAS, LAPACK, Intel MKL.
	- Solvers for sparse systems: Hypre, PETSc, Trilinos.
- Graph partitioning/load balancing:
	- METIS, Scotch, Zoltan, Chaco
### Domain-specific Languages/Frameworks
- Structured grids: SAMRAI, Chombo, AMREx.
- Unstructured grids: MFEM, Quinoa.

# The N-Body Problem
- Simulate the motion of celestial objects interacting with one another due to gravitational forces.
- Naive algorithm: O($n^2$)
	- Every body calculates forces pair-wise with every other body (particle).
## Data Distribution in N-Body Problems
- Naive approach: assign n/p particles to each process.
- Other approaches:
	- Space filling curves.
	- ORB (orthogonal recursive bisection).
Let us consider a 2-D space with bodies/particles in it:
![[Pasted image 20241204180013.png|200]]
Using ORB, we can split the space apart:
![[Pasted image 20241204180137.png|500]]
## Different Parallelization Methods
- Tree codes: Barnes-Hut simulations.
- Fast multipole methods (FMM): Greengard and Rokhlin.
- Particle mesh methods.
- Particle-particle particle-mesh (P^3M) methods.
### Barnes-Hut Simulation
- Represent the space containing the particles as an oct-tree.
- Pairwise force calculations for nearby particles.
- For tree nodes that are sufficiently far away, approximate the particles int he node by a single large particle at the center of mass.
- O($N*log(N)$) complexity.
- ![[Pasted image 20241204181156.png|400]]
### Fast Multipole Methods
- Use multipole expansion for distant particles.
- Takes advantage of the fact that for nearby particles, multipole-expanded forces from distant particles are similar.
- Reduces the time complexity further to O($n$)
### Particle-Particle Particle-Mesh Methods (P^3M)
- Explicit calculation of forces on nearby particles.
- Fourier-based Ewald summation or calculating potentials on a grid.
- Smoothed particle hydrodynamics.
