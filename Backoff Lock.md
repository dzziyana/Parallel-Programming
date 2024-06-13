*Lecture 17 pt. 1?*
# Idea
Threads go to sleep with #random duration and the duration increases each time the resource is not free.
```
wait_time =
min (MAX_WAIT, wait_time * 2)
```

# Implementation
![[Pasted image 20240518145949.png]]
- the else block **goes to sleep for a while** - that's all what it does

That's what the going to sleep part (backoff.backoff()) should look like:
![[Pasted image 20240518150349.png]]