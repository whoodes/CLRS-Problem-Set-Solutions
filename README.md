# CLRS Problem Set Solutions

## Table of Contents
* [Problem Set 01](#problem-set-01)
  * [Linear Search](#linear-search)
  * [Binary Search](#binary-search)
  * [Bubble Sort](#bubble-sort)
* [Problem Set 02](#problem-set-02)
  * [Asymptotic Analysis](#asymptotic-analysis)
  * [Print Binary Tree](#print-binary-tree)
  * [Merge Insertion Sort](#merge-insertion-sort)
* [Problem Set 03](#problem-set-03)
  * [Expected Length of a Coding Scheme](#expected-length-of-a-coding-scheme)
  * [Hashing with Chaining](#hashing-with-chaining)
  * [Skip Lists](#skip-lists)
* [Problem Set 04](#problem-set-04)
  * [Master Method](#master-method)
  * [Substitution](#substitution)
  * [More Fun with Binary Search Trees](#more-fun-with-binary-search-trees)
* [Problem Set 05](#problem-set-05)
  * [D-Ary Heaps](#d-ary-heaps)
  * [3 Way Quicksort](#3-way-quicksort)
* [Problem Set 06](#problem-set-06)
  * [Sorting Large Numbers](#sorting-large-numbers)
  * [Red Black Trees](#red-black-trees)
  
This is a collection of course work, mostly consisting of problems taken from the CLRS MIT Press
textbook.  The algorithms course was completed in December of 2018.  This repository is a 
demonstration of my own personal understanding of the material covered.  There are a total of 
eleven problem sets in all.  The first half deals mainly with a slightly more mathematically 
rigorous look into some classic data structures and algorithms.  Then we'll dive into a more 
exciting domain (graphs!) and explore more awesome concepts! 

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

**Analysis**

The algorithm works in **O**(n) time.  The while loop takes a TreeNode as the root and prints it.
Each subsequent node is pushed on the stack.  For a pre-order traversal, the left child is pushed
after the right, given the first-in last-out nature of a stack. So, every right sub-tree is appended
until all left sub-roots have been popped and printed.  The stack then maintains the order of 
visitation within the tree.  Thus, we have *n* nodes entering the stack, with each node being 
operated on in **O**(1) time.

&#8756; PrintBinaryTreeNodes operates in **O**(n) time.

### Merge Insertion Sort
[Back to Top](#table-of-contents)

The idea for a hybridization of merge sort and insertion sort depends on the fact that, given
a small enough list size, insertion sort outperforms merge sort.  Insertion sort can also sort
in-place, giving it an advantage when implemented on most machines.  The idea then is to coarsen 
the leaves of the merge sort recursion tree by calling insertion sort when the list size becomes
sufficiently small.  We will show the time-complexity in two parts, then find a bound for values
 of *k*...

##### Claim: 
Insertion sort can sort *n* / *k* sub-lists, each of length *k*, in &#920;(*nk*)

##### Proof:

Let us take a set of *n* elements and split it into *k* subsets of length *n* / *k*.

We are then sorting a set of *k* elements *n* / *k* times, or...

<img src="https://latex.codecogs.com/gif.latex?\frac{n}{k}&space;\sum_{i=2}^{k}&space;i&space;=&space;\frac{n}{k}&space;\sum_{j=1}^{k}&space;(j-1)" title="\frac{n}{k} \sum_{i=2}^{k} i = \frac{n}{k} \sum_{j=1}^{k} (j-1)" />
<br>
<br>
<img src="https://latex.codecogs.com/gif.latex?=&space;\frac{n}{k}&space;*&space;\frac{k(k&plus;1)}{2}-1" title="= \frac{n}{k} * \frac{k(k+1)}{2}-1" />
<br>
<br>
<img src="https://latex.codecogs.com/gif.latex?=&space;\frac{n(k&plus;1)}{2}-1" title="= \frac{n(k+1)}{2}-1" />
<br>
<br>
<img src="https://latex.codecogs.com/gif.latex?=&space;\Theta(nk)" title="= \Theta(nk)" />
<br>
<br>

In terms of a recursion tree, merge sort originally bottoms out when the length of the set is 1. 
Instead, we are now bottoming out when the set has a length of size *n* / *k*, or...

<img src="https://latex.codecogs.com/gif.latex?T(n)&space;=&space;2T(\frac{n}{2k})&space;&plus;&space;cn" title="T(n) = 2T(\frac{n}{2k}) + cn" />
<br>
<br>

Thus, the height of the hybridized recursion tree is *lg* (*n* / *k*) + 1, with a cost of *cn* at 
each level.

So, T(n) = *cn* ( *lg* (*n* / *k*) + 1 )

= &#920;( *n* *lg* (*n* / *k*) )

Putting these two results together, we have the total cost of our algorithm as the sum of the
constituent parts...

&#920;( *nk* + *n* *lg* (*n* / *k*) )

**A bound on the value of *k***

It is easy to see that if *k* were larger than O(*lgn*) than the value of *k* would become more
than a constant factor.

This can be shown by substituting the bound on *k* into our hybrid's complexity equation, and as 
result, simplifying back to the original time-complexity for merge sort.

Let *k* = O( *lgn* ) = *lgn*

&#920;( *n lgn* + *n* *lg* (*n* / *lgn*) )

*n* / *lgn* must certainly grow slower than *n*, thus the leading expression dominates.

This results in &#920;( *n lgn* ).

QED

#### IRL

Constants pertaining to the particular instances of the algorithms, the languages they are written
in, and the machines they will be running on all affect performance.  As such, using our analytical
findings in conjunction with performance testing would give us an optimal solution for *k*.

We could simply run a series of tests, setting the value of *k* to the upper bound we found through
analysis.  We then run the sort, decrementing the value of *k* with each test case.  After which,
we then analyze the data looking for the optimal value for the length of the sub-lists.

## Problem Set 03
[Back to Top](#table-of-contents)

### Expected Length of a Coding Scheme

This simple exercise will look at the expected bit-length of 3 character encoding schemes.
The set of characters for all of these cases is: {A, B, C, D, E}.  The first scheme uses a 3-bit 
encoding for all 5 characters, i.e., 

A => 001

B => 010

C => 011

D => 100

E => 101

The characters in our set occur with the following probabilities:

Pr[A] = 0.3 ; Pr[B] = 0.3 ; Pr[C] = 0.2 ; Pr[D] = 0.1 ; Pr[E] = 0.1

Let X<sub>n</sub> be the expected length of encoding *n* letters

Then X<sub>n</sub> = *n* (3Pr[A] + 3Pr[B] + 3Pr[C] + 3Pr[D] + 3Pr[E])

= n (3(0.3) + 3(0.3) + 3(0.2) + 3(0.1) + 3(0.1))

= 3n

We now evaluate using the same probability distribution, but a different encoding length:

A => 0

B => 10 

C => 110

D => 1110

E => 1111

We will set up the following scheme:

A : 0 : A.length = 1 : Pr[A] = 0.3

B : 10 : B.length = 2 : Pr[B] = 0.3

C : 110 : C.length = 3 : Pr[C] = 0.2

D : 1110 : D.length = 4 : Pr[D] = 0.1

E : 1111 : E.length = 5 : Pr[E] = 0.1

Again we let X<sub>n</sub> be the expected length of encoding *n* letters...

X<sub>n</sub> = *n* (0.3 + 2(0.3) + 3(0.2) + 4(0.1) + 4(0.1))

= 2.3*n*

For the final encoding we use the same encoding length, but the probability distribution has changed to...

Pr[A] = 0.5 ; Pr[B] = 0.2 ; Pr[C] = 0.2 ; Pr[D] = 0.05 ; Pr[E] = 0.05

With this scheme we have the equation...

*n* (0.3 + 2(0.3) + 3(0.2) + 4(0.05) + 4(0.05))

= 1.9*n*

### Hashing with Chaining
[Back top Top](#table-of-contents)

We now consider a hash table with *m* slots that uses chaining for collision resolution.
With the table initially empty, we will find the probability that a chain of size *k* exists
after *k* insertions.

##### Proof: 

X<sub>k</sub> = After *k* keys, there is a chain of size *k*

= I { *k* *h*( *k* ) = n } , where *n* is some slot in the table.

The probability of a key mapping to some slot is 1 / *m*.  The following *k* - 1 keys must
also map to the same slot. 

&#8756; X<sub>k</sub> = 1 / *m*<sup>k - 1</sup> 

*Working on a way to format hash tables in an aesthetically pleasing way, until then this
section will be somewhat bare...*

### Skip Lists
(*coming soon! Again with the aesthetics...*)

## Problem Set 04
[Back top Top](#table-of-contents)

In this problem set we will tackle a few recurrence relations using the Master
Method.  For one of those relations, we will use our findings from the MM to
use as our guess within the substitution method.  Where will see that 
sometimes less really is more!

After, we'll take deeper look into binary search trees. We will first prove
a basic lemma by showing that the individual parts are true.  Then we'll run
through an exercise in modifying a deletion algorithm.  Finishing up with an 
algorithm to construct a balanced binary search tree along with a time
complexity analysis.

### Master Method
**T(*n*) = 2 T( *n* / 4 ) + &#8730;*n***

*a* = 2 ; *b* = 4 ; *f* (*n*) = *n*<sup>1/2</sup>

But *n*<sup>log<sub>4</sub>2</sup> = *n*<sup>1/2</sup>

So, in the relation we can express *f* as an element of the set
&#920;( n<sup>log<sub>b</sub>a</sup> ).  By case 2 of the master theorem,
this tells us that T(*n*) = &#920;(&#8730;*n* * lg*n*).

QED

**T(*n*) = 3 T( *n* / 9 ) + *n*<sup>3</sup>**

*a* = 3 ; *b* = 9 ; *f* (*n*) = *n*<sup>3</sup>

*n*<sup>log<sub>9</sub>3</sup> = *n*<sup>1/2</sup>

So, we have the cost at each level of the recurrence tree as an element of the set 
&#937;( *n*<sup>1/2 + &#949;</sup> ) where &#949; = 5 / 2.

When using a lower bound on our cost function , we must perform a check according to 
the method:
 
 *a* *f* ( *n* / *b* ) &#8804; *c* *f* (*n*) for some *c* &#60; 1 and 
sufficiently large values of *n*.

3 ( n / 9 )<sup>3</sup> &#8804; *c* *f* (*n*)

3 ( n / 3<sup>2</sup> )<sup>3</sup> &#8804;

( 3n<sup>3</sup> / 3<sup>6</sup> ) &#8804;

( n<sup>3</sup> / 3<sup>5</sup> ) &#8804; *c* *n*<sup>3</sup>

We can arbitrarily choose *c* = 1 / 3

&#8756; T(*n*) = &#920;( *n*<sup>3</sup> )

QED

**T(*n*) = 7 T( *n* / 3 ) + *n***

*a* = 7 ; *b* = 3 ; *f* (*n*) = *n*

n<sup>log<sub>3</sub>7</sup> &#62; *n*

So, *f* (*n*) = O (n<sup>log<sub>3</sub>7 - &#949;</sup>) for some constant &#949; &#62; 0

&#8756; T(*n*) = &#920;( n<sup>log<sub>3</sub>7</sup> ) by case 3 of the Master Theorem.

### Substitution
[Back to Top](#table-of-contents)

Here, we will use the above result from the relation...

**T(*n*) = 7 T( *n* / 3 ) + *n***

Guess: T(*n*) = &#920;( n<sup>log<sub>3</sub>7</sup> )

We must show that 0 &#8804; c*n*<sup>log<sub>3</sub>7</sup> &#8804; T(*n*) 	&#8804; 
cn<sup>log<sub>3</sub>7</sup>

0 &#8804; c( *n* / 3 )<sup>log<sub>3</sub>7</sup> + *n* &#8804; T(*n*) 	&#8804; 
c( *n* / 3 )<sup>log<sub>3</sub>7</sup> + *n*

0 &#8804; c( *n* )<sup>log<sub>3</sub>7</sup> + *n* &#8804; T(*n*) 	&#8804; 
c( *n* )<sup>log<sub>3</sub>7</sup> + *n*

This is where our attempt breaks down as there is no way to remove *n* from
the inequality as to match our guess.

Let's instead guess T(n) = &#920;( n<sup>log<sub>3</sub>7</sup> ) - *dn*

0 &#8804; c( *n* )<sup>log<sub>3</sub>7</sup> + *n* - *dn* &#8804; T(*n*) &#8804; 
c( *n* )<sup>log<sub>3</sub>7</sup> + *n* - *dn* 

Thus when *d* = 1 our inequality holds.

QED

### More Fun with Binary Search Trees
[Back to Top](#table-of-contents)

#### Lemmas
The lemma to be proven:

If a node X in a binary search tree has two children, then its successor S has no left
child and its predecessor P has no right child.
 
##### Claim: 
The successor S cannot be an ancestor or cousin of X

##### Proof:
Suppose that the successor S is an ancestor or cousin of X.

S, the successor is an ancestor, but S must also reside in the minimum of X's right subtree.
Surely this is impossible!  Consider now that S is a cousin of X, which means it resides at 
the same level with a different parent.  However, this implies X and S and not separated
by an intermediate node because S is X's successor, but two nodes that are cousins are surely
separated by a common ancestor above the parent!

&#8756;	our hypothesis is a contradiction.

##### Claim:
if X has two children, the successor S must then be contained in it's right sub tree such 
that the successor is the key with th minimum value.

##### Proof:
Suppose not, that is suppose X's successor is not located at the minimum of X's right
subtree.  There are only two other positions then: S is the parent of X, or S is somewhere in
X's left subtree.

Case 1: S is the parent of X.  We know that X has two children. So then S would be a
successor to multiple nodes, surely this contradicts the structure of a binary search tree.

Case 2: S is somewhere in X's left subtree.  If this were the case then S would both precede
and succeed X, but this is impossible!

&#8756;	our negated hypothesis is a contradiction.

##### Claim:
S cannot have a left child.

##### Proof:
Again, suppose not.  That is, suppose S does have a left child.  We showed
before that S is the minimum of X's right sub tree.  If S has a left child that child is 
the minimum or else contains the minimum, but if S is the successor then it is 
also the minimum. This is impossible! There is only one minimum!

QED

#### Deletion
[Back to Top](#table-of-contents)

The pseudocode for Tree-Delete (pg.298 CLRS)

```asp
TREE-DELETE(T, z) 
  if z.left == NIL
    Transplant(T, z, z.right)
  else if z.right == NIL
    Transplant(T, z, z.left)
  else
    y = Tree_Minimum(z.right)
    if y.p != Z
      Transplant(T, y, y.right)
      y.right = z.right
      y.right.p = y
    Transplant(T, z, y)
    y.left = z.left
    y.left.p = y  
```

The above pseudocode relies on the logic of the above lemma when finding the successor node to 
replace the deleted node in the event that the right sub tree is not NULL.

The following is a modification of the foregoing code using the predecessor rather than the
successor...

```asp
TREE-DELETE(T, z) 
  if z.left == NIL
    Transplant(T, z, z.right)
  else if z.right == NIL
    Transplant(T, z, z.left)
  else
    y = Tree_Minimum(z.left)
    if y.p != Z
      Transplant(T, y, y.left)
      y.left = z.left
      y.left.p = y
    Transplant(T, z, y)
    y.right = z.right
    y.right.p = y  
```

As one can easily see, the symmetry of the binary tree structure also us to take a 'mirror image,'
so to speak.

#### Constructing Balanced Trees
[Back to Top](#table-of-contents)

Binary trees can become linear if special care is not taken in regard to how a particular 
implementation of the tree is built.  We focus on *balance* here.  That is, we recursively define a 
balanced binary tree with balanced binary sub trees.  Here we assume a sorted array is passed as
the actual parameter.

```asp
BalancedBinaryTree(A, start, end)
  if start > end
    return NIL
    
  middle = floor((start + end) / 2)
  node = A[middle]
  
  node.left = BalancedBinaryTree(A, start, middle - 1)
  node.right = BalancedBinaryTree(A, middle + 1, end)
  
  return node
```

The two recurrence calls in the above code split the problem in to two parts.
Each subsequent problem is half the size of the array passed as the parameter.
A constant cost at each level of recursion is required to compute the middle
variable and assign pointers between nodes.

Thus we have T(*n*) = 2 T( *n* / 2 ) + *c* 

The recursion is obviously dominated by the leaves...

*c* = O ( *n*<sup>lg 2 - &#949;</sup> ) where &#949; = 1

&#8756;	T(*n*) = &#920;(*n*) by case 1 of the Master Theorem 

## Problem Set 05
[Back to Top](#table-of-contents)

Things are picking up momentum and getting a bit more interesting in problem set 05.  We'll
be taking an in-depth look into d-ary heaps.  This will include diving into a height analysis,
along with implementations of *Extract_Max* and *Insert*, with corresponding analyses for each.

We then pay a small homage to Dijkstra with an implementation of a *3_Way_Quicksort*.  A powerful
approach in the respect that such an algorithm addresses the weakness Quicksort has when 
it comes to sorted, reverse sorted, or nearly sorted inputs.

### D-Ary Heaps
To start things off, let's take a look at how one would represent a d-ary heap in an array.
We can accomplish this by verifying the following two formulas:

*j<sup>th</sup>*-*child* ( *i*, *j* ) = *d* ( *i* - 1 ) + *j* + 1

*D_Ary_Parent* ( *i* ) = *floor* ( ( *i* - 2 ) / *d* ) + 1

We will verify by way of a kind of inverse identity...

##### Claim:
*D_Ary_Parent* ( *j<sup>th</sup>*-*child* ( *i*, *j* ) ) = *i*

##### Proof:
Let us assume we are considering the first child so that *j* = 1

*floor* ( [ *d* ( *i* - 1 ) + 1 + 1 - 2 ] / *d* ) + 1 = *i*

= [ *d* ( *i* - 1 ) ] / *d* + 1

= *i* - 1 + 1

= *i*

QED

#### Height of a d-ary heap
[Back to Top](#table-of-contents)

The number of nodes in a completely filled *i* <sup>th</sup> level of some d-ary heap
would of course be *d <sup>i</sup>*.  Let us agree that the last filled level is the
*k <sup>th</sup>* level.  The number of nodes must then be the sum from level 0 to level
*k*.

Or, *d <sup>0</sup> + d <sup>1</sup> + d <sup>2</sup> + ... + d <sup>k</sup>*

<img src="https://latex.codecogs.com/gif.latex?=&space;\sum_{i=0}^{k}&space;d^i" title="= \sum_{i=0}^{k} d^i" />
<br/>
<br/>
Which is of course the simple geometric series...
<br>
<br>
<img src="https://latex.codecogs.com/gif.latex?=\frac{d^{k+1}-1}{d-1}" title="=\frac{d^{k+1}-1}{d-1}" />
<br>
<br>

The last node must reside in the filled level, or else it exists in the succeeding partially
filled level, *k* + 1.

Our final node, let's call it *n*, can be represented with the following inequality...

<br>
<img src="https://latex.codecogs.com/gif.latex?\frac{d^{k&plus;1}-1}{d-1}&space;\leq&space;n<&space;\frac{d^{k&plus;2}-1}{d-1}" title="\frac{d^{k+1}-1}{d-1} \leq n< \frac{d^{k+2}-1}{d-1}" />
<br>
<br>
<br>
<img src="https://latex.codecogs.com/gif.latex?d^{k&plus;1}-1\leq&space;n(d-1)<&space;d^{k&plus;2}-1" title="d^{k+1}-1\leq n(d-1)< d^{k+2}-1" />
<br>
<br>
<br>
<img src="https://latex.codecogs.com/gif.latex?d^{k&plus;1}\leq&space;n(d-1)&plus;1<&space;d^{k&plus;2}" title="d^{k+1}\leq n(d-1)+1< d^{k+2}" />
<br>
<br>
<br>
<img src="https://latex.codecogs.com/gif.latex?k&plus;1\leq&space;log_d(n(d-1)&plus;1)<&space;k&plus;2" title="k+1\leq log_d(n(d-1)+1)< k+2" />
<br>
<br>
<br>
<img src="https://latex.codecogs.com/gif.latex?k\leq&space;log_d(n(d-1)&plus;1)-1<&space;k&plus;1" title="k\leq log_d(n(d-1)+1)-1< k+1" />
<br>
<br>

The inequality holds for *k*, but in the case of a partial level we would take the ceiling
of the logarithm because at least *k* + 1 levels are needed as levels are discrete.

&#8756;	The height of a d-ary heap of *n* elements is...

&#8968; log<sub>d</sub> ( *n* ( *d* - 1 ) + 1 ) &#8969; - 1

Using the change of base formula this can be seen as...

&#920;	( *lg* ( *nd* ) )

QED

#### Extract_Max
[Back to Top](#table-of-contents)

To implement extract max on a d-ary heap would remain the same as an implementation
on a binary heap, except for an alteration to the *Max_Heapify* sub-routine.  The code
would be altered to simply find the largest element *j*, such that...

*j* = *max* ( A[ *i* ], the children of A[ *i* ] )

Therefore each node is compared to it's *d* children
```asp
for j = 1 to d 
  max = A[i]
  if A[j] > A[i]
    max = A[j]
return max
```

This results in *Extract_Max*'s run-time being &#920; ( *d lgn* )

#### Insert
This particular implementation would also remain the same as a binary heap.  That is,
the key would be inserted at the last position of the heap, then the *Heap_Increase_Key*
sub-routine would be called on the newly inserted key.  If a key were greater than the
parent, then it is also surely greater than it's siblings.  Thus the implementation does
not change.  The key is swapped with it's corresponding parent until the correct position
in the heap is found.  In the worst case the new key belongs at the root.

&#8756; *Insert* = &#920; ( *h* )

= &#920; ( *lg* ( *nd* ) )

### 3 Way Quicksort
[Back to Top](#table-of-contents)

See also [Dutch National Flag Problem](https://en.wikipedia.org/wiki/Dutch_national_flag_problem)

Here is a recursive implementation written in python...
```asp
def 3_Way_Partition(A, p, r):
  e, g, q = p, r, A[r] 
  i = e
  
  while i < r:
    if A[i] < q:
      A[i], A[e] = A[e], A[i]
      e += 1
    elif A[i] > q:
      A[i], A[g] = A[g], A[i]
      g -= 1
      i -= 1
    i += 1
  return e, g
  
def 3_Way_Quicksort(A, p, r):
  if p < r:
    e, g = 3_Way_Partition
    3_Way_Quicksort(A, p, e - 1)
    3_Way_Quicksort(A, g + 1, r)
```

If we assume a balanced partitioning of *3_Way_Quicksort*, we still have 2 
sub-problems each half the size of the original.  For random valued keys as the
partition A[ *e*..*g* ] could be empty with a high probability.

Thus, T( *n* ) = 2 T ( *n* / 2 ) + &#920; ( *n* )

= &#920; ( *n lgn* ) by the Master Theorem

Now in the case of *n* identical items, *3_Way_Partition* is adaptive as it will 
iterate over the *n* elements only one time.  By doing this it creates a partition
where *p* = *e* and *g* = *r*.  In other words, the conditional base case of the 
recursion is true for a single call to *3_Way_Quicksort*, namely the initial call. 

T( *n* ) = 0 T ( *n* / 2 ) + &#920; ( *n* )

= &#920; ( *n* )

QED

### Problem Set 06
[Back to Top](#table-of-contents)

We have past the half way point and find ourselves dealing with ways to escape the clutches
of O(*nlgn*) comparison based sorting.  After that, we work with red-black trees and they're
equivalent (2, 4) representations.

#### Sorting Large Numbers
What we are faced with is the task of sorting *n* integers in the range of 0 to 
*n* <sup>4</sup> - 1 in O( *n* ) time.

Let's look at *Counting_Sort* as an option...

From CLRS (pg. 195)
```asp
Counting_Sort(A, b, k)
  let C[0..k] be a new array
  for i = 0 to k
    C[i] = 0
  for j = 1 to A.length
    C[A[j]] = C[A[j]] + 1
.
.
.
```
We need only examine the first for loop to see that *Counting_Sort* does not accomplish our goal.
The loop iterates *i* = 0 to *k*, menaing &#920;( *k* ) time.  The overall time remains as 
&#920;( *n* + *k* ), but *k* = *n* <sup>4</sup> - 1.

Thus the best run time that *Counting_Sort* can achieve is &#920;( *n* <sup>4</sup> ).  Which
certainly does not achieve our goal of a linear sorting time.

Let's instead take a look into *Radix_Sort*...

From CLRS (pg. 198)
```asp
Radix_Sort(A, d)
  for i = 1 to d
    use a stable sort to sort array A on digit i
```
Again, our problem consists of *n* integers ranging in values from 0 to *n* <sup>4</sup> - 1.
Then we have *d* = log<sub>k</sub>( *n* ) digits.  In the worst case *d* is not constant and thus
with base-10 counting, *k* is in the domain [0, 9].

*Radix_Sort* = &#920; ( log<sub>k</sub>( *n* )( *n* + 10) )

= &#920;( *n lgn* )

We need to make a slight change to our representation of the integers comprising our array
if we wish to achieve escape velocity, if you will, from the &#920;( *n lgn* ) bound.

The issue with an unmodified *Radix_Sort* is the logarithmic term corresponding to the amount
of digits to loop through.  In the foregoing attempt we used base-10 digits, thus *k* = 10.
and *d* = log<sub>k</sub>( *n* ).  Instead, if we let *k* = *n* then our *n* integers are
represented as base-*n* giving us...

*Modified_Radix_Sort* = &#920;( log<sub>n</sub>( *n* )( *n* + *n*) )

= &#920;( 2*n*)

= &#920;( *n* )

Our goal has been achieved!

#### Red Black Trees
[Back to Top](#table-of-contents)

<img class="image" src="https://github.com/whoodes/whoodes.github.io/blob/master/images/%233-a-md.png" />
