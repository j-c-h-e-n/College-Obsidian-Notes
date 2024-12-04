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
