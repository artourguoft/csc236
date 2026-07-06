# <u>Recursive Correctness</u>
Before proving anything, we visualize recursive function calls with **call trees** and **traces**; these can use the function arguments (if the function takes arguments) or return values (if non-deterministic or does not take arguments) to make the chain of calls clear
- It is easiest to list the calls in execution order, ie. fully trace each recursive call until it returns, before moving on to the next (this also allows you to correctly catch infinite recursion before assuming that later code is even reached)
- Trace through the entire chain first (determine all arguments), then evaluate the return values for each

To prove **recursive correctness**:
1. **Define a size function** for the inputs; generally for a function `func(n)` take `size(n) = n`
2. **Valid measure**; show why this measure is **always in** $\mathbb{N}$ (usually, simply by the function preconditions) and is **always defined** for values that satisfy the function preconditions
3. **Assume the Recursive Hypothesis**; that the return of every recursive call satisfies the function postconditions
4. **Correct base case return values**; show that the static return values of the base cases **satisfy the function postconditions**
5. **Conditions** for recursion; deduce facts about input required for execution to go past base cases, ex. a new lower bound on the input since it still satisfies the preconditions but now also explicitly doesn't satisfy the base cases
6. **Valid arguments** in recursive calls; show that the now modified arguments in the recursive call **still satisfy all preconditions**
7. **Valid recursion**; show that **size is strictly decreasing** in the recursive call, ie. arithmetic proof of simplest example `size(n-m) < size(n)`
8. **Correct final return value**; given the assumed Recursive Hypothesis (that return values of all recursive calls satisfy the postconditions), show how the call in context finally returns a value that satisfies the postconditions as well (use the RH similar to how you would use an IH)
9. **Valid non-recursive calls**; throughout the entire function show that each non-recursive call (even including control flow and operators!) is valid, ie. is being called with the correct types, returns the correct types, etc.
# <u>Mathematical Induction</u>
**Simple Induction:** $P(0)∧(\forall k\in \mathbb{N},P(k)\implies P(k+1))\implies \forall n\in \mathbb{N},P(n)$
- The key idea behind Simple Induction is the ability to break down a problem of size $k+x$ into something **exactly** $x$ **size smaller**, which then allows us to use the **induction hypothesis** which here is $P(k)$
	- This is not always possible! A common pitfall of Simple Induction is that our induction hypothesis is not strong enough to still apply when we break an object down into problems $x$ size smaller, consider an example:
		- We have an induction hypothesis about full binary trees $P(nodes-1)\implies P(nodes)$; but removing a node from a full binary tree makes the tree not full, thus we cannot use it in the predicate $P$
	- The solution is to either look at Complete Induction as we will show below, or to try to do the entire induction proof on a stronger predicate
- Note that the relative size of the problems **does not have to mean the number** $1$; Simple Induction can also be used to prove statements for all naturals that are multiples of $5$ for example, in which we would look at $k+5$
- **Bases greater than** $0$ need to be explicitly included in the induction hypothesis, but of course they also change the definition of what we are proving by the principle of induction, ex.: $P(2)\wedge(\forall k\in \mathbb{N}_{\geq 2},P(k)⇒P(k+1))\implies \forall n\in \mathbb{N}_{\geq 2},P(n)$
- **Multiple bases** must be included in some scenarios, for example if we are trying to prove a predicate for all naturals but our IH only proves $\forall k\in \mathbb{N},P(k)\implies P(k+3)$, then we needed base cases $P(0)\wedge P(1)\wedge P(2)$
- When the IH uses $k$ in the conclusion and an expression that **decrements** $k$ **in the antecedent**, be careful that your IH always has **recourse to a base case** by selecting an appropriate **lower bound**, ex.:
	- $P(1)\wedge (\forall k\in \mathbb{N}_{\geq 1},P(k-1)\implies P(k))\implies \forall n\in \mathbb{N}_{\geq 1},P(n)$ is incorrect; IH instantiates $P(0)\implies P(1)$ for $k=1$
	- $P(1)\wedge(\forall k\in \mathbb{N}_{\geq 2},P(k-1)\implies P(k))\implies \forall n\in \mathbb{N}_{\geq 1},P(n)$ is correct
- When working with complicated step sizes, ensure that your IH always has **recourse to a base case** by selecting an appropriate lower bound and / or **multiple base cases**, for ex.:
	- Assume undefined $P(0),P(1)$, so want to prove $\forall n\in \mathbb{N}_{\geq 2},P(n)$ with inductive step $P\left( \left\lceil  \frac{k}{3}  \right\rceil \right)\implies P(k)$
		- It would not suffice to set $\forall k\in \mathbb{N}$ because for $k=2,3$ we get $\left\lceil  \frac{2}{3}  \right\rceil,\left\lceil  \frac{3}{3}  \right\rceil=1$ which gives us $P(1)\implies P(2)$ and $P(1)\implies P(3)$, but $P(1)$ is undefined and out of scope of the proof!
			- The correct structure then is $P(2)\wedge P(3)\wedge(\forall k\in \mathbb{N}_{\geq 4},P\left( \left\lceil  \frac{k}{3}  \right\rceil \right)\implies P(k))\implies \forall n\in \mathbb{N}_{\geq 2},P(n)$, start with base $P(2)$ and add more cases until the IH always instantiates to a base case in the antecedent, here that is $k=4$ since $\left\lceil  \frac{4}{3}  \right\rceil=2$

**Complete Induction:** $[\forall n\in \mathbb{N},(\forall k\in \mathbb{N}_{<n},P(k))\implies P(n)]\implies \forall n\in \mathbb{N},P(n)$
- We have seen that it is not always possible to link problems with size $n$ to problems with just size $n-x$; this is where Complete Induction comes in; now our induction hypothesis assumes the predicate is true for all $k<n$
	- Note the IH here is $(\forall k\in \mathbb{N}_{<n},P(k))$; we are assuming that $P$ is true for all $k<n$ and need to prove that this implies $P(n)$ for a fixed $n$, this is different from Simple Induction wherein the IH was simply $P(k)$ for a fixed $k$
- Consider the lack of an explicit base case; if we expand out the antecedent in the principle of Complete Induction, the first term would be $\forall k\in \mathbb{N}_{<0},P(k)\implies P(0)$, within which the antecedent is vacuously true, so it is equivalent to $P(0)$
	- Then we consider that $P(0)$ will be implicitly clear in the proof; an example is the prime factorization proof by induction where instantiations with a prime number are all one special implicit base case; in such proofs, we simply set up $n$ and $k$, and assume the IH
- However, **usually an explicit base case is needed**, so we prove $P(0)$ directly as a base case, then the full statement is: $P(0)\wedge(\forall n\in \mathbb{N},(\forall k\in \mathbb{N}_{<n},P(k))\implies P(n))\implies \forall n\in \mathbb{N},P(n)$
	- In such proofs, we set up $n$ and prove the base cases directly; after that we have $n$ greater than the largest base case, and now we can set up $k$s and show their validity, then we can assume the IH, etc.
- When proving a predicate for **naturals greater than or equal to some constant** $c$, we prove $P(c)$ and any other necessary base cases directly, then we assume in the IH that the predicate holds for all $c\leq k<n$
	- The whole statement becomes $P(c)\wedge(\forall n\in \mathbb{N}_{\geq c},(\forall k\in \mathbb{N},(c\leq k<n)\implies P(k))\implies P(n))\implies \forall n\in \mathbb{N}_{\geq c},P(n)$
	- In the inductive step, the key is the **decomposition** of $n$ into smaller elements (ex. $k=n-3$), ie. finding valid smaller sizes of the problem to use the IH with
		- Once we determine this decomposition, we must demonstrate for the $k$s that we use that $c\leq k<n$ and that $k\in \mathbb{N}$, and only then can we apply the IH to prove the implication of $P(n)$
	- **Additional base cases** may be needed, depending on the nature of our decomposition:
		- First, we need $c\leq k$, so if we are using a result of our assumed IH such as ex. $P(k-j)$, then we need $c\leq k-j$, so $c+j\leq k$, thus we need to prove cases $c$ to $c+j$ separately as base cases
		- Then, we need $k<n$, so if we are using a result of our assumed IH such as $P\left( \left\lceil  \frac{n+1}{2}  \right\rceil \right)$, we notice that $\left\lceil  \frac{n+1}{2}  \right\rceil\geq n$ for $0\leq n<3$, so we need to prove cases $P(0)\wedge P(1)\wedge P(2)$ as base cases
		- Note, these considerations depend on what relationship we are actually using in the arithmetic of the proof itself
			- Sometimes there is **no strict relation**, ex. binary trees and height, we may take the two subtrees $t_{l},t_{r}$ of an arbitrary tree of $h\geq 2$ and apply the IH since we know $h_{t_{l}},h_{t_{r}}<h$ but we don't know the exact difference
	- The statement of induction sometimes cannot take all of this into account initially; the necessary base cases and the decomposition of $n$ will become elucidated as we try to use our IH to prove $P(n)$
# <u>Structural Induction and Recursively Defined Sets</u>
Induction can also be used to **define sets**:
- First, define the **basis** which is the **smallest or simplest object** (or multiple objects!) in the set
- Then, define the induction step which defines the **recursive rule** (or multiple rules!) of how larger or more complicated objects in the set can be constructed from the smaller or simpler objects
- We state the set we are defining is the smallest set such that the rules above are followed, meaning that it contains only elements that satisfy these rules and no others; ie. it is the intersection of all possible sets that satisfy these rules
	- This is a key part of why we can use the principle of structural induction which implies a claim about all members of a set
So far, we have used induction only over $\mathbb{N}$, but it is no coincidence that the set of naturals can also be defined as described above, consider:
- $0\in \mathbb{N}$
- $n\in \mathbb{N}\implies n+1\in \mathbb{N}$
Once we have a recursively defined set, we can **recursively define functions** which **take that set as their domain**
- The format is the same, wherein we define the function explicitly for the bases and then recursively for remaining cases

Logically, we want to **prove properties about sets defined by recursion, by using induction**; this is called **Structural Induction**
- Consider a set $X$ defined recursively, and a property $P$ that we want to prove applies to all elements of $X$, then:
	- We prove that the basis elements satisfy $P$
	- Then, we prove that **each** of the finitely many ways of constructing new objects with the recursive rules in the induction step preserves property $P$; that is, if arbitrary elements of $X$ satisfy $P$, then so does the result formed from combining them by a recursive rule
		- This is equivalent to the induction step from previous forms of induction; we take arbitrary elements of $X$ and assume they satisfy $P$ (which is equivalent to assuming the IH), and prove that $P$ is preserved under **each** of the recursive rules
		- These proofs often introduce multiple arbitrary elements (since further elements are made by combining others); so, **recall that multiple variables can instantiate the same value** unless explicitly stated .: $x_{1}\neq x_{2}$

**Binary trees** can be represented as recursively defined sets, where the **base objects** are represented as **single node trees** or **empty trees** (depending on the problem), and constructed objects are larger trees whose subtrees are the simpler objects
- Our formatting:
	- Empty tree is $t_{e}$
	- Single node is $(t_{e},t_{e})$
	- Tree with subtrees $t_{l},t_{r}$ is $(t_{l},t_{r})$
- We define the set of all **binary trees** $T$ recursively as follows:
	- The basis will be the **empty tree**: $t_{e}\in T$
	- The recursive rule is: $\forall t_{l},t_{r}\in T,(t_{l},t_{r})\in T$, and this implicitly allows for $t_{l},t_{r}=t_{e}$ since we chose the empty tree as a base case, so we don’t have to explicitly handle the single node scenario $(t_{e},t_{e})$, nor the one empty subtree scenario; they are handled by this recursive rule
- Essentially, we **recursively build the tree upward from its empty tree leaves**!
	- Then we can use this recursive structure to define **size and height functions recursively** as well:
		- $s(t_{e})=0$, and $\forall t_{l},t_{r}\in T, s((t_{l},t_{r}))=s(t_{l})+s(t_{r})+1$
		- $h(t_{e})=0$, and $\forall t_{l},t_{r}\in T,h((t_{l},t_{r}))=\max(h(t_{l}), h(t_{r}))+1$
- Then, **Structural Induction** over the set of all binary trees then takes the form:
	- $P(t_{e})\wedge[\forall t_{l},t_{r}\in T,(P(t_{l})\wedge P(t_{r}))\implies P((t_{l},t_{r}))]\implies \forall t\in T,P(t)$
		- We prove that the basis element (the empty tree $t_{e}$) satisfies $P$
		- Then, we assume two arbitrary trees which satisfy $P$ and prove that under the one recursive rule for creating all other objects in the set of binary trees, the property $P$ is maintained
		- If there are **multiple recursive rules** (ex. one building from one arbitrary element, another building from multiple), then we also get **multiple IHs** and in turn prove each separately
# <u>Well-Ordering Principle</u>
**Well-Ordering:** any **nonempty** subset of $\mathbb{N}$ contains a minimum element; ie. for any $A\subseteq \mathbb{N}$ such that $A\neq \emptyset$, we have $\exists a\in A,\forall a'\in A,a\leq a'$
- Well-ordering applies to all nonempty subsets of $\mathbb{N}$, including infinite subsets
- Well-ordering does not apply to all subsets of $\mathbb{Z}$ or $\mathbb{R}$; ex. $\{ \dots,-2,-1 \}$, or the set of $\mathbb{Q}$ between $0$ and $1$ (despite this set having an infimum $0$), so the property is not as trivial as it seems
- When using the WOP in proofs, we usually prove an existential on a universal variable, so we want to show that the **minimum of some set** can be used as the **existentially quantified variable** to satisfy what we want to prove:
	- Define a set of quantities for which we want to find a minimum **for each instance** of the universally quantified variable; note, we **cannot** simply have the relationship that we want to prove as the set restriction!
		- But we can (and almost always do) have part of the required relationship as a part of the set definition, ex. the upper bound of an inequality, then prove that the minimum of this set must also satisfy the lower bound
	- Justify why that **set is not empty for all instances** of the universal, since the existential depends on the universal here
		- Usually by simply showing that there is an element **equal to the universally quantified variable** in the set, or $0$, etc.
	- Recall, the **minimum is also an element of the set** and thus must satisfy the set restriction
- The last step is to prove the facts that we need about the minimum; usually this takes one of two forms:
	- The **contrapositive** form where we use the fact that elements less than the minimum are by definition not in our set and thus have the negation of the properties in the set restriction
		- Ex., for proving forms like $\forall n\in \mathbb{N}_{+},\exists k\in \mathbb{N},2^{k-1}\leq n < 2^k$  we define a set $S_{n}=\{ s \in \mathbb{N}:n<2^s \}$, then by definition $S_{n}\subseteq \mathbb{N}$ and $S_{n}\neq \emptyset$ since we always have $n\in S_{n}$ since $\forall n\in \mathbb{N}_{+},n<2^n$, and thus by WOP we know there exists a minimum element $s_{m}\in S_{n}$
		- Then by contrapositive $(k\in S\iff n<2^k)\equiv (n\geq 2^k\iff k\not\in S)$, and $s_{m}-1<s_{m}\implies s_{m}-1 \not\in S\implies n\geq 2^{s_{m}-1}$, thus we take $k=s_{m}$ for each $S_{n}$
		- Note, lower bound cases like $s_{m}=0$ might have to be handled directly and explicitly since we cannot subtract from $0$ and still be in $\mathbb{N}$, and then the case $s_{m}\geq 1$ can be handled generally
	- The **contradiction** form where we need the minimum to be equal to or less than or greater than another quantity, so suppose the negation and show that it leads to a contradiction; ie. we can find a smaller element
		- Ex., for proving forms like $\forall n\in \mathbb{N}_{+},\exists s,t\in \mathbb{N},n=s^2+t\wedge t\leq 2s$ we define a set $T_{n}=\{ t\in \mathbb{N}: \exists s,n=s^2+t\}$, then by definition $T_{n}\subseteq \mathbb{N}$ and $T_{n}\neq \emptyset$ since we always have $n\in T_{n}$ since $n=0^2+n$, and thus WOP we know there exists a minimum element $t_{m}\in T_{n}$
		- Assume for contradiction that $t_{m}>2s$, then for $s'=s+1$ we have $n=(s')^2+t_{m}=(s+1)^2+t_{m}=s^2+2s+1+t_{m}$ which must mean $a$
# <u>Iterative Correctness</u>
A