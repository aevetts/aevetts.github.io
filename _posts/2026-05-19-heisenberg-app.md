---
layout: single
title:  "Heisenberg groups 1: Normal forms"
permalink: /heisenbergnf/
---

<!-- 
<script type="text/javascript" async
     src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-mml-chtml.js">
</script> -->


I've been meaning to write some code to deal with calculations in finitely generated nilpotent groups for ages. I've finally got round to it, so I thought I'd write down a few things about what I've been doing. In this first blog post, I'll define the Heisenberg group, and describe the calculation to put words into normal form.

I've made this into a Streamlit app, which you can find [here](https://heisenbergnf.streamlit.app/).

### The Heisenberg group
We're talking about the *integer* or *discrete* Heisenberg group here. This is the group with presentation $H_1 = \langle a,b \mid [[b,a],a]=[[b,a],b]=1\rangle$, the free nilpotent group of rank 2 and class 2. 

### Normal form
We can rewrite this to an equivalent presentation by setting $c=[b,a]$, so we get $\langle a,b,c \mid c=[b,a], [c,a]=[c,b]=1\rangle$. The relation $[b,a]=c$ can be rewritten as $ba=cab$, and then $ba=abc$ since $c$ commutes with both $a$ and $b$. This means that any word over the generators $\{a,b,c\}$ can be rearranged, moving all the $b$s to the right of all the $a$s, and adding appropriate powers of $c$. So every group element has the form $a^ib^jc^k$ for some integers $i,j,k$ (and it is straightforward to prove that no pair of distinct words in this form represent the same element).

This is what's known as a *normal form* for a group. Formally, a normal form is a section of the evaluation map $\\{a,b,c\\}^*\to H_1$. Informally, it is a choice of representative word for every group element. The bottom line is that an element of $H_1$ is determined by a triple of integers $(i,j,k)$.

We would like to be able to easily rewrite any word into its normal form. Rearranging $bab$ into $ab^2c$ is easy, but rearranging $b^2ac^{-1}b^{-2}acb^{-1}a^{-1}bc^2a^2b$ by hand feels like a recipe for arithmetical errors. So lets write a program that does it for us.

We could do this rewriting using string manipulations: scan the word for the first instance of letters out of order, rearrange them, and rescan. But this is slow. So we make use of some algebra.

### Matrix representation
Here is another group, this time given as a set of matrices. You can either believe me that it is a group, or check that it is closed under the usual matrix multiplication and inverse operations, making it a subgroup of $\mathrm{GL_3(\mathbb{Z})}$.

$$G = \left\{ \begin{pmatrix} 1 & x & z \\ 0 & 1 & y \\ 0 & 0 & 1 \end{pmatrix} \colon x,y,z\in\mathbb{Z} \right\}$$ 

So an element is determined by the triple $(x,y,z)$. Seems familiar.

Lets define a map $\theta\colon H_1\to G$ via 

$$a^ib^jc^k\to \begin{pmatrix} 1 & j & k \\ 0 & 1 & i \\ 0 & 0 & 1 \end{pmatrix}$$ 

(I know, I know, the $i$ and $j$ look wrong, but I promise it works out).

We can check that this is a homomorphism (that the map preserves the group structure) by carrying out the following calculations:

$$a^ib^jc^k\cdot a^lb^mc^n = a^{i+l}b^{j+m}c^{k+n+jl}$$

$$\begin{pmatrix} 1 & j & k \\ 0 & 1 & i \\ 0 & 0 & 1 \end{pmatrix}\begin{pmatrix} 1 & m & n \\ 0 & 1 & l \\ 0 & 0 & 1 \end{pmatrix} = \begin{pmatrix} 1 & j+m & k+n+jl \\ 0 & 1 & i+l \\ 0 & 0 & 1 \end{pmatrix}$$

and if we agree that every element is given by a triple, and every triple defines an element, we agree that $\theta$ is surjective. Furthermore, if we agree that the group elements in each case are uniquely determined by the triples $(i,j,k)$ and $(x,y,z)$, respectively, we also agree that this homomorphism is injective. So $G$ is isomorphic to $H_1$, and we have a new way of expressing the Heisenberg group.



### Calculation
The point of this second point of view on $H_1$ is that matrix multiplication is very well-understood, and implemented efficiently in Python (and everywhere else). Our piece of code will take as input any word over the generators (and their inverses), find the matrix representation of the element by multiplying the matrix for each letter in turn, then convert back into a normal form word for the output.

For reasons explained in a future post, we use sympy's ImmutableMatrix and set up each generator as a matrix. For ease of typing and reading, we'll use capitals to denote inverses, writing $A$ for $a^{-1}$ etc. We also set up a dictionary to convert the generators letters $\\{a,A,b,B,c,C\\}$ to the appropriate matrix.

```python
from sympy import ImmutableMatrix

a = ImmutableMatrix([[1, 0, 0], [0, 1, 1], [0, 0, 1]])
b = ImmutableMatrix([[1, 1, 0], [0, 1, 0], [0, 0, 1]])
c = ImmutableMatrix([[1, 0, 1], [0, 1, 0], [0, 0, 1]])

gen_dict = {'a': a, 'b': b, 'c': c, 'A': a**-1, 'B': b**-1, 'C': c**-1}
```

The key part of the code is converting a word to a matrix. This just requires starting with the identity matrix and then looping through the letters of the word, post-multiplying by the appropriate matrix each time. The matrix we're left with is exactly the matrix representation of the original word.

```python
def wordToMatrix(word):
    result = ImmutableMatrix([[1, 0, 0], [0, 1, 0], [0, 0, 1]])
    for char in word:
        result = result * gen_dict[char]
    return result
```

To display the result, we just use an ugly series of if statements to convert positive and negative integers to powers of the appropriate generator, outputting a word in the form $a^ib^jc^k$ (or $A^ib^jC^k$, etc).

This code is all wrapped up nicely as a Streamlit app, and you can play with it [here](https://heisenbergnf.streamlit.app/) or see the code itself on [GitHub](https://github.com/aevetts/heisenberg).

### Next Time
The Heisenberg group is denoted $H_1$ here for a reason. In the next post I'll talk about the *higher* Heisenberg groups, and calculating the values of their *growth function*.



