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

> [!Important]
> `notify()` pushes a thread into the `Blocked` state first, and proceeds to transform it into a `Runnable` only after the thread also acquires the monitor lock
> 

> [!NOTE]
> Es gibt einen Unterschied zwischen `notifyAll()` und einer for-Schleife, die für ein `numThreads` `notify()` ausführt : Das letztere gibt **keine Garantie**, dass alle Threads geweckt werden, denn es könnten zwischenzeitlich welche warten. Bei `notifyAll()` wird die Gesamtheit der Queue von Threads in den Runnable state gebracht. (Es hört nicht auf, bis die Queue leer ist) 
