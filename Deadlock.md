##### Wikipedia
In [concurrent computing](https://en.wikipedia.org/wiki/Concurrent_computing "Concurrent computing"), **deadlock** is any situation in which no member of some group of entities can proceed because each waits for another member, including itself, to take action, such as sending a message or, more commonly, releasing a [lock](https://en.wikipedia.org/wiki/Lock_(computer_science) "Lock (computer science)").[[1]
### Formal definition
$\exists$ a cycle in a thread-lock graph or state diagram.
![[Pasted image 20240613144021.png]]
### Freedom
Freedom of Deadlock = The system makes progress as a whole
-> weaker condition than...
Freedom of [[Starvation]] = All threads make progress. 
e.g.: A mutual exclusion algorithm that must **choose** to allow **one of two processes** into a [critical section](https://en.wikipedia.org/wiki/Critical_section "Critical section") (cf. [[Behind Locks]]) and picks one **arbitrarily** is **deadlock-free, but not starvation-free.**

