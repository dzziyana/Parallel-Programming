##### Wikipedia
In [concurrent computing](https://en.wikipedia.org/wiki/Concurrent_computing "Concurrent computing"), **deadlock** is any situation in which no member of some group of entities can proceed because each waits for another member, including itself, to take action, such as sending a message or, more commonly, releasing a [lock](https://en.wikipedia.org/wiki/Lock_(computer_science) "Lock (computer science)").[[1]

*from Lecture 17 pt 2* :
>[! Formal definition]
> Two or more processes are **mutually blocked** because each process waits for another of these processes to proceed.
#### $\exists$ a cycle in a [thread-lock directed graph] or state diagram.
![[Deadlock illustration.png]]
***Legend:**
 Red: $T_n \rightarrow R_n$ = thread $T_n$ **attempts to acquire** resource (lock) $R_n$
Green: $R_n \rightarrow T_n$ = resource (lock) $R_n$ **is held** by thread $T_n$

## Livelock
>[!Definition]
  Competing processes are able to **detect a potential deadlock** but make **no observable progress** while trying to resolve it

> Deadlocks can, in general, not be healed. Releasing locks generally leads to inconsistent state.
> -> ***Avoidance by design*** is the only option

Some notable examples of nuanced deadlock situations is the **account-transfer problem** as well as the historic example of the .append() method in the StringBuffer class of Java standard library (They ended up creating the StringBuilder class because of that)
### Avoidance techniques 
#### Two-phase locking with retry (*release when failed*)
usually in databases where transactions can be aborted without consequences (modifications of local state)
#### Non-overlapping (smaller) CSs
#### Global Lock
very easy and safe, but extremely inefficient
#### Global Ordering of resources
usually in parallel programming where global state is modified
>**Trick**
> *No globally unique order available? Generate it!*
> Just like with tickets in the [[Bakery Lock - fair filter]], except they get assigned at creation of resource
##### Easy possibility: Fixed order among types
Example: *"When moving an item from hashtable to work queue, never try to acquire the queue lock while holding the hashtable lock"*
### Freedom
Freedom of Deadlock = The system makes progress as a whole
-> weaker condition than...
Freedom of [[Starvation]] = All threads make progress. 
e.g.: A mutual exclusion algorithm that must **choose** to allow **one of two processes** into a [critical section](https://en.wikipedia.org/wiki/Critical_section "Critical section") (cf. [[Behind Locks]]) and picks one **arbitrarily** is **deadlock-free, but not starvation-free.**


