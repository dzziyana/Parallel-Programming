#Slides-Lecture-19 
> [!Definition]
> assumes that **contention is unlikely**, i.e. it will attempt to get the data without locks, and then check if any other thread has interrupted this. If not, proceed, else start over.
- CAS & TAS
- **Optimistic list** (without CAS/TAS) :
	- 1) Find all nodes involved (without locking)
	- 2) Make change iff everything is fine 

*Assignment 9: From exercise / TA slides*
![[Optimism vs Pessimism - exercise code.png]]

![[Optimistic Locking code example 1.png]]
## LinkedList example
![[Optimistic Locking - LinkedList add.png]]
![[Optimistic Locking - LinkedList remove.png]]

### `validate(Node pred, Node curr)` method
![[Optimistic Locking - LinkedList validate.png]]

### Good
+ No contention on traversals
+ Traversals are wait-free (see [[Progress Assumptions and Progress Conditions#Wait-free]])
+ less lock acquisitions
### Bad
+ need to always traverse twice
+ `contains()` needs to acquire locks
+ not starvation free
	+ The validation could always fail if another thread repeatedly inserts/removes the successor node of the locked node