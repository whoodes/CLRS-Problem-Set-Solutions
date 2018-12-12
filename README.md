# CLRS Problem Set Solutions

## Table of Contents
* [Problem Set 01](#problem-set-01)
  * [Linear Search](#linear-search)
  * [Binary Search](#binary-search)
  
This is a collection of course work, mostly consisting of problems taken from the CLRS MIT Press textbook.  The algorithms
course was completed in December of 2018.  This repository is a demonstration of my own personal understanding of the
material covered.  There are a total of eleven problem sets in all.


## Problem set 01

A collection of loop invariant proofs, as well as a recurrence tree proof.  The algorithms involved in this problem set include:

- linear search
- binary search
- bubble sort

### Linear Search

```
LinearSearch(A, n, v)
  for i = 0 to n - 1
    if A[i] == v
      return i
  return NIL
```

**Loop invariant:** 

At the start of each iteration of the for loop, the sub-array A'[*0..i - 1*] does not contain the value *v*.

Initialization: 

When the for loop is read, *i = 0*, thus the sub-array A' is empty.  Therefore the invariant is vacuously true.

**Maintenance:** 

With each iteration, assume A'[0..*i* - 1] does not contain *v*, if A[ *i* ] == *v*, *i* is returned.  Otherwise, *i = i +
1*.  So, we have... 

*i - 1 = (i - 1) + 1*

*i - 1 = i*

Therefore the invariant is maintained.

**Termination:**

if *i* = *n* - 1 and A[ *i* ] != *v*, then *i* = *n* and the loop exits.  Because the invariant was maintained, the sub-array 
A'[*0..i - 1*] does not contain *v*, but *i* = *n*.  So, we have...

*i - 1 = n - 1*

Therefore A[*0..n - 1*] does not contain *v*.

QED

### Binary Search

A recurrence relation can be defined to represent binary search as follows...

*T(n) = aT(n / b) + D(n) + C(n)*

Assume *T(1) = &#920;(1) = c*

Let *D(n)* be the cost to divide and *C(n)* be the cost to compare.

*D(n) = &#8970;(low + high) / 2&#8971; = &#920;(1) = c*

Where *low* and *high* are the respective indices corresponding to the sub-arrays within each level of recursion.

It is easy to see *C(n) = &#920;(1)*, as only a single key comparison occurs in the array A<sub>k</sub> on the *kth* call
to *T(n)*.

For the values of *a* and *b*, binary search divides the problem in half, working on only one of the resulting halves. This gives us...

*a = 1 and b = 2*

So, putting this all together we have...

*T(n) = T(n / 2) + &#920;(1) + &#920;(1)

*= T(n / 2) + &#920;(1)*

*= T(n / 2) + c* where *T(1) = c*




