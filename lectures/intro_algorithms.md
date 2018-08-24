#  Introduction to Algorithm

*Notes on MIT lectures, start writing from Aug 21, 2018*

* toc
{:toc}

## 1 Intro

### insertion sort

> iteration: sort first j of the array, then sort j+1 into it

$\Theta(n^2)$

### merge sort

> recursive: sort $A[1]$ to $A[ceil[n/2]]$and $A[ceil[n/2]+1]$ to $A[n]$
> until one element remain
> then merge the two groups (see below)

key subroutine: merge

> pick the smallest, then go on, every time you have two candiates from two subgroups to determine

for the complexity analysis, one again follow the recursive logic to get the recursive formula: $T(n)=2T(n/2)+\Theta(n)$, the first term on the right is for recursive and the second term is for the merge routine. You need solve a recurrence.

Recursion tree: make $\Theta(n)=cn$, following a brute-force expansion as a tree structure on paper. Every level of the tree count a $\Theta(n)$ and the depth of the tree is $\ln n$.

$\Theta(n\ln n)$

### complexity

worst time, asymptotic estimation

best time estimate make no sense as it can cheat

$\Theta$ notation for asymptotic analysis

## 2 Math basics

### asymptotic notation

* Big O notation ($\leq$)

$f(n)=O(g(n))$ means there are consts $C,n_0$, such that $0\leq f(n)\leq cg(n)$ for all $n>n_0$.

$O(g(n))$ can also be viewed as function sets with all possible $f(n)$.

macro convention: A set in a formula represents an anonymous function in that set. eg. $f=n^3+O(n^2)$.

large O in left hand, asymmetric understanding of the 'equal': is or belongs to. 

eg. $n^2+O(n)=O(n^2)$: for any $h$ in $O(n)$ there is a $f$ in $O(n^2)$, such that $n^2+h=f$.

* Big Omega notation ($\geq$)

$\Omega(g(n))$ is a set of functions which there exist const $c,n_0$, such that $0\leq cg(n)\leq f(n)$ for all $n\geq n_0$.

eg. $\sqrt{n}=\Omega(\ln n)$.

* Big Theta notation ($=$)

$\Theta(g(n))=O(g(n))\cap \Omega(g(n))$.

* little o($<$) and omega($>$) notation:

hold for all constant $c$ instead just for one, $n_0$ can dependent on $c$. c plays role of epsilon here.
eg. $2n^2=o(n^3)$, $n^2=\Theta(n^2)\neq o(n^2)$.

### recurrence solution

No general approach, of course.

* Substitution method

guess the answer form and match the coefficient.

loop hole: induction proof on $n=O(1)$. Since $1=O(1)$, by induction, assume $n-1=O(1)$, it is easy to conclude that $n=O(1)$ which is obviously not true. 
hint: never use O notation in induction, when n is expected to be infinite in some sense, the constant c in the O notation are actually increase each time which might end up infinite when n is infinite. Therefore when one use induction expand large O notation explicitly with constants.

in the subsitution method, one may want propose a O notation of the solution instead of explicit function form, and use induction to prove the O assumption. Such a bound guess is enough for complexity estimation, one dont need to exactly solve the recurrence, let alone there may be some O notation in the recurence at beginning. 

Sometimes, one need tighten the bound of O notation a little by add lower order expansions to prove the exact bound. eg $O(n^2)\sim c_1n^2-c_2n$ may be necessary for the proof. 

So basically the question, given $T(n)=4T(n/2)+n$, show that $T(n)<= c n^2$ for some constant n using induction, a cool problem because it is simpler to prove a tighter conclusion than the original one, which is hard to come up.

* Recursion Tree Method

not rigourous, just illustration.

eg. $T(n)=T(n/4)+T(n/2)+n^2$, expand in tree form with dot dot dot and add all thights up. Every step in expansion, writing constant above and $T$ below for another expansion. Finally estimate the leaf numbers and depth, then sum level by level. For the example above, it is a geometric series for level sum.

* Master method

Only applies to particular famlily of recurrence. $T(n)=aT(n/b)+f(n)$. $a\geq 1, b>1$, $f(n)$ should be asymptotically positive (for large enough n, f should be positive).

Thm: three cases below

Case 1) $f(n)=O(n^{\log_b a-\epsilon})$ for some $\epsilon>0$, then $T(n)=\Theta(n^{\log_b a})$.

Case 2) $f(n)=\Theta(n^{\log_b a}\log^k_2 n)$ for some $k\geq 0$, then $T(n)=\Theta(n^{\log_b a}\log_2^{k+1}n)$.

Case 3) $f(n)=\Omega(n^{\log_b a+\epsilon})$ for some $\epsilon>0$ and $af(n/b)\leq (1-\epsilon')f(n)$ for some $\epsilon'>0$, then $T(n)=\Theta(f(n))$.

How to apply: 1) compute $n^{log_b a}$, see f (compare f with) and jump to the specific case, note $k=0$ if not occur in f and $k<0$ case cannot be derived by master method.

This thm can be shown by recursive tree very easily.

## 3 Divide and Conquer

devide et impera

1. divide 2. conquer 3.combine

* binary search

find x in a already sorted array: 1 compare x with middle elements 2 reduce to find x in one sub array 3 combine: nothing. $T(n)=T(n/2)+\Theta(1)$, ie $T(n)=\log n$.

* powering a number

given number x and power interger $n\geq 0$, return $x^n$.

Naive alg: $\Theta(n)$. 

D&C: $f(x,n)=f(x,n/2)*f(x,n/2)$ , for odd n, just time one x more. Complexity: $\Theta(\log n)$

* Fibonacci number

Naive recursive: $\Omega(\phi^n)$ where $\phi=\frac{1+\sqrt 5}{2}$.

Bottom-up: compute in order, save them in cache: $\Theta(n)$

Naive recursive squaring: $F_n = round(\frac{\phi^n}{\sqrt 5})$, $\Theta(\log n)$, floating number destroy the approach

The correct approach: Thm: $\begin{pmatrix}F_{n+1}& F_n\\F_n& F_{n-1}\end{pmatrix}=\begin{pmatrix}1&1\\1&0\end{pmatrix}^n$. complexity $\Theta(\log n)$

* Matrix multiplication

$A=[a_{ij}], B=[b_{ij}]$ two matrix with $n*n$, return $C=[c_{ij}]$.

Naive: $\Theta(n^3)$

First B&C: divide matrix in four block for each one ($n/2\times n/2$) . $T(n)=8T(n/2)+\Theta(n^2)$, namely the complexity is still $\Theta(n^3)$.

Strassen's alg: somehow reduce number of multiplications from 8 to 7! Recall the division $A=[a,b,c,d]$ and $B=[e,f,g,h]$, we now compute seven $P_i$ each involving one matrix multiplication. And reudce the seven P to the four result blocks with only plus. Following these formula one can easily show the correctness of the alg. We now have subcubic alg for matrix multiplication: $T(n)=7T(n/2)+\Theta(n^2)$.

$\Theta(n^{\log 7})$.

*How to apply to non $2^n$ matrix. First, one can zero padding. And the method can similarly applies to non-square matrix too.*

Best so far alg is $\Theta(n^{2.376})$.

* VLSI (very large scale integration) layout

Problem: complete binary tree with n leaves, embed into agrid with minimum area. (embed meaning: nodes on vortex and link on edge of the square grid without crossing)

naive embedding: just drawing as the original structure of the tree on the grid. The height $H(n)=H(n/2)+\Theta(1)$, the weights $W(n)=2W(n/2)+O(1)$. The area is $\Theta(n\log n)$. Not a minimum configuration.

the H layout: every level lools like H letter where root node in the middle. Now the recurrence is $H(n)=2H(n/4)+\Theta(1)$, namely the $H(n)=\Theta(\sqrt n)$ and namely the area is of order n.

## 4 Quicksort

### quicksort

sorts in place (in terms of space); D&C; practical (with tuning)

> partition the input array into two subarray around pivot  x such that elements in the lower subarray $\leq x \leq$ elements in upper subarray
>
> recursively sort the two subarrays
>
> combine: trivial

subroutine partition:

> input A[p..q], pick pivot as A[p], compare all elements to the pivot by loop, if the elements is small than the pivot, exchange it with the one one the boundary, which is an integer index tracked by the loop at the same time

subroutine complexity: $\Theta(n)$. 

 tail recursive optimization

Analysis: assuming all elements are distinct. 

* Worst time analysis: Again $T(n)$ as the worst-case time. Worst case is that the input is sorted or reverse-sorted in which one of the partition has no elements. ie $T(n)=T(0)+T(n-1)+\Theta(n)$,  $T(n)=\Theta(n^2)$. 
* Best time analysis: partition split in the middle every time. $T(n)=2T(n/2)+\Theta(n)$, $T(n)=n\log n$.
* Suppose the split is alway 0.1:0.9. $T(n)=T(n/10)+T(9n/10)+\Theta(n)​$, do a recursion tree, and it gives every level no larger than $cn​$ with height between $\log_{10/9}n​$ and $\log_{10} n​$, and this gives $T(n)=\Theta(n\log n)​$, as lucky as the half split.
* Suppose we alternate lucky and unlucky case in the partition process. $L(n)=2U(n/2)+\Theta(n)$, and $U(n)=L(n-1)+\Theta(n)$. L and U for luck and unluck time. By combine the two, we have $L(n)=\Theta (n\log n)$.

### randomized quicksort

By picking random pivot every time. running time is independet on input array. no assumption on input distribution. no specific input can elicit the worst case behavior.

let $T(n)$ as the random variable. Define random variable $X_k=\delta(k=i)$, where i is the pivot out of n elements. $T(n)=\sum_k(T(k)+T(n-k)+\Theta(n))X_k$ with the probability of $1/n$.

Then $E[T(n)]=\sum E(X_k)*E(T+T-\Theta)$, due to the independence. $E[T(n)]=2/n(\sum_{k=2}^{n-1} E(T(k)))+\Theta(n)$. 

And prove $E=\Theta(n\log n)$ by substitution method of induction. One need the fact $\sum_{k=2}^{n-1}k\log k\leq 1/2 n^2\log n-1/8 n^2$, which is very easy to show by integral.

### *heapsort*

make use of complete binary tree (not necessary full binary tree) which can transform as a list with fixed relation to find the parent and child node indexes. if within such a tree, every node value are not less than their children, we call it max heap. 

two steps of heapsort: 1) make maxheap from array 2) pickup the max item(the first one) then maxheapify the new array

complexity: $\Theta(n \log n)$ due to $\Theta(\log n)$ time for heapify a node (insert node at the last position and make it move up to appropriate positions), in step 1, one need heapify around n nodes and in step 2, one heapify only root node each time sort one element. Note to build the heapify, it can only cost $O(n)$ time, there is a huge difference between using siftup or siftdown! 

See [this](https://stackoverflow.com/questions/9755721/how-can-building-a-heap-be-on-time-complexity) and [this](http://128kj.iteye.com/blog/1728555) for algorithm  on heapsort and see [wiki](https://en.wikipedia.org/wiki/Sorting_algorithm) on more aspects of sorting algorithms.

## 5 sort in linear time

How fast can we sort: it depends on models of what you can do with the elements.

all the above are comparison sorting models, you can only allowed to do compare pairs.

### map to the decision trees

Thm: no comparison sorting algorithm can run faster than $\Theta(n\log n)$.

Decision-tree model:

Give all the decision comparison branches with leaves as the sorting results.

Map decision tree to comparison sorts: one tree for each n.

Height of the tree is the worst time of sort alg.

Lower bound on decision tree sorting: the height is at least $n\log n$. Since no. of leaves is $n!$ , a tree with height h has at most $2^h$ leaves. Therefore $h_{min}=\log_2 n!\sim \log n^n$, DONE.

### sort beyond comparison model

* counting sort

each element of n-array input is an integer between 1 to k. 

require a length k asstitance array to keep the count of each category. $\Theta (k+n)$. 

* stable sort

so called stability preserve the order of equal elements, this property plays role when the elements are tuple or some thing with more info.

* radix sort

sort say integers with many digitals from the last (namely less importat at first) digital (using stable sort).

correctness proof (induction): induct on digit position that already sorting t, by assuming t-1 digits are already sorted.

comlexity analysis: using counting sort for each digit (one or several a time). assume each binary as b bits long, split it into single bit a time may not be optimal, so we set the split as b/r. digits are each r bits long. $T=b/r\Theta(2^{r}+n)$, where n is 2 this case a fix number for counting system. Now one only need to minimize T in terms of r (optimal when $r\sim \log n$). Finally we have $T=O(\frac{b n}{\log n})\sim O(\log k)$ where k is the range of the number.

## 6 Order statistics

given unsorted array, return the k-th smallest elements (elements of rank k).

Naive: sort first and return the k-th element. $\Theta(n\log n)$.

If $k=1$ or $k=n$, you can keep the minimum while scan the array. $\Theta(n)$

Medium case: a bit trickier

### randomized D&C (random select)

> recursively use the random partition from quicksort, which returns the final position index of pivot element (randomly chosen) k. assuming the rank we want is i, if i=k, done; if $i<k$ or $i>k$ , recusively use the partition subroutine for left of right subarray (remember the rank i we are looking for may get offsets if we now moves to the right subarray!).

Analysis: first assuming elements all distinct.

Best case: constant split every time, $T(n)=T(9/10n)+\Theta(n)$, namely $T(n)=\Theta(n)$.

Worst case: pivot always on the edge, $T(n)=T(n-1)+\Theta(n)$, namely $T(n)=n^2$.

Average case:  $ET(n)\leq\sum_k E[(T(\max(n-k-1,k))+\Theta(n))X_k]=1/n\sum_k E[T(\max(n-k-1,k))]+\Theta(n)$.

$ET(n)\leq 2/n\sum_{k=[n/2]}ET(k)$. Then use substitution induction method to show $ET=\Theta(n)$.

### worst-case linear-time order statistics

generate good pivot recursively.

> Divide the n elements array into 5 elements subarrays ([n/5]), omit the residue part.
>
> Recursively find median in each 5-elements group. $\Theta(n)$
>
> Recursively compute the median x of these medians. T(n/5)
>
> Use x as the pivot and do the partition $\Theta(n)$ and recursive call as above alg. T(3/4 n)

By the median choice, we have 3*[[n/5]/2] elements less and equal to x(the same for larger and equal). If n is large enough, this value is larger than $n/4$. 

The complexity recurrence $T(n)\leq T(n/5)+T(3/4n)+\Theta(n)$. Substitution induction to prove $T(n)=\Theta(n)$. If one want to get a linear upper bound, the intuition is make all the recurrence work unit together smaller than 1. Practically, this algorithm have large prefactor constants so maybe not so fast.

Why group of 5? Number larger than 5 also works. You need require $1/k+(k/2+1)/2k<1$, where I omit some [] function.

## 7 hashing

symbol table problem: table S holding n records, key-data pair

Operations: Insert (by pair), Delete (by key), Search (by key)

Direct access table: works when keys are distinct and are drawn a set U of m elements, namely two list one for keys and one for values. All operations take $\Theta(1)$ time in the worst case. Very impractical.

hash function H to map keys randomly into slot(index) table T. 

collision: resolve collisions by **chaining**. link records in the same slot into a list.

worst case analysis: all keys are hased to the same slot. (reduce to linked list). access takes $\Theta(n)$ time.

average case: simple uniform hashing. one hashtable, n keys and m slots. The load factor is $\alpha=n/m$, which is average no. of keys per slot.

Expected unsuccessful search time (return None): $\Theta(1+\alpha)$, first hash the value for the key and search the linked list.

### choose hash functions

regularity of key distributions should not affect unifomity of hash.

* Division method

$h(k)=k \mod m$. m with small factor is bad. Even mod even are always even, so m with factor 2, will map all even to even instead of uniformity. Pick m as a prime (not too close to power of 2 or 10) is good practice.

* Multiplication method

Assume the slot $m=2^r$, w-bit words, $h(k)=(Ak \mod 2^w).rightshift(w-r)$. A is an odd integer with bits the same as w. The formula is in the language of binary. A do not: not pick near power of 2.

### resolve collisions by open addressing

if collision use different hash function, probe table systematic till empty slots is found. $h(keys,probnum)\rightarrow slots$. deletion is difficult in this scheme, one can add a state to label the delete status instead of really delete items. 

* linear probing

$h(k,i)=(h'(k)+i) \mod m$. if filled, just scan down the nex slot. disadvantage: primary clustering (long runs of filled slots)

* double hashing

good scheme. $h(k,i)=(h_1(k)+i h_2(k))\mod m$. usually m is taken as power of 2 and force $h_2$ to be odd.

Analysis of open addressing:

assumption of uniform of hashing: each key is equally likely to have any of the $m$ permutations as its probe sequence independent of other keys.

Thm: E[#probes_of_fail]$\leq 1/(1-\alpha)$, of course $\alpha<1$ is required in open addressing scheme

Proof: for unsuccessful search, first hit and with $n/m$ probability to get collision, and then a second probe, again collision probability $(n-1)/(m-1)$ and so on. Note $(n-i)/(m-i)<\alpha$, sum them together, we have the thm results. The same result applies to successful search case. So make th hash table sparse to keep it fast.

##  8 hashing II

### universal hashing

idea: choose a hash function at random to avoid delibrately collision. *Within one hashtable, we dont randomly pick hash functions everytime, instead it is only selected when initilizing the hash table once. See [this post](https://stackoverflow.com/questions/10416404/finding-items-in-an-universal-hash-table) in stackoverflow*

U be a universe of keys, and H be a finite collection of hash functions: maping U to to sorts 1 to m.

Definition: We say H is universal if for all paris of distinct keys,  the number of h in H, which map the two keys into the same slot, is $|H|/m$. i.e. if h is chosen randomly from H, the probability of collision between two keys is $1/m$.

Thm: choose h randomly from H, suppose we hash n keys in T into m slots, for a given key x, the expecation number of collision with x is less than $\alpha=n/m$.

Proof: $C_x$ be the random variables denoting the total number of collsions of keys in T with x. $c_{xy}$ is nonzero only when keys x and y collision. namely $E(c_{xy})=1/m$. As $C_x=\sum_{y\in T-\{x\}}c_{xy}$, $E(C_x)=(n-1)/m$. 

### construction of universal hash functions

m is prime, decomose key k into r+1 digits based m (take mod of m step by step to get the r+1 digits decomp). pick $a$ at random, which is also based-m number, each a for a hash function. Define hash $h_a(k)=\sum a_ik_i\mod m$.

How big is the set of hash function $|H|$? Answer: $m^{r+1}$. 

Proof on H is universal: pick two distinct keys x, y with their decomposition representation in based-m. Say they are different in the zero digit. Count the number of $h_a$ which collide x,y, ie $h_a(x)=h_a(y)$. Namely $a_ix_i \equiv a_iy_i\mod m$, where summation convention is assumed,

$a_0(x_0-y_0)\equiv  \sum_{i=1}^r -a_i(x_i-y_i)  (\mod m)$ 

* Number theory fact detour

(from theory of finite fields) let m be **prime**, for any $z\in Z_m$ such that $z\not\equiv 0$, there exists a unique $z^{-1}\in Z_m$ such that $zz^{-1}\equiv 1 \mod m$.

So we now definitely have well defined $(x_0-y_0)^{-1}$ , therefore

$a_0 \equiv -\sum a_i(x_i-y_i) (x_0-y_0)^{-1}$, so freedom minus 1, and the number of possible $a$ to collide is $|H|/m$.

### perfect hashing

keys are all give at the beginning , determine the best hash function for constructing the static hash table. with space $m=O(n)$, make search $O(1)$ in time in the worst case (actually only two times of hashes).

idea: two-level scheme with universal hashing at two levels. If collide at level 1 slot, then the slot only save lists of random label $a$ for the second hash. no collision in level 2. if n collision in level 1, use $n_i^2$ slots for level 2 hash i. 

Thm: hash n keys to $n^2$ slots using h from universal set H, the expected number of collisions is less than $1/2$. The probability to collide is $1/n^2$, and no. of pairs key is $C_n^2$, hence the expected no. of collisions are products of the two factors, which is $1/2-1/(2n)$. (similar analysis as birthday paradox)
No collision at all probability is at least 1/2. 

Since the probability is very large, just try some random hash function, and one should hopefully work as $h_i$ from collision slots in level 1 to level 2.


* Markov inequality

For random variable $X\geq 0$ , $Pr[x\geq t]\leq \frac{E[x]}{t}$. (*too trivial to be a statement...*)

Space analysis: level 1, choose number of slots m to be equal to the number of keys n. let $n_i$ be random variable living in i slot in level 1. In level 2: the total space is $E=n+\sum_{i=1}^{n-1}E(n_i^2)=\Theta(n)$. Noting the counting of collsion is $\sum E(n_i^2)=\sum\sum E(I_{ij})$ , where I is the indicator variable give 1 when $h(i)=h(j)$. 

There are some middleware steps. First, if $\sum n_i^2> cn$ for some constant c, redo step 1, namely pick a new hash function h to rehash all things. The most nontrivial part of perfect hashing idea is actually those square level 2 slots are of the same order of n in total.

To summarize, pefect hashing is the type of an algorithm of designing hash function of fixed $n$ given keys which requires $O(1)$ times for accessing data by keys, and with space memory (slots in total) at most of order of keys $O(n)$. Furthermore, to find such function it should take no more than polyminal time with high probability (or in expectation context). (Actually we cannot assert this is always the case even for worst case, and this is a general property of algorithms with randomness. Say one flip the coins, he may not get heads for 1000 times of trials in theory, so we can only assert in expectation language or in high probability language). 

To recap the solution, we design a two level universal hashing. For colliding keys in the first round we find a second round hashing for each slot in first round and separate these keys in the second round. This is guaranteed by the square of the size of conflict keys in the first round in each slots. And similar to the birthday paradox, it is easy to show than the probability of avoding collision in the second round is lager than 0.5. In other words, it is fast to find effective hash function from the universal set for every second-round hash functions. The non-trivial part of that is in fact such a large space of slots in two levels are also of order $n$. In other words,  we can tune $c$ to make the probability of picking first round hashing probility also lager than 0.5. Therefore we can conclude that such a scheme can be designed in polyminal times, which finish the proof.

## 9 binary search trees (BST)

Some word on BST: insert - compare again and again, insert as a leaf. delete - find the exchange partner leaf, exchange and done.

balanced vs unbalanced: $\Theta(\log n)$ vs $\Theta(n)$

BST sort: 1) insert each item of array into BST, 2) do in-order tree-walk (recursively print left child then right, which lead to in-order traverse, i.e. left middle right sequence).

complexity: 1) $\Omega(n\log n)$ and $O(n^2)$ 2) $O(n)$ 

for 1) worst case: already sorted, best case: balanced tree in every step

Note the similarity between BST sort and quicksort, they make the same comparisons in different order.

Following the pivots of quicksort in each step (the first elements in remaining arrays), we have the exactly same tree as BST of the array.

### randomized BST sort

> randomly permute the input array and then call the usual BST sort.

Such an algorithm is equvilent to randomized version of quick sort. So the time complexity is the same.

$\Theta(n\log n)=E(\sum_{x}depth(x))$. Therefore, the average depth of elements in the tree is $\Theta(\log n)$. (not the height of the tree: which is the max depth of node).

Thm: E(height of rand BST) is $O(\log n)$. (the depth expecation is trivial but this thm is nontrivial)

### the proof 

Jensen's ineuqailty: convex function f and random variable x: $f(E[x])\leq E(f(x))$.

Basic idea, let $Y_n = 2^{X_n}$, where X is the height random variable, and we prove $E(Y_n)=O(n^3)$ first. 

If the root has rank k, then we have $X_n=1+\max(X_{k-1},X_{n-k})$. 

$Y_n=2\max(Y_{k-1},Y_{n-k})$. 

def indicator random variables: $Z_{nk}$ is 1 when the root has rank k and zero otherwise. $E(Z_{nk}=1/n)$.

$Y_n=\sum_{k=1}^n Z_{nk}2\max(Y_{k-1},Y_{n-k})$,

for expecation $E[Y_n]=2/n(\sum E[\max(Y_{k-1},Y_{n-k})])\leq 4/n \sum E(Y_k)$ ,  now we have the recurrence. We can use substitution method to prove this by induction $E[Y_n]\leq c n^3$. Done. (you have to use integral of cubic sum in the induction proof).

Precisely $E\approx 2.9882 \log n$.

## 10 balanced search trees

a tree guaranteed to be $\log n$ in height

including: AVL trees; 2-3 trees; 2-3-4 trees; B-trees; red-black trees; skip lists; treaps

### Red-black trees

binary search trees with extra color filed in each node.2

red-black property: 

1. Every node is either red or black

2. The root and leaves (imaginary null pointers, not real leaf nodes as usual) are all black

3. The parents of every red node is black

4. All simple path from node X to a descendant leaf of x: have same number of black nodes on the path. (black-height of x: exclude x itself) In other words: every node has consistent black-height definitions.

Thm: The height of red-black trees of n keys at most $2\log(n+1)$.

Proof sketch: Merge each red node to its parent black node (not a binary tree anymore, but 2-3-4 tree, all the leaves now has the same height which is the black-height in the original tree).

Now we have:

1. every internal nodes has 2-4 children
2. every leaves have the same depth, the height of this tree is denoted as $h'$

note there are $n+1$ leaves for n node trees. also in 2-3-4 tree, the number of leaves has to be beteen $2^{h'}$ to $4^{h'}$. Therefore $h'\leq \log(n+1)$. Furthermore $h\leq 2h'$ due to property 3 of red-black tree. Done the proof.

How to do relevant operations (insert or delete) to keep the tree of red-black trees.

* insert

> do BST insert, and pick red for it (property 3 is violated)
>
> color changes: move violation up by recoloring until we can fix it by rotation and recoloring. specifically, the subroutine is when the input node x is red and x.p is also red, we need to resolve the violation, do the loop: suppose x.p == x.p.p.l (x.p == x.p.p.r is similar), set y=x.p.p.r (uncle of x), if y is red, do case 1. elif  x == p.r, do case 2. if x == p.l do case3. 
>
> finally fix root as black
>
> case 1) x.p and x.uncle change to black, x.p.p change to red, recusively input x.p.p into the subroutine 
>
> case 2) left rotation on x.p, then x==x.p.l just go to case 3)
>
> case 3)  recolor x.p and x.p.p and right rotate of x.p.p done.

right-rotate of some node (left child turn to parent and the right child of original left child are not the left child of the original root). preserve binary search property. the reveser operation is left-rotate. rotation takes constant time.

It is nice the rotation takes $O(1)$ and less than recoloring, because recoloring doesn't affect the queries.

* *deletion*

first of all, if the node to be deleted has two non-leaves children, then use the similar delete method for binary tree(namely find the node with max value in left descendants), exchange them and delete that one which is at most with one non leave child. And if the node to be deleted has only leaves children, already done.

so we only care about cases where node with one non-leaves node child. again case by case, delete red, with black child instead of it done! delete black with red child, move up the child can recolor it to black, done! 

The only subtle case is black node with one black child. For detailed process, see [wiki](https://en.wikipedia.org/wiki/Red%E2%80%93black_tree#Removal).

*see visualization of rb trees in this [post](http://saturnman.blog.163.com/blog/static/5576112010969420383/).*

### *other types of balanced trees*

* treap

treap = tree (with value)+ heap (with priority), priority is random given at the beginnig of insertion.

For insert operation, first insert the node as binary tree, and use siftup in heapify to move it up. Since all operations in siftup are rotation which preserve the binary tree requirements, we can finnaly have a treap. Note this takes $O(\log n) $ rotations compared to RB tree, where only O(1) rotations.

In a word, treap is the dynamic version of randomized binary trees. We introduce priority set of values to realize the permutation of input array dynamically. So it can not gurantee the average cost when worst case happens.

* AVL trees

from any node, the difference height between left and right subtree are not greater than 1. single and double rotations are needed for insertion and deletion.

* B trees

2-3 or 2-3-4 trees are all special cases of general B trees. See detail about B-trees [here](https://blog.csdn.net/v_july_v/article/details/6530142).

database prefer trees than hashtables: see [this](https://stackoverflow.com/questions/7306316/b-tree-vs-hash-table)

* skiplist

idea: two double linked list with cross pointer linker with the same value, one list has less members.

search in such 2 linked list, walk on top list until go too far (transit to the lower list with all members then)

actually see lectures later, no more notes here for skiplist.



## TODO

- [x] binary trees basic operations implementation
- [x] dynamics of randomized binary search trees
- [ ] all kinds of balanced trees implementation 
- [ ] induction proof of height of red-black tree