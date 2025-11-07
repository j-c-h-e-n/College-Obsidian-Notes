Query hierarchy in the previous notes.
This half of the semester talks about "How to build a database".
# Storage Hierarchy
- Cache
- Main memory (RAM)
- Flash memory
- Magnetic disks
- Optical disks
- Magnetic tape
## Cache
- Super fast; volatile (loses state on power loss); typically on chip or very close to chip.
- L1 vs L2 vs L3 caches
	- L1 about 64KB; L2 about 1MB; L3 8MB (on chip) to 256 (off chip)
## Main memory
- 10s or 100s of ns; volatile.
- Cheap and prices are still dropping
## Flash
- Slower than main memory (DDR3/4/5).
## Magnetic Disk
- non-volatile
- Much slower than memory.
# Storage
- Primary
	- Main memory, cache, typically volatile, fast.
- Secondary
	- Disks, SSDs, HDDs, non-volatile.
- Tertiary
	- Usually for backups, slow.
# Simple Storage Hierarchy
- CPU registers $\rightarrow$ cache $\rightarrow$ memory $\rightarrow$ I/O devices
- Much or our discussions will be maintaining our important data on our memory, preventing from us needing to access our disk storage (orders of hundreds of thousands slower).
![[Pasted image 20251029144842.png]]
# Magnetic Disks
## Accessing Data (on Magnetic Disks)
- Time to *seek* to the track (seek time)
	- average 4 to 10ms.
- Waiting for the sector to get under the head (rotational latency)
	- average 4 to 11ms.
- Time to transfer the data (transfer time)
	- very low.
- About 10ms per access.
	- If random access can only do 100 block transfers/second: 100 x 512 bytes = 50KB/s.
- Data transfer rates (sequential)
	- Rate at which data can be transferred without seeks.
	- 30-50MB/s to up to 500MB/s (compared to random access of 50KB/s).
		- Seeks are bad.
## Reliability
- Mean time to/between failure (MTTF/MTBF):
	- 57 to 136 years.
- Consider:
	- 1000 new disks
	- 1,200,000 hours of MTTF each.
	- On average, one will fail 1200 hours = 50 days.
		- Failures are often and we need to optimize for them.
# Indexing
(Main topic for project 3).
## Sequential File Organization
- Keep sorted by some search key.
- Insertion:
	- Find the block in which the tuple should be.
	- If there is free space, insert it.
	- Otherwise, must create *overflow pages*.
- Deletions:
	- Delete and keep the free space.
	- Databases tend to be insert heavy, so free space gets used fast.
- Can become *fragmented*
	- Needs to be reorganized once in a while.
- What if I want to find a particular record by value?
	- Account info for ID=123.
	- `SELECT * FROM relation WHERE relation.ID = 123`
	- Approaches:
		- Go into every file, scan everything for the matching case.
			- If no index (ID is not a primary key), we can perform binary search to find target, but only if data is sorted.
			- Requires log(n) accesses, but these jumps are seeks, each takes significant time. **Sometimes, it's faster to do a sequential search than a binary search**.
				- If we have n=1,000,000,000, then log(n) = 30 jumps.
				- If each jump takes 10MS, then it takes 300MS to find just one account. Only fulfilling <4 requests per minute.
## Ordered Indexes

## Hashed Indexes
### Dense Index
- Can be used for primary and secondary indices.
- Every key must appear in the index.
- Can answer of "what doesn't exist" without having to actually go there. Very good at validation(?).
### Primary Sparse Index
- When accessing sparse indexes, we access every index until we go over the target. Once we recognize we've gone over (hopefully only by 1 sparse index) we return to the previous sparse index, then sequentially read until we get the index we want.
	- Ex: sparse indexes of 21, 41, 81, 121 and we want index 120. We go through 21, 41, 81, 121. At 121 we realize we've gone over and we can't sequentially read backwards. So we return to 81 and then proceed to 120 from there.
## Secondary Index

## Multi-level Indexes
 - B-Trees.
 - Sparse indexing that goes to another level of sparse (or dense) indexes, which then links to another level of more indexes (sparse or dense), etc.
 - Not great when we need to write to the indexes.
 - 