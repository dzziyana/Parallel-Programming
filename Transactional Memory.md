#Slides-Lecture-26

*Literature reference:* Herlihy Chapter 18.1 - 18.2

**Motivation:**
+ **Lack of composability** with locks and other synchronization methods (example: multiple locks [[Locking Strategies#General]] -> Warning, remember the bank account example from the exercises)
+ Locks are  **pessimistic**, we always protect everything and pay for it
+ **Locking mechanism is hard-wired to the program**: To change the synchronization scheme, we need to change the whole program. We can not separate synchronization / rest of program.
+ programming with locks is difficult
+ lock-free programming even more so

**Goal**:
+ let synchronization be done by the system (hardware / software)

#TODO: Maybe move this to the respective sections of locking / CAS etc. (not sure if this would be better)
### Problems with [[Locks|locking]]
+ [[Deadlock|Deadlocks]] ![[ProblemsLocking1.png]]
+ **Convoying**: thread holding a resource $R$ is descheduled while other threads queue up waiting for $R$ ![[ProblemsLocking2.png]]
+ **Priority Inversion**: Lower priority thread holds a resource $R$  that a higher priority thread is waiting on ![[ProblemsLocking3.png]]
+ Association of locks and data **established by convention** (e.g. [[Monitors]])
  $\Longrightarrow$ the best you can do is reasonably document your code
### Problems with [[TAS and CAS|CAS]]
Take for example [[Lock Free Programming#Lock-free unbounded queue]]

The implementation is **very hard**. We had multiple wrong attempts!

Even in the finished version, we rely on having a half-finished enqueue that is visible to other operations!

Note how much simpler the could would have been with DCAS. 

## The idea
`atomic` blocks / transactions 
![[AtomicBlocks.png]]
**Difference to locks:** Locks enforce mutual exclusion, here we only specify: *"We want this to happen like it was executed atomically."* 
The **execution** differs

> [!Transactionan Memory]
> ### Usage
> + Programmer defines **atomic code sections**
> + Programmer is concerned with 
> 	+ **what** should be atomic
> 	+ **not how** this is done (left to the system)
> 	
> $\Longrightarrow$ declarative approach
> 
> ###  Semantics
> #### Atomicity
> **changes made by a transaction are**
> + made visible **atomically**
> + other threads preserve either the **inital state** or the **final state** but nothing inbetween
> 
> #### Isolation
> **Transactions run in isolation**
> + while a transaction is running, **effects from other threads are not observed**
> + **as if** the transaction takes a **snapshot** of the global state before operating on the snapshot
> #### Serializability
> ![[TMSerializability.png]]

### ACID
**Transactional memory** is heavily inspired by **databases**

It follows the **ACID** principle
+ Atomicity
+ Consistency (data remains in a consistent state)
+ Isolation (no mutual corruption of data)
+ *not important for TM*: Durability (e.g. transaction effects will survive power loss -> stored in disk)
## Benefits
+ simpler and less error-prone code
+ higher level (declarative) semantics
+ **composable**
+ **optimistic** by design (mutual exclusion not required)
+ (analogy to garbage collection:
  Dan Grossman. 2007. *"The transactional memory / garbage collection analogy"*. SIGPLAN Not. 42, 10 (October 2007), 695-706.)

## Problems
+ Not clear what the best semantics for transactions are
+ getting good performance can be challenging
+ I/O operations
  Can we perform I/O operations in a transaction?


## Implementation
**Naive idea**: Big lock around all atomic sections
+ gives nearly all properties, but is **not scalable**
	+ #TODO **Which property is missing?**
+ not done in practise (for obvious reasons)

**Good idea**: Keep track of operations performed by each transaction
+ ensures atomicity
+ concurrency control

### Handling of conflicts
+ Transactions **will be aborted** if necessary
+ Handled by a **Concurrency Control** (CC) mechanism

**Possible actions after abortion**:
+ retry transaction automatically
+ notify user

#### Conflict Example (basic)
![[TMConflictExample1.png]]
$TX_A$ worked with the value $a = 0$ which was **changed** by the committing of $TX_B$ while $TX_A$ was still running.
![[TMConflictExample2.png]]
#### Conflict Example (Bank Account)
![[TMConflictExampleBA1.png]]![[TMConflictExampleBA2.png]]
![[TMConflictExampleBA3.png]]#### Conflict Example (Exception)
![[TMConflictExampleException.png]]

## Design choices
### Software (STM)
+ In the (parallel programming) language
+ greater flexibility
+ good performance might be challenging
+ Examples: Haskell, Clojure, ...
### Hardware (HTM)
+ can be fast
+ but bounded resources
+ can often not handle big transactions
**Examples**
+ Intel Haswell -> first widely available implementation of TM on x86 (future unclear)![[IntelHaswellHTM.png]]

+ Sun (now Oracle) Rock -> was not released
+ Supercomputers (IBM's Blue Gene/Q) -> long retired
Hybrid TM (Hardware + Software)
+ research topic

### General
+ Implementations are still immature
+ Many approaches
+ **STM still actively developed**

### Isolation
**What happens when shared state accessed by a transaction is also
accessed outside of a transaction?**
Are the transactional guarantees [[Transactional Memory#ACID]] still maintained?

**Strong isolation:** Yes
+ easier to port existing code
+ difficult to implement, overhead
**Weak Isolation**: No

### Nesting
**What are the semantics of nested transactions?** (Important for composability)
+ Flat nesting
+ Closed nesting
+ Other approaches (e.g. open nesting)
#### Flattened nesting
![[FlattenedNesting.png]]
**inner aborts --> outer aborts**
**inner commits --> changes visible only if outer commits**
#### Closed nesting
Similar to **flattened nesting** but
+ **inner aborts $\huge\not\to$ outer aborts**

Inner transaction commits
+ change visible to outer commit
+ but not to other transactions
Outer transaction commits
+ changes of inner transactions become visible

### What is part of the transaction?
![[PartOfTheTransaction.png]]If all variables are protected, it is **easier to port existing code** but this is difficult to implement as you need to check every memory operation.

## Reference-based STMs
+ **Mutable state** is put into **special variables**
	+ can only be modified inside a transaction
+ Everything else: **immutable or not shared**
### `scala-stm`
**Java does not include STM support**
+ `scala-stm` is an STM library built for scala
+ Java interface

No compiler support for ensuring that **refs are only accessed inside a transaction**

Java  7 does not have lambdas -> each transaction defined as `Runnable`

#### Bank account with ScalaSTM
![[TMBankAccountScala.png]]![[TMBankAccountScalaRunnable.png]]
![[TMBankAccountScalaGetBalance.png]]
![[TMBankAccountTransfer.png]]
**What do we do when there are not enough funds for a money transfer?** We want to wait until it does, before completing the transfer.
+ **locks -> conditional variables**
+ **TM -> retry**
![[TMBankAccountTransferRetry.png]]

## Retry
**How does it work?**
Implementations need to **track what reads/writes a transaction performed**
to detect conflicts
+ Typically called &**read-/write-set** of a transaction
+ When retry is called, transaction aborts and will be retried when **any of the
variables that were read, change**
+ In our [[Transactional Memory#Bank account with ScalaSTM|our example]] when `a.balance` is updated, the transaction will be retried

## Simplest STM Implementation / Clock-based STM System
> [!Ingredients]
> **Threads that run transactions with thread states:**
> + active
> + aborted
> + committed
>   
> **Objects representing state stored in memory**:
> + methods like constructor, read, write
> + and **copy**

![[ClockBasedSTM.png]]
#### Atomic objects
**each transaction**: local **read- and write-set** holding all locally read and written objects.

**Transaction calls `read`
+ Object in write set? 
	+ Yes-> return this (new) version
	+ No -> object's timestamp $\leq$ transaction's birthdate
		+ Yes -> add to read set, return
		+ No -> abort (since the data has changed)
**Transaction calls `write`**
+ If object not in write set, create copy in write set

#### Transaction life time
![[TransactionLifeTime.png]]
![[TMClockSuccessfulCommit.png]]![[TMClockAbortedCommit.png]]

## Dining philosophers with Scala-STM
#Slides-Lecture-27
#TODO Link to Dining Philosophers
![[DiningPhilosophersScalaSTM.png]]
**Fork**
```java
private static class Fork {
	public final Ref.View<Boolean> inUse = STM.newRef(false);
}
```
**Philosophers**
```java
class PhilosopherThread extends Thread {
	private final int meals;
	private final Fork left;
	private final Fork right;
	public PhilosopherThread(Fork left, Fork right) {
		this.left = left;
		this.right = right;
	}
	public void run() {
		for (int m = 0; m < meals; m++) {
			// THINK
			pickUpBothForks();
			// EAT
			putDownForks();
		}
	}
	
	private void pickUpBothForks() {
		STM.atomic(new Runnable() { public void run() {
			if (left.inUse.get() || right.inUse.get())
				STM.retry();
			left.inUse.set(true);
			right.inUse.set(true);
		}});
	}
	private void putDownForks() {
		STM.atomic(new Runnable() { public void run() {
			left.inUse.set(false);
			right.inUse.set(false);
		}});
	}
}
```
