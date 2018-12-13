# CLRS Problem Set Solutions

## Table of Contents
* [Problem Set 01](#problem-set-01)
  * [Linear Search](#linear-search)
  * [Binary Search](#binary-search)
  * [Bubble Sort](#bubble-sort)
* [Problem Set 02](#problem-set-02)
  
This is a collection of course work, mostly consisting of problems taken from the CLRS MIT Press textbook.  The algorithms
course was completed in December of 2018.  This repository is a demonstration of my own personal understanding of the
material covered.  There are a total of eleven problem sets in all.

## Problem set 01

A collection of loop invariant proofs, as well as a recurrence tree proof.  The algorithms involved in this problem set
include:

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

```
BinarySearch(x, A, min, max)
  low = min
  high = max
  while low <= high
    mid = floor( (low + high) / 2 )
    if x < A[mid]
      high = mid - 1
    else if x > A[mid]
      high = mid + 1
    else
      return mid
  return NIL
```

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

*T(n) = T(n / 2) + &#920;(1) + &#920;(1)*

*= T(n / 2) + &#920;(1)*

*= T(n / 2) + c* 

Where *c = T(1)*

If defined as a recurrence tree, binary search could be thought of as a path from the root to a single leaf, as only one edge
per level is traversed.

Or, *c + c + ... + c*

*= c(1 + 1 + ... + 1)*

**Claim:** *c(1 + 1 + ... + 1) = c(log<sub>2</sub>(n) + 1)*

**Proof:**

Base case: *n = 1*

Hypothesis: *The levels of a tree with 2<sup>i</sup> leaves = *log<sub>2</sub>(2<sup>i</sup>) + 1 = i + 1*

Let *n = 2<sup>i + 1</sup>*

*log<sub>2</sub>(2<sup>i + 1</sup>) + 1 = i + 1 + 1*

*(i + 1)log<sub>2</sub>2 + 1 = i + 2*

*i + 2 = i + 2*

**Therefore** in the worst case, binary search has a constant cost, *c*, *log<sub>2</sub>(n) + 1* times, or...

*c(log<sub>2</sub>(n) + 1)*

*= clog<sub>2</sub>(n) + c*

*= &#920;(log<sub>2</sub>(n))*

**QED**

### Bubble Sort

One of the main benefits of bubble sort is early termination if no 'bubbling' occurs.  In this example, no such consideration
is taken.

```
BubbleSort(A)
  for i = 1 to A.length - 1
    for j = A.length downto i + 1
      if A[j] < A[j - 1]
        exchange A[j] with A[j - 1]
```

This implementation also takes a 'bubble down' approach.  Our proof will attack the inner loop first, working our way out.

**Loop Invariant:**

At the start of each iteration of the inner loop, the element A[ *j* ] &#8804; A[*j + 1*].

**Initialization:**

When the loop begins, *j* = A.length, because no element exists at A[*j + 1*] the invariant is vacuously true.

**Maintenance:**

With each successive iteration, if A[ *j* ] &#60; A[*j - 1*] then the values are exchanged.  Because *j* is decremented,
*j* = *j - 1*.  Therefore the exchange results in A[ *j* ] &#8804; A[*j + 1*] by susbstituting the values of *j*.

**Termination:**

When *j* = *i + 1*, the comparison taking place occurs between *j* and *i* because *i* = *j - 1*.  When *j* is decremented
once more, the loop exits with *j* = *i*.  The invariant was maintained, thus by substitution...

A[ *i* ] &#8804; A[*i + 1*]

Let us now use this result to prove correctness of the outer loop...

**Loop Invariant:**

The Array A' consists of the elements A[*1..i*] such that A'[ 1 ] &#8804; A'[ 2 ] &#8804; ... &#8804; A'[ *i* ]

**Initialization:**

The loop begins with *i* = 1, thus the invariant vacuously holds for an array of size one.

**Maintenance:**

With each iteration of the outer loop, we showed that termination of the inner loop results in A[ *i* ] &#8804; A[*i + 1*].
Each pass then increments *i*, resulting in *i* = *i + 1*, but this means A[*i - 1] &#8804; A[ *i* ].  Therefore A' is
maintained.

**Termination:**

When *i* = A.length the loop exits.  Maintenance shows that A'[ 1 ] &#8804; A'[ 2 ] &#8804; ... &#8804; A'[ *i* ], but 
*i* = A.length.  Therefor A'[1..A.length] is the output.

QED

**Analysis:**

In the worst case, the inner loop iterates *n - (i + 1) + 1* or *n - i* times in relation to the outer loop.

So,

*i = 1 : j = n - 1*

*i = 2 : j = n - 2*

.
.
.

*i = n - 1 : j = 1*

Which is equivalent to...

*(n - 1) + (n - 2) + (n - 3) + ... + 1*

= *n(n - 1) / 2*

= *(n<sup>2</sup> - n) / 2*

= ***O**(n<sup>2</sup>)*

In our implementaiton we can make this a tight bound of &#920;(n<sup>2</sup>) because no optimization takes place.

## Problem Set 02

A collection of asymptotic proofs, in regard to defining a handful of functions into sets of **O**, &#920;, &#937;,
as well as **o**, &#952;, &#969;.  We will also take a brief look into binary trees, along with an anaylsis of a
merge-insertion sort hybrid algorithm.

