# Parallel Filesystem
## When Do Parallel Programs Perform I/O?
- Reading input datasets.
- Writing numerical output.
- Writing checkpoints.
## Non-Parallel I/O
- Designated process does I/O.
- All processes send data to/receive data from that one process.
- Not scalable.
## Parallel Filesystem
- Home directories and scratch space are typically on a parallel file system.
- Mounted on all login and compute nodes.
- Also referred to as I/O sub-system.
![[Pasted image 20241203234857.png|400]]
![[Pasted image 20241203234909.png||400]]
## Links Between Cluster and Filesystem
![[Pasted image 20241203235018.png]]
## Different Parallel Filesystems
- Lustre: open-source
- BeeGFS: community and commercially supported.
- GPFS: General Parallel File System
	- IBM product. Now called Spectrum Scale
- PVFS: Parallel Virtual File System
### Benefits of Parallel Filesystems
- Improves I/O bandwidth by spreading read/write across multiple OSTs (Object Storage Target) such as disks, even for single files.
- Files can be striped within and across multiple I/O servers (OSS).
- Each client (compute node) runs an I/O daemon to interact with the parallel filesystem mounted on it.
- MDS (Metadata server) serves file metadata (ownership, permissions), and in-node/directory updates.
#### Tape Drives
- Magnetic tape that we store data on.
- Used for archiving data (Tape is long-term stable).
- Use robotic arms to access the right tape.
- ![[Pasted image 20241203235407.png|300]]
# Burst Buffer
- Fast intermediate storage between compute nodes and the parallel filesystem.
	- Typically some form of non-volatile (NVM, essentially super stable RAM) memory, for persistence, high capacity, and speed (read and writes).
	- Slower, but higher capacity than on-node memory (DRAM).
	- Faster, but lower capacity than disk storage on parallel file system.
		- Tape Drive, HDD, SSD, Burst Buffer, DRAM (DDR2, DDR3, ...)
			- Some burst buffer implementations use SSDs.
- Two designs:
	- Node-local burst buffer.
	- Remote (shared) burst buffer.
![[Pasted image 20241204124920.png]]
## Use Cases
- Storing checkpoint data.
- Prefetching input data.
- Workflows that couple simulations to analysis/visualization tasks.
# I/O Libraries
- High-level libraries: HDF5, NetCDF.
	- Both are libraries and file formats for n-dimensional data.
- Middleware: MPI-IO
	- Support for POSIX like I/O in MPI for parallel I/O.
- Low-level: POSIX IO
	- Standard Unix/Linux I/O interface.
## Different I/O Patterns
- One process reading/writing all the data.
- Multiple processes reading/writing data from/to same file.
- Multiple processes reading/writing data from/to different files.
- Performance depends on the number of readers/writers (how many processes/threads), file sizes, filesystem, etc.
## I/O Profiling Tools
- Darshan
	- Lightweight profiling tool from Argonne National Library.
- Recorder
	- Research tool from UIUC.
	- Tracing framework for capturing I/O activity.
	- Provides support for different I/O libraries: HDF5, MPI-IO, POSIX IO