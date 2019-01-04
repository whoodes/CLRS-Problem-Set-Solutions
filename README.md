# CLRS Problem Set Solutions

## Table of Contents
* [Problem Set 01](#problem-set-01)
  * [Linear Search](#linear-search)
  * [Binary Search](#binary-search)
  * [Bubble Sort](#bubble-sort)
* [Problem Set 02](#problem-set-02)
  * [Asymptotic Analysis](#asymptotic-analysis)
  * [Print Binary Tree](#print-binary-tree)
  * [Merge-Insertion Sort](#merge-insertion-sort)
  
This is a collection of course work, mostly consisting of problems taken from the CLRS MIT Press textbook.  The algorithms
course was completed in December of 2018.  This repository is a demonstration of my own personal understanding of the
material covered.  There are a total of eleven problem sets in all.

**Note to reader: Problem sets are currently in the process of being transmogrified into this GitHub page.**

## Problem set 01

[Back to top](#table-of-contents)

A collection of loop invariant proofs, as well as a recurrence tree proof.  The algorithms involved in this problem set
include:

- linear search
- binary search
- bubble sort

### Linear Search

```asp
LinearSearch(A, n, v)
  for i = 0 to n - 1
    if A[i] == v
      return i
  return NIL
```

**Loop invariant:** 

At the start of each iteration of the for loop, the sub-array A'[*0..i - 1*] does not contain the value *v*.

**Initialization:**

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

```asp
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

Hypothesis: *The levels of a tree with 2<sup>i</sup> leaves = log<sub>2</sub>(2<sup>i</sup>) + 1 = i + 1*

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

```asp
BubbleSort(A)
  for i = 1 to A.length - 1
    for j = A.length downto i + 1
      if A[j] < A[j - 1]
        exchange A[j] with A[j - 1]
```

This implementation also takes a 'bubble down' approach.  Our proof will attack the inner loop first, working our way out.

**Loop Invariant:**

At the start of each iteration of the inner loop, the element A[ *j* ] &#8804; A[ *j + 1* ].

**Initialization:**

When the loop begins, *j* = A.length, because no element exists at A[ *j + 1* ] the invariant is vacuously true.

**Maintenance:**

With each successive iteration, if A[ *j* ] &#60; A[ *j - 1* ] then the values are exchanged.  Because *j* is decremented,
*j* = *j - 1*.  Therefore the exchange results in A[ *j* ] &#8804; A[ *j + 1* ] by susbstituting the values of *j*.

**Termination:**

When *j* = *i + 1*, the comparison taking place occurs between *j* and *i* because *i* = *j - 1*.  When *j* is decremented
once more, the loop exits with *j* = *i*.  The invariant was maintained, thus by substitution...

A[ *i* ] &#8804; A[ *i + 1* ]

Let us now use this result to prove correctness of the outer loop...

**Loop Invariant:**

The Array A' consists of the elements A[ *1..i* ] such that A'[ 1 ] &#8804; A'[ 2 ] &#8804; ... &#8804; A'[ *i* ]

**Initialization:**

The loop begins with *i* = 1, thus the invariant vacuously holds for an array of size one.

**Maintenance:**

With each iteration of the outer loop, we showed that termination of the inner loop results in A[ *i* ] &#8804; A[ *i + 1* ].
Each pass then increments *i*, resulting in *i* = *i + 1*, but this means A[ *i - 1* ] &#8804; A[ *i* ].  Therefore A' is
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

In our implementation we can make this a tight bound of &#920;(n<sup>2</sup>) because no optimization takes place.

## Problem Set 02

[Back to top](#table-of-contents)

A collection of asymptotic proofs, in regard to defining a handful of functions into sets of **O**, &#920;, &#937;,
as well as **o**, &#952;, &#969;.  We will also take a brief look into binary trees, along with an analysis of a
merge-insertion sort hybrid algorithm.

### Asymptotic Analysis

|      f(n)       |       g(n)        |  O    |   o   |   &#937;   |   &#969;   |   &#920;   |
| :-------------: | :-------------:   | :---: | :---: | :--------: | :--------: | :--------: |
| 4n<sup>2</sup>  | 4<sup>lgn</sup>   | 1     |   0   |   1        |   0        |   1        |
| 2<sup>lgn</sup> | lg<sup>2</sup>n   | 0     |   0   |   1        |   1        |   0        |
| n<sup>1/2</sup> | n<sup>sin n</sup> | 0     |   0   |   0        |   0        |   0        |


The following simple mathematical proofs will serve as confirmation for the above table.
For every proof, we assume  *c* and *n* &#8712; R, and that each holds 
&#8704;n | n 	&#8805; n<sub>0</sub> with n<sub>0</sub> &#8805; 0.

#### Row 1

**Big-O / Little-o**

Let *f(n) = 4n<sup>2</sup> and g(n) = 4<sup>lgn</sup>*

Assume *f(n) = O( g(n) )*

Then 0 &#8804; *f(n)* &#8804; *cg(n)*

4n<sup>2</sup> &#8804; c4<sup>lgn</sup>

&#8804; c(2<sup>2</sup>)<sup>lgn</sup>

&#8804; c2<sup>2lgn</sup>

&#8804; c2<sup>lgn<sup>2</sup></sup>

&#8804; cn<sup>2</sup>

So,

0 &#8804; 4n<sup>2</sup> &#8804; cn<sup>2</sup>

Let *c* = 4 and *n<sub>0</sub>* = 1

&#8756; *f(n) = O( g(n) )*

We can easily show that little-o does not apply here as Big-O was proved by showing equality 
of *f* and *g*.

&#8756; *f(n) &#8800; o( g(n) )*

**Big-Omega / Little-omega**

Assume *f(n)* = &#937;( g(n) )

Then 0 &#8804; *cg(n)* &#8804; *f(n)*

We can easily show this by the equality shown in the Big-O proof.

&#8756; *f(n) = &#937;( g(n) )*

Similarly to little-o, the equality of *f* and *g* proves *f(n) &#8800; &#969;( g(n) )*

**Big-Theta**

Assume *f(n) = &#920;( g(n) )*

Then 0 &#8804; *c<sub>1</sub>g(n)* &#8804; *f(n)* &#8804; *c<sub>2</sub>g(n)*

Let  *c<sub>1</sub>* = *c<sub>2</sub>* = 4 and *n<sub>0</sub>* = 1

We have, 0 &#8804; *4n<sup>2</sup>* &#8804; *4n<sup>2</sup>* &#8804; *4n<sup>2</sup>*

&#8756; *f(n) = &#920;( g(n) )*

QED

#### Row 2

**Big-O / little-o**

Let *f(n) = 2<sup>lgn</sup> and g(n) = lg<sup>2</sup>n*

Assume *f(n)* = *O( g(n) )*

Then 0 &#8804; *2<sup>lgn</sup>* &#8804; *clg<sup>2</sup>n*

2<sup>lgn</sup> = n

lg( 2<sup>lgn</sup> ) = lg(n)

lg(n) * lg(2) = lg(n)

lg(n) = lg(n)

n = n

So we have,

0 &#8804; *n* &#8804; *clg<sup>2</sup>n*

But any linear function will grow faster than any logarithmic function given large
enough values of *n*.

&#8756; *f(n)* &#8800; *O( g(n) )*

If *f(n)* &#8800; *O( g(n) )* then it is certainly not *o( g(n) )*.

That is, *f(n)* &#8805; *cg(n)* where *c* = 1 and &#8704;*n* : *n* &#8805; 16

&#8756; *f(n)* &#8800; *o( g(n) )*

**Big-Omega / little-omega**

Assume *f(n)* = &#937;( g(n) )

Let *c* = 1 and *n<sub>0</sub>* = 16

Then 0 &#8804; *lg<sup>2</sup>n* &#8804; *n* holds true &#8704;*n* &#8805; 16

&#8756; *f(n)* = &#937;( g(n) )

**Big-Theta**

Because *f(n)* &#8800; O( *g(n)* ), *f(n)* cannot be tightly bounded by *g(n)*.

&#8756; *f(n)* &#8800; &#920;( *g(n)* )

QED

#### Row 3

The functions, *&#8730;n* and *n<sup>sin(n)</sup>*, are not asymptotically comparable because
the exponent of the sinusoidal function *n<sup>sin(n)</sup>* oscillates between the values 1 and -1. 

### Print Binary Tree
[Back to top](#table-of-contents)

Below is a non-recursive pre-order traversal of a binary tree, written in Java.  The traversal
prints each node using a stack as an auxiliary data structure. We assume the tree consists of
objects of the class *TreeNode*.  We also assume the *toString* method has been overridden to
print that data from the node in a desirable format.

```java
public class BinaryTree {
  
  // Assume TreeNode class, member variables and methods
  
  private void PrintBinaryTreeNodes(TreeNode root) { 
    if (root == null) {
      return;
    }   
    
    Stack<TreeNode> treeStack = new Stack<>();
    treeStack.push(root);
       
    while (!stack.isEmpty()) {
      TreeNode node = treeStack.pop();
      node.toString();
         
      if (node.right != null) {
        treeStack.push(node.right);
      }
         
      if (node.left != null) {
        treeStack.push(node.left);
      }
    }
  }
}
```
