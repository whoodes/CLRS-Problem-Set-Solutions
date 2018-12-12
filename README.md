# CLRS Problem Set Solutions

## Table of Contents
* [Problem Set 01](#problem-set-01)

### Problem set 01

#### Linear Search

```
LinearSearch(A, n, v)
  for i = 0 to n - 1
    if A[i] == v
      return i
  return NIL
```

Loop invariant: 
At the start of each iteration of the for loop, the sub-array A'[0..i - 1] does not contain the value *v*.

Initialization: 
When the for loop is read, *i* = 0, thus the sub-array A' is empty.  Therefore the invariant is vacuously true.

Maintenance:  
With each iteration, assume A'[0..i - 1] does not contain *v*, if A[i] == v, *i* is returned.  Otherwise, *i* = *i* + 1.  So, we have... 

*i* - 1 = (*i* - 1) + 1
*i* - 1 = i

Therefore the invariant is maintained.
