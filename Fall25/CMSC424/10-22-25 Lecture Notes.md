Normalization is covered on exam. Topics after normalization will not be covered on the exam on 10/27.
# Storage
- A relatively new topic.
- "How do we build these databases".
- Useful for database optimization.
# Query Processing/Storage
- Query Processing Engine
	- Given some user query, decide how to execute it.
	- Specify sequence of pages to be brought in memory.
	- Operate upon the tuples to produce results.
- Buffer Management
	- Bringing pages from disk to memory.
	- A temporary intermediate space that serves as memory.
	- Have to manage the limited memory.
- Space Management on Persistent Storage
	- Storage hierarchy.
	- Where are relations stored?
	- How are tuples mapped to disk blocks?