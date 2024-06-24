...featuring
- **flag**[] - **AtomicBoolean[]** (? - #TODO) 
- **victim** - **AtomicInteger**

##### (wrong) implementation example from Lecture {number} #TODO :
![[Pasted image 20240513180936.png]]
code incorrect tho because the entire boolean array is marked as volatile, whereas the correct approach would be to make the elements **AtomicIntegers** and the array an **AtomicIntegerArray**

#TODO *: why AtomicIntegerArray and not AtomicBoolean[]? (internet suggests the latter is faster) Also, find the proper implementation from lecture*

*From TA slides:*
![[Peterson's Lock - TA slides.png]]


#TODO
MRMW access requirement.