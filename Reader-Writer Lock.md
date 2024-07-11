#Slides-Lecture-19 
- Two locks : one for read, one for write access
- As long as memory location is immutable, it is usually safe to read concurrently
- The read lock can be held by multiple threads at once as long as no thread holds the write lock
> [!Definition]
> A **Reader/Writer** lock states fall into these cathegories
> + `not held`
> + `held for writing`
> + `held for reading`
>
> With the following **invariants**:
> + $0 \le \text{writers} \le 1$
> + $0 \le \text{readers}$
> + $\text{writers} \cdot \text{readers} = 0$

![[MethodsRWLock.png]]

> [!Warning]
> The default java implementation has neither *Reader*- nor *Writer*-priority!
## Monitor based implementation  (Reader priority)
![[RWLockMonitor.png]]
## Monitor based implementation  (Writer priority)
![[RWLockMonitorWriter.png]]

## Monitor based implementation (after $k$ readers, writer)
introduces an additional variable `writersWait` to keep track of if readers still have priority, which is set dynamically depending on how many readers wait at a time.s
![[RWLockMonitorAfterK.png]]

## code example
source: https://www.youtube.com/watch?v=ddUSe3A9MMg
![[Pasted image 20240614132251.png]]