# Stack
## valid methods
![[Pasted image 20240612123846.png]]
# Queue
- only present as an interface in Java
- most common implementing classes are PriorityQueue and LinkedList, but those are not threads-safe
## valid methods


- add(element): Adds an element to the rear of the queue. If the queue is full, it throws an exception.

- offer(element): Adds an element to the rear of the queue. If the queue is full, it returns false.

- remove(): Removes and returns the element at the front of the queue. If the queue is empty, it throws an exception.

- poll(): Removes and returns the element at the front of the queue. If the queue is empty, it returns null.

- element(): Returns the element at the front of the queue without removing it. If the queue is empty, it throws an exception.

- peek(): Returns the element at the front of the queue without removing it. If the queue is empty, it returns null.

# [[Skip List (WIP)]]