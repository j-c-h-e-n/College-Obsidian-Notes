# Message Passing
# Hello World in MPI
```cpp
#include "mpi.h"
#include <stdio.h>

int main(int argc, char *argv[]) {
	int myrank, numpes;
	MPI_Init(&argc, &argv);

	MPI_Comm_rank(MPI_COMM_WORLD), &myrank);
	MPI_Comm_size(MPI_COMM_WORLD, &numpes);
	printf("Hello world! I'm %d of %dn", myrank, numpes);

	MPI_Finalize();
	return 0;
}
```
- `int MPI_Init(int argc, char **argv)`
	- Initializes the MPI Execution environment.
- `int MPI_Finalize(void)
	- Terminates the MPI execution environment.
	- ALL MPI processes must call this before the process exits (not including errors).
- `int MPI_Comm_size(MPI_Comm comm, int *size)`
	- Determines the size of the group associated with a communicator.
- `int MPI_Comm_rank(MPI_Comm comm, int *rank)`
	- Determines the rank (ID) of the calling process in the communicator.
- Communicator - a set of processes identified by a unique tag.
	- Default communicator: `MPI_COMM_WORLD`
# Compiling and Running an MPI Program
- Compiling:
	- `mpicc -o hello hello.c`
- Running:
	- `mpirun -n 2 ./hello`
	- "2" denotes how many processes to spin up.
	- Throw this inside your sbatch script.\

# PT2PT
## Send a Blocking PT2PT Message
- `int MPI_Send(const void *buf, int count, MPI_Datatype datatype, int dest, int tag, MPI_Comm comm)
	- `buf`: address of send buffer.
	- `count`: number of elements in send buffer.
	- `datatype`: datatype of each send buffer element.
	- `dest`: rank of destination process.
		- needs `MPI_Comm_rank()`
	- `tag`: message tag.
	- `comm`: communicator.
		- almost always `MPI_COMM_WORLD`
- One sender, one receiver.
## Receive a Blocking PT2PT Message
```cpp
int MPI_Recv(void *buf, int count, MPI_Datatype datatype, int source, int tag, MPI_Comm comm, MPI_Status *status)
```
- `buf`: address of receive buffer.
	- You must ensure the buffer is big enough to receive all of the data.
- `count`: maximum number of elements in receive buffer.
- `datatype`: datatype of each receive buffer element.
- `source`: rank of source process.
	- The rank of the process that's sending the message.
	- Can set to "don't care".
- `tag`: message tag.
- `comm`: communicator.
- `status`: status object.
	- Serves as a verifier. Keeps track of how much data is actually received.
### MPI_Status Object
- Represents the status of the received message.
```cpp
typedef struct _MPI_Status {
	int count;
	int cancelled;
	int MPI_SOURCE;
	int MPI_TAG;
	int MPI_ERROR;
} MPI_Status, *PMPI_Status;
```
- `count`: number of received entries.
- `MPI_SOURCE`: source of the message.
- `MPI_TAG`: tag value of the message.
- `MPI_ERROR`: error associated with the message.

## Semantics of PT2PT Communication
- A receive *matches* a send if certain arguments to the calls match.
	- Source, tag, and communicator are matched.
	- If the datatypes and count don't match, this could lead to memory errors and correctness issues.
- If a sender sends two messages to a destination, and both match the same receive, the second message cannot be received if the first is still pending.
	- "No-overtaking" messages. They are queued.
	- Always true when processes are single-threaded.
- Tags can be used to disambiguate between messages in case of non-determinism.
	- Additional layer of identification to ensure compliance.

## Simple Send/Receive in MPI
```cpp
int main*(int argc, char *argc[]) {
	...
	MPI_Comm_rank(MPI_COMM_WORLD, &myrank);

	int data;
	if (myrank == 0) {
		data = 7;
		MPI_Send(&data, 1, MPI_INT, 1, 0, MPI_COMM_WORLD);
	} else if (myrank == 1) {
		MPI_Recv(&data, 1, MPI_INT, 0, 0, MPI_COMM_WORLD,
				MPI_STATUS_IGNORE);
		printf("Process 1 received data &d from process 0\n", data);
	}
	...
}
```

## Basic MPI_Send and MPI_Recv
- MPI_Send and MPI_Recv routines are blocking.
	- Only return when the buffer specified in the call can be used again.
	- Send: Returns once sender can reuse the buffer.
	- Recv: Returns once data from Recv is available in the buffer.

### Example Program
```cpp
int main (int argc, char *argv[]) {
 ...
 MPI_Comm_rank(MPI_COMM_WORLD, &myrank);
 ...
 if (myrank % 2 == 0) {
	 data = myrank;
	 MPI_Send(&data, 1, MPI_INT, myrank+1, 0, ...);
 } else {
	 data = myrank * 2;
	 MPI_Recv(&data, 1, MPI_INT, myrank-1, 0, ...);
	 ...
	 printf("Process %d received data %d\n", myrank, data);
 }
 ...
}
```
![[Pasted image 20240919151853.png]]
## MPI Communicators
- Communicator represents a group or set of processes numbered 0, ..., n-1.
	- Identified by a unique "rag" assigned by the runtime.
- Every program starts with `MPI_COMM_WORLD` (this is the default communicator).
	- Defined by the MPI runtime, this group includes all processes.
- There are several MPI routines that creates sub-communicators:
	- `MPI_Comm_split`
	- `MPI_Cart_create`
	- `MPI_Group_incl`
## MPI Datatypes
- Can be a pre-defined one: `MPI_INT`, `MPI_CHAR`, `MPI_DOUBLE`, ....
- Derived or user-defined datatypes:
	- Array of elements of another datatype.
	- Struct datatype to accommodate sending multiple datatypes together.