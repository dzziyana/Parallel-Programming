[[Maps and Reduces]]
#Reductions - only work with **associative operations**
#Maps - each element of a data structure gets **processed individually**

## #prefixSum

0. Creates tree - **NEITHER** a map nor a reduce operation)
1. **UP**-pass:  builds the tree bottom up (in parallel) - **REDUCE**
i.e. subdivides the input array into appropriate ranges, **sums leaves** and then propagates to top
3. **DOWN**-pass: traverses the tree top-down - **NEITHER**
i.e. assigning the ***fromLeft*** parameter for every node
```
root.from_left = 0
...
from_left = parent.from_left + left.sum;
```
4. Leaves write to **OUTPUT** - **MAP** but with time complexity of reduce:
```
output = input + from_left
```
=> #parallelism is $\frac{n}{\log n}$
### details :
![[Pasted image 20240430124706.png]]
![[Pasted image 20240430124823.png]]
![[Pasted image 20240430124840.png]]
The addition of a #SequentialCutoff :
- **UP**: leaf nodes shall hold the sum of a range
- **DOWN**:
```
output[lo] = fromLeft + input[lo];
for(i = lo+1; i < hi; i++)
	output[i]=output[i-1] + input[i]
```
## applicable as a #pattern to a myriad of problems such as 
- **Min/Max** of all elements **to the left of i**
- **existence** of an element satisfying some property **to the left of i**
- **count** **of el-s to the left of i** satisfying some property
## #Pack
Given an array, produce an output array containing only elements such that f(el) is true
example: input 
-> can be reduced to a prefix problem:
1. parallel #map to compute a bit-vector for true elements
```
bits [1, 0, 0, 0, 1, 0, 1, 1, 0, 1]
```
2. parallel #prefixSum on the bit-vector
```
bitsum [1, 1, 1, 1, 2, 2, 3, 4, 4, 5]
```
3. parallel map with condition to produce the output
```
output = new array of size bitsum[n-1] 
FORALL (i=0; i < input.length; i++){
	if(bits[i]==1)
		output[bitsum[i]-1] = input[i];
}
```
=> O(n) work, O(logn) #span
### partitioning #QuickSort with #Pack :
- pack el-s less than pivot into the left side of the auxiliary array (aux)
- pack el-s greater than pivot into the right side of aux
- put pivot inbetween and sort recursively
=> $O(logn)$ span for partition, in total $O(log^{2}n$) due to the default $O(logn)$ parallelism
![[Pasted image 20240612153229.png]]![[Pasted image 20240612153403.png]]