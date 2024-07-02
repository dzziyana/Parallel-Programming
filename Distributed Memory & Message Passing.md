#Slides-Lecture-27 

Many of the problems of parallel/concurrent programming come from sharing state:
[[Locks]], [[Races|Race Conditions]], ...\

## Message passing
### **Isolated mutable state**
+ State is mutable, but not shared: Each thread/task has **its private state**
+ Tasks cooperate via **message passing**
![[IsolatedMutableState.png]]
#### Example: Counter
![[DMCounter.png]]
![[DMCounter2.png]]

#### Example (Bank Account)
![[DMBankAccount.png]]

### Techniques
**Programming Models**
+ CSP: Communicating Sequential Processes
+ Actor programming model - Go language
**Framework / library**
+ MPI (Message Passing Interface)

### Synchronous vs Asynchronous messages
**Synchronous**:
+ sender blocks **until message is received**
**Asynchronous**:
+ sender **does not block** (fire and forget)
+ placed into a buffer for receiver to get
## Example (Go program)
> [!Info]
> Go channels are synchronous by default

![[ChannelsGoExample.png]]
![[GoConcurrentPrimeSieve.png]]
## MPI (Message Passing Interface)
**Libraries**:
+ PVM (Parallel Virtual Machines) 1980s
+ MPI (Message Passing Interface) 1990s\
**MPI = Standard API**
+ Hides Software / Hardware details
+ portable, flexible
+ implemented as a library
### Process identification
+ Each group can have multiple colors (sometimes called context)
+ <font color="red">Group + color == comunicator</font> (its like a name for the group)
+ When an MPI application starts, the group of all processes is initially given a predefined name called <font color="red">MPI_COMM_WORLD</font>
	+ The same group can have many names, but simple programs do not have to worry about multiple names
+ A process is identified by a unique number within each communicator, called <font color="red">rank</font>
	+ For two different communicators, the same process can have two different ranks: thus, the meaning of a “rank” is only defined when you specify the communicator
### MPI Communicators
![[MPICommunicators.png]]![[MPICommunicators2.png]]
### Process Ranks
Processes are identified by nonnegative integers, called *ranks*

$p$ processes are numbered $0, 1, 2, \dots, p-1$
```java
	public static void main(String args []) throws Exception {
	MPI.Init(args);
	// Get total number of processes (p)
	int size = MPI.COMM_WORLD.Size();
	// Get rank of current process (in [0..p-1])
	int rank = MPI.COMM_WORLD.Rank();
	MPI.Finalize();
}
```

### SPMD (Single Program Multiple Data)

![[SPMD.png]]
### Communication

![[MPICommunication.png]]

### Example: Parallel sort
![[MPIParallelSort.png]]