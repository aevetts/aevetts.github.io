---
layout: single
title:  "Heisenberg groups 2: Growth"
permalink: /heisenberggrowth/
---

<!-- ### Higher Heisenberg groups -->

In my last post I wrote about the *first* Heisenberg group and normal forms. As the name suggests, it is just the first of a family of similar groups, sometimes known as the *higher Heisenberg groups*. For each positive integer $r$, $H_r$ is the group of $(r+2)\times(r+2)$ upper triangular matrices with ones on the diagonal, integers in the first row and last column, and zeroes in the rest of the upper triangle. For example, $H_2$ is all matrices of the form

$$\begin{pmatrix} 1 & j_1 & j_2 & k \\ 
                  0 & 1 & 0 & i_1 \\
                  0 & 0 & 1 & i_2 \\
                  0 & 0 & 0 & 1 \end{pmatrix}$$ 

where $i_1,i_2,j_1,j_2,k$ are integers. Generalising the argument from last time proves that $H_r$ has a presentation

$$H_r = \left\langle a_1,\cdots,a_r,b_1,\cdots,b_r \mid \substack{[b_i,a_j]=[a_i,a_j]=[b_i,b_j]\text{ for }i\neq j,\\
[b_i,a_i]=[b_j,a_j],~[[b_i,a_i],x]=1\text{ for all }x}\right\rangle$$

which isn't the easiest thing to parse. But, as before, we can always rearrange things so that the $a$s are to the left of the $b$s (in order), with a power of $c=[b_i,a_i]$ at the end, giving the normal form

$$\left\{a_1^{i_1}a_2^{i_2}\cdots a_r^{i_r}b_1^{j_1}b_2^{j_2}\cdots b_r^{i_r}c^k \colon i_\lambda,j_\lambda,k\in\mathbb{Z} \right\}.$$

For $r=1$ this is the free class 2 nilpotent group of rank 2. However, the fact that all of the non-trivial commutators are equal means that $H_r$ is no longer free in this sense for larger $r$. For more on free nilpotent groups, see [this](https://terrytao.wordpress.com/2009/12/21/the-free-nilpotent-group/) old blog post of Terry Tao, or look up 'basic commutators' in, for example, Hall's classic [The Theory of Groups](https://books.google.co.uk/books/about/The_Theory_of_Groups.html?id=oyxnWF9ssI8C&redir_esc=y).



### Growth
A classic invariant of geometric group theory is the *growth* of a group. The (cumulative) growth function $\beta(n)$ is the number of group elements that can be reached with a word of length at most $n$. This depends on the choice of generating set, and to make it an invariant we must consider it up to a certain equivalence relation.

In the first Heisenberg group, we can map the normal form $a^ib^jc^k$ to coordinates in Euclidean 3-space. From the definition of $c$, we get that $[b^n,a^n]=c^{n^2}$, so we can reach a quadratic number of elements of the form $c^k$ with words of only linear length. Roughly speaking, this means that the ball around the origin contains a linear number of elements along each of the $a$- and $b$-axes, but a quadratic number along the $c$-axis. Picturing the ball as a sort-of distorted octohedron around the origin, you might be persuaded that it contains something on the order of $n^4$ elements. A much more precise and convincing treatment of the growth of nilpotent groups can be found in Mann's [How Groups Grow](https://books.google.co.uk/books/about/How_Groups_Grow.html?id=IGSVDLUsKhUC&redir_esc=y). Two particularly important results are Bass and Guivarc'h's formula relating the growth of nilpotent groups to the their algebraic structure, and Gromov's seminal Theorem which asserts that the groups with polynomial growth function are exactly the nilpotent groups and their finite extensions.

Calculating the exact terms of the growth function is a different matter to making asymptotic estimates. One approach is through the *growth series*, the generating function associated to the sequence of growth values. There is plenty of literature on this topic (including some of my own, ahem). Stoll's paper [*Rational and transcendental growth series for the higher Heisenberg groups*](https://link.springer.com/article/10.1007/s002220050090) is a great start for further reading.

### Brute force calculation
As an exercise, we'll try to find precise values of the growth function for small $n$. A paper of [Blachère](https://www.semanticscholar.org/paper/Word-distance-on-the-discrete-Heisenberg-group-Blachere/e5a76207e21df5c30a8803f7a5efc178478a8969) provides a formula for the word length of any element of $H_1$, but to my knowledge there is no known formula for general $r$. That might be a doable research project which would be worth publishing... 

In the absence of a clever way to find the lengths of elements, we will calculate the growth function by brute force. Given the ball of radius $n$, we can multiply every element by every generator (remember this is just matrix multiplication) to produce a set of potentially new elements. The ones that are truly new and not already in the ball somewhere are precisely the sphere of radius $n+1$. So we record the number of these truly new elements, update the ball to include them, and repeat ad infinitum. Well, until we run out of (human) patience or (computer) memory. The ball (and sphere) of radius $0$ just contains the identity, the element represented by the 'empty word', the unique word of length $0$

When we check that our new elements are really new, we'd best have the ball stored as a set, so that python can check efficiently if the new elements are in there. To form them into a set, we need the elements to be hashable. ImmutableMatrix from sympy gives us a matrix which is immutable and therefore hashable, and we can carry out the calculations at the level of matrix multiplication.

### Tuples

These sets get big pretty quickly, (well *actually* they only grow polynomially so *actually* they get big slowly but *actually* that's still fast enough to be a problem) so the calculation slows down fast as we increase $r$ and $n$. We also run into memory problems. To give us a tiny bit more headroom, we can store the elements in a more efficient way than as matrices. Note that we only really need to know the entries in the top row and the last column, since everything else is fixed (and in fact the first entry of the top row and the last entry of the last column are always 1). So we could store elements as $(2r+1)$-tuples, which is certainly more space-efficient than $(r+1)\times(r+1)$ matrices. Integers are hashable so tuples of integers are also hashable, so we're safe on the set front.

To save having to convert to matrices to do the calculations, we can define the group operation on tuples. Staring at the matrix representation for a moment should convince you that multiplying $(i_1,\ldots,i_r,j_1,\ldots,j_r,k)$ by $(l_1,\ldots,l_r,m_1,\ldots,m_r,n)$ gives 

$$(i_1+j+1,\ldots,i_r+j_r, k+n+j_1m_1+\cdots+j_rm_r).$$

We can implement this as follows:
```python
def tupleMult(g,h):
    r = (len(g)-1)//2
    return tuple([g[i]+h[i] for i in range(len(g)-1)]) + tuple([g[-1]+h[-1]+sum([g[r+i]*h[i] for i in range(r)])])
```
With an easy function `getGens` to produce all the generators and their inverses for given $r$, we can implement the brute force counting described above as follows:
```python
def growthHr(r, radius):
    
    ball = {tuple(0 for l in range(2*r+1))} # start with just the identity
    sphere = ball
    seq = [1]
    gens = getGens(r) # get the generators and inverses as tuples
    
    for _ in range(radius):
        newelts = {tupleMult(g,gen) for g in sphere for gen in gens} # potential new elements
        sphere = {new for new in newelts if new not in ball} # actual new elements
        
        ball.update(sphere) # new ball 
        seq.append(len(sphere)) # record the sphere size

    return seq
```
We're using list comprehension (the Python equivalent of what some maths textbooks would call 'set-builder notation') because its quicker than nested loops, allowing python to be more efficient. 

The full code is on [GitHub](https://github.com/aevetts/heisenberg/blob/main/heisenberggrowth.py).


### OEIS
Now we have a nice* piece of code that returns the beginning of the growth function for any $r$ that we like, until we run out of memory. A useful exercise for me, but potentially helpful for others. To make this even more public than my (obviously widely read) blog, I submitted some sequences to the [Online Encyclopedia of Integer Sequences (OEIS)](https://oeis.org/).

Perhaps unsurprisingly, the growth of the first Heisenberg group is already there (as sequence [A063810](https://oeis.org/A063810)). No higher Heisenberg groups appeared, though, which gave me the chance to submit my first ever sequences! You can see them now, beginning at [A396031](https://oeis.org/A396031).

*I mean, it's OK.

### Conclusion

We've learned a little bit about growth in the Heisenberg groups, and something about hashing and sets in Python. This has been a fun excuse to ramble on about my favourite kinds of groups, get some sequences onto the OEIS, and sharpen up my Python skills in the process.

Some plans for the future:
 - Adding higher Heisenberg groups to the normal form calculator app we built last time.
 - Coding up a word length calculator, or perhaps a utility to check if words are geodesic.
 - A discussion of other, more exotic, kinds of growth in these and similar groups.

