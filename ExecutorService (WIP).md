#### Helpful web resource
https://www.baeldung.com/java-executor-service-tutorial
#### General comparison to [[ForkJoin]]
The **ForkJoin framework** uses a work-stealing thread pool for computation, which delegates ForkJoinTasks to the threads. **ExecutorServices** generally also maintain a thread pool, though (very) strictly speaking they are not required to do so by the interface.

#WIP #TODO
- `implements Runnable`
- `extends Thread`
- `wait()`
- `notifyAll()`
alerts all waiting threads.
- `notify()`
alerts a random waiting thread.
