The Filter Lock is an extension of the [[Peterson Lock]] from 2 to $n$ Threads.
- every thread knows its level in the filter level[t]
- last level is CS
- in order to enter CS, a thread has to eliminate all levels
- **for each level, Peterson's mechanism is used** to filter **at most one thread** (victim) which stays behind, if other threads are at a higher level
## Pseudocode
![[FilterLockPseudoCode.png]]
> [!Warning]
> `level` and `victim` both have to be `AtomicIntegerArray`s


Each level can be seen as a [[Peterson Lock]]
```java
	// i... current level
	// me... my thread id (in [0..n-1])
	level[me] = i; // equivalent to flag[P] = true in Peterson lock
				   //  signals that we are currently trying to aquire
				   //  the lock on this level
	victim[i] = me;// equivalent to victim = P, 
				   //  makes sure P has to wait
				   //  all other threads are allowed to continue
				   // because we have n levels, this ensures, that in the last 
				   // level, only one two threads remain. This is then a 
				   // regular peterson lock
				   
				   
	while(There exists other k: level[k] >= i && victim[i] == me);
	
```

## Java Implementation
 ![[FilterLockCode.png]]

## Problems
- The Filter lock is not fair #personalTODO , it is **not** first come, first serve
- It is very inefficient for large $n$: It uses $O(n)$ memory and has to check $O(n)$ levels for each try at acquiring the lock.

### Why is a filter lock not fair?
The Peterson locks in each level only ensure that a single Thread stays behind in the level, while all other advance. 
Let $P$ be the thread that is waiting on some level $L$, while there are other threads in higher levels. Assume that $P$ tried to acquire the lock before $Q_1$, $Q_2$
Let $Q_1, Q_2$ be threads that are one level below $P$, on level $L-1$ and are not the victim on this lower level.

Consider now the case, that all levels above $P$ become empty:

It could happen that $Q_1$, $Q_2$ enter level $L$ before $P$ checks if the higher levels are empty. Let $Q_2$ be the last to enter level $L$. $Q_2$ is now the new victim of level $L$. Since $Q_1$ and $P$ are not the victim of level $L$ anymore, they can advance to level $L + 1$. Assume $Q_1$ sets itself to the victim of level $L + 1$ before $P$. Now $P$ must wait while $Q_1$ can advance. This is against the **first come, first serve** principle, that fairness entails, since $P$ entered the 'doorway' before $Q_1$, but $Q_1$ was able to acquire the lock before $P$.[]()