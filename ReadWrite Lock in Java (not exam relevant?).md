- Two locks : one for read, one for write access
- As long as memory location is immutable, it is usually safe to read concurrently
- The read lock can be held by multiple threads at once as long as no thread holds the write lock
## code example
source: https://www.youtube.com/watch?v=ddUSe3A9MMg
![[Pasted image 20240614132251.png]]