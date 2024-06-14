#### Freedom of starvation = Every thread that tries to make progress, eventually makes progress
... Freedom of starvation is a stronger guarantee than the absence of deadlock: A mutual exclusion algorithm that must **choose** to allow **one of two processes** into a [critical section](https://en.wikipedia.org/wiki/Critical_section "Critical section") (cf. [[Behind Locks]]) and picks one **arbitrarily** is **deadlock-free, but not starvation-free.**

*As opposed to:*
Freedom of [[Deadlock]] = The system makes progress as a whole