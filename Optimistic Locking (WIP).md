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
[[Locking Strategies (WIP)]]