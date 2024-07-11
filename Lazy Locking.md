#Slides-Lecture-19 
## Lazy List
- Works similar to an Optimistic List (see [[Optimistic Locking]]), but scans only once.
- `contains()` is #wait-free!
- Additional concept: "to logically remove an object"

> [!Basic Idea]
> Leave cleanup for later, mark nodes as deleted. 
> 
> **Invariant**: Every *unmarked* node is reachable.

![[LinkedListLazyRemove.png]]
![[LinkedListLazyRemove2.png]]
![[LinkedListLazyContains.png]]

## Lazy Skip List
A **practical concurrent set implementation**
### Interface
 + `add`
+ `remove`
+ `find`

### Assumptions
+ many calls to `find`
+ fewer calls to `add`
+ even fewer calls to `remove`

### Motivation
While we would typically use balanced trees in a non thread-safe context, it is very hard to implement an efficient, thread-safe tree structure.

Skip Lists solve the problem **probabilistically**.

### Implementation
**Sorted Multi-level list**

Each node has a **height** that is probabilistic. 
$$\mathbb{P}(height = n) = 0.5^n$$
![[SkipListSimple.png]]

#### Searching / `find`
![[SkipListFind2.png]]
![[SkipListFind1.png]]
## Adding / `add`
![[SkipListAdd.png]]

### Removing / `remove`

![[SkipListRemove.png]]
### Contains / `contains`

>[!Important]
>`contains` is **[[Progress Assumptions and Progress Conditions#Wait-free|wait free!]]**

![[SkipListContains.png]]