https://www.baeldung.com/java-fork-join
#Slides-Lecture-10-11 
## Important Algorithms / subroutines to know
- See [[Simple parallel programs]]: Prefix sum
- [[Reduction Operation]]
- [[Map Operation]]

For all of these: **Call `.compute()` after `.fork()`!**
### Important facts when implementing using the library

**Generally a cutoff value of 100-5000 basic operations is recommended!**
This was a question in an old exam.

#### General comparison to [[ExecutorService (WIP)]]
- uses a *pool* of threads

![[Screenshot 2024-06-12 at 13.52.58.png]]



#### **work stealing** organized by thread pool
#FIXME: **may not be relevant**
In a *work stealing* scheduler, each processor in a computer system has a queue of work items (threads) to perform. Each work item consists of a series of instructions, to be executed sequentially, but in the course of its execution, a work item may also _spawn_ new work items that can feasibly be executed in parallel with its other work. These new items are initially put on the queue of the processor executing the work item. When a processor runs out of work, it looks at the queues of the other processors and "steals" their work items. In effect, work stealing distributes the scheduling work over idle processors, and as long as all processors have work to do, no scheduling overhead occurs.

Work stealing contrasts with _work sharing_, another popular scheduling approach for dynamic multithreading, where each work item is scheduled onto a processor when it is spawned. Compared to this approach, work stealing reduces the amount of [process migration](https://en.wikipedia.org/wiki/Process_migration "Process migration")between processors, because no such migration occurs when all processors have work to do.
### RecursiveTasks and RecursiveAction
Tasks : haben ein return value
Action : void (map operations!)

## Fork Join Program as DAG
![[ForkJoinAsDAG.png]]
**Simple Example**: (most of our algorithms look similar)
![[ForkJoinGraph.png]]
### Performance 
![[ForkJoinDAGTp.png]]

See [[Basic formulas]]

The **ForkJoin** library gives an **expected-time garantuee** that is asymptotically optimal!

So
$$T_p = O((T_1 / p) + T_\infty)$$
# Pool
![[Pasted image 20240612140235.png]]

# methods
- **.compute()** - äquivalent zu Thread.run() (d.h. führt lediglich sequentiell den Code im selben Thread aus) aber kann im Gegensatz zu .run() auch einen Rückgabewert haben
- **.fork()** - neuer Thread im Pool
- **.join()** - falls compute() ein return value hat, wird dieser ebenfalls returned
- **fjPool.invoke(new fjTask(...))** submits a task and waits until completed
- **fjPool.submit()** submits a task, receiving a Future