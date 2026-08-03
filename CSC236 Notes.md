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
- A common arithmetic relationship that is used in WOP proofs: $\forall m,n\in \mathbb{N}, m<n\implies (m+1\leq n)\wedge(m\leq n-1)$
- When using the WOP in proofs, we usually prove an existential on a universal variable, so we want to show that the **minimum of some set** can be used as the **existentially quantified variable** to satisfy what we want to prove:
	- Define a set of quantities for which we want to find a minimum **for each instance** of the universally quantified variable; note, we **cannot** simply have the relationship that we want to prove as the set restriction!
		- But we can (and almost always do) have part of the required relationship as a part of the set definition, ex. the upper bound of an inequality, then prove that the minimum of this set must also satisfy the lower bound
	- Justify why that **set is not empty for all instances** of the universal, since the existential depends on the universal here
		- Usually by simply showing that there is an element **equal to the universally quantified variable** in the set, or $0$, etc.
	- Recall, the **minimum is also an element of the set** and thus must satisfy the bounds of the set restriction; this becomes important for the **lower bound** since we are working with a minimum
		- In most cases we can explicitly reason about a specific lower bound; then in the remainder of the proof we can deduce whether any new minimums we find are still in $\mathbb{N}$ or not as we go based on that lower bound
- The last step is to prove the facts that we need about the minimum; usually this takes one of two forms:
	- The **contradiction** form where we assume that the minimum satisfies the negation of the property we want to prove (**not** the set restriction!), and show that it leads to a contradiction; ie. we can find a smaller element than the supposed minimum (that is still in $\mathbb{N}$ and satisfies the set restriction)
	- The **contrapositive** form where we prove a lemma that for any element of the set restriction that satisfies the negation of the property we want to prove (**not** the set restriction) we can find a smaller element, and by contrapositive of that lemma we conclude that the minimum satisfies the desired property
- Ex., for $\forall n\in \mathbb{N}_{+},\exists k\in \mathbb{N},2^{k-1}\leq n < 2^k$ we define a set $S_{n}=\{ k \in \mathbb{N}:n<2^k \}$, then by definition $S_{n}\subseteq \mathbb{N}$ and $S_{n}\neq \emptyset$ since we always have $n\in S_{n}$ since $\forall n\in \mathbb{N}_{+},n<2^n$, and thus by WOP we know there exists a minimum element $k_{m}\in S_{n}$ where $n<2^{k_{m}}$
	- We know $k\neq 0$ since $2^k=2^0=1$ and $n\in \mathbb{N}_{+}$ so we cannot have $n<1=2^k$; so $k\geq 1$
	- By contradiction: assume that $n< 2^{k_{m}-1}$, but $k_{m}-1\in \mathbb{N}$ since $k_{m}\geq 1$ and by definition $k_{m}-1\in S$; but since $k_{m}-1<k_{m}$ this contradicts that $k_{m}$ is the minimum of $S$, and thus it must be that $2^{k_{m}-1}\leq n$
	- By contrapositive: we start by proving the lemma $\forall k\in S_{n},n<2^{k-1}\implies \exists k'\in S_{n},k'<k$ by taking $k'=k-1$, where $k-1\in \mathbb{N}$ since $k\geq 1$ and $k-1\in S_{n}$ by definition, then the contrapositive is $\forall k\in S_{n},\forall k'\in S_{n},k'\geq k \implies n\geq 2^{k-1}$ as needed
- Ex., for $\forall n\in \mathbb{N}_{+},\exists s,t\in \mathbb{N},n=s^2+t\wedge t\leq 2s$ we define a set $S_{n}=\{ t\in \mathbb{N}: \exists s \in \mathbb{N},n=s^2+t\}$, then by definition $S_{n}\subseteq \mathbb{N}$ and $S_{n}\neq \emptyset$ since $n\in S_{n}$ from $n=0^2+n$, and thus by WOP we know there exists a minimum element $t_{m}\in S_{n}$ where $\exists s_{m} \in \mathbb{N}$ where $n=s_{m}^2+t_{m}$
	- By contradiction: assume that $t_{m}>2s_{m}$, then $t_{m}\geq 2s_{m}+1$, then $n=s_{m}^2+(2s_{m}+1)+t_{m}-(2s_{m}+1)=(s_{m}+1)^2+(t_{m}-2s_{m}-1)$
		- But $t_{m}-2s_{m}-1\in \mathbb{N}$ since $t_{m}\geq 2s_{m}+1$, and then by definition $t_{m}-2s_{m}-1\in S_{n}$ with $s=s_{m}+1\in \mathbb{N}$, but $t_{m}-2s_{m}-1<t_{m}$ which contradicts the minimality of $t_{m}$, and thus it must be that $t_{m}\leq 2s_{m}$
# <u>Iterative Correctness</u>
First we introduce new terminology and two types of predicates, regarding iterative correctness proofs:
- **Initialization:** this is the code before the loop which generally sets up variables necessary for the loop
- **Condition:** this is the logical or arithmetic check which determines whether the loop iterates or not; we use the predicate $C_{k}(\dots)$ which is the condition right before the $k^{th}$ iteration of the loop
	- The correct predicate to use as the condition is explicitly clear in the code as the loop condition; generally $C(i):i< \text{len(L)}$ where $i$ is the loop variable and $\text{L}$ is some list
- **Iteration:** one execution of the entire loop body while the condition is true
- **Loop Invariant:** the predicate $I_{k}(\dots)$ is **true every time the loop condition is checked**
	- This is necessary for us to reason about **all** iterations of the loop; without something that is uniformly true among them we would need explicit values for each iteration
		- Thus a loop invariant is correct if it is **always true at the beginning of every loop** iteration, **including the loop check that fails**, causing the loop to terminate
		- This also means invariants **must not depend on the iteration number** $k$; they must only depend on program variables
	- Loop invariants are used to prove the **validity** of each loop step, and finally the **correct return value** in the end
		- Note, this often requires **separate loop invariants** to prove each requirement
		- These predicates may require some creativity, but usually:
			- To prove **validity**; we use an invariants $I^S$ and $I^V$ consisting of the **loop constraints and preconditions** respectively, needed for all operations in the loop body to be valid (ex. list indices remaining valid, etc.), most commonly:
				- $I^S:i\in \mathbb{N}\wedge i\in[0:\text{len(L)}]$ where $i$ is the loop variable and $\text{L}$ is a list indexed from $0$; this is **standard** for most proofs 
				- $I^V$: other variables which change in the loop, preconditions which may be affected by loop operations, etc. 
				- Once we prove these invariants are always true, we can take this together with $C$ to conclude that inequality comparisons, list indexing, incrementing, etc. are all valid for all loop iterations
			- To prove **correct return value**; we use an invariant $I^P$ consisting of the **postconditions** (more on this a bit later)
- **Loop Variant:** the loop variant $V_{k}$ is a function of some variable(s) in the iterative function which must always be a **natural number** and **strictly decrease** on each loop iteration
	- The loop variant is used to prove **termination**; that the loop does not run infinitely
	- We prove that the image of the $V_{k}$ function is a subset of $\mathbb{N}$ and is a strictly decreasing sequence, and thus we can invoke WOP to deduce that the loop cannot run forever
	- A common ex.; for a loop that checks `while (i < n)` we define `V = n - i`
		- A more complex ex.; for a loop that checks `while (a > 0 or b > 0)` and decrements either variable, we define `V = a + b`

The symbolic forms of these proofs:
$$
(\text{Pre}\implies I_{0})\wedge [\forall k\in \mathbb{N},(I_{k}\wedge C_{k})\implies I_{k+1}] \tag{Invariants}
$$
$$
(\text{Pre}\implies V_{0}\in \mathbb{N})\wedge [\forall k\in \mathbb{N},(V_{k}\in \mathbb{N}\wedge I_{k}\wedge C_{k})\implies V_{k+1}\in \mathbb{N}\wedge V_{k}>V_{k+1}] \tag{Variants}
$$
Now the Simple Induction that we perform in these proofs becomes clear:
- For **invariants** (both validity and correct return values!):
	- Base Case: $\text{Pre}\implies I_{0}$
	- Induction Hypothesis: $I_{k}\wedge C_{k}$
- For **variants**:
	- Base Case: $\text{Pre}\implies V_{0}\in \mathbb{N}$
	- Induction Hypothesis: $V_{k}\in \mathbb{N}\wedge I^V_{k}\wedge C_{k}$
- The preconditions implying the invariants and variant immediately **before the first loop iteration** are the base cases, and the states of the predicates before some arbitrary loop iteration are the induction hypotheses
	- Note that the variant proof may rely on an already proven validity invariant!
Note, these are only **proofs that the invariants and variant are always true** whenever the condition is checked; **direct proofs** of validity and termination (and correctness; more on that just below) themselves follow these proofs!

With validity and termination proven, we prove that the function ultimately has a **correct return value(s)**, ie. that it satisfies the postconditions
- This follows directly from the fact that upon loop termination we have $I^S,I^V$ and $I^P$ (by invariant proofs) and $\neg C$ (by definition), from which we can conclude that the postconditions are satisfied directly (assuming we designed a good invariant!)
$$
I^S\wedge I^V\wedge\neg C\wedge I^P\implies \text{Post} \tag{Correct Return Values}
$$
At the start of the $k^{th}$ loop iteration, we notice that the postcondition is satisfied for the slice $\text{L}[0:i_{k}]$; thus we usually define $I^P_{k}(i): \text{Post(L}[0:i_{k}])$
- Then, on termination of the loop we have from $I^S$ that $i\leq \text{len(L)}$, and from $\neg C$ that $i\geq \text{len(L)}$, which together imply $i=\text{len(L)}$, from which we directly have $I^P(\text{len(L)})\equiv \text{Post(L}[0:\text{len(L)}])$; ie. the postconditions are satisfied for the entire input list as needed
- Note, we assume Python list slicing conventions where the slice range is **exclusive** of the upper bound despite notation implying otherwise!
# <u>Runtime of Recursive Algorithms</u>
A
# <u>Regular Languages and Finite Automata</u>
Definitions related to strings:
- **Alphabet:** a **finite** set of symbols, denoted $\Sigma$
- **String:** a **finite** sequence of symbols over $\Sigma$
	- The **empty string** denoted $\epsilon$ is the sequence of zero symbols
	- The **infinite** set (allowing for repeated symbols) of **all strings** over an alphabet $\Sigma$ is denoted $\Sigma^*$ 
	- The **length** of a string $w\in\Sigma^*$ is the number of symbols appearing the string
		- The **finite** set of **all strings of length** $n$ over $\Sigma$ is denoted $\Sigma^n$, from which it follows that:
			- $|\Sigma^n|=|\Sigma|^n$
			- $\Sigma^0=\{ \epsilon \}$
- **Language:** a **finite or infinite** subset of strings over $\Sigma$; ie. $L\subseteq\Sigma^*$ or equivalently $L\in \mathcal{P}(\Sigma^*)$
	- **Union:** $L\cup M=\{ x \in\Sigma^* : x \in L\vee x \in M \}$
	- **Concatenation:** $LM=\{ xy\in\Sigma^* : x \in L\wedge y\in M \}$
	- **Kleene Star:** $L^*=\{ \epsilon \}\cup \{ x \in\Sigma^* : \exists w_{1},w_{2},\dots,w_{n}\in L,x=w_{1}w_{2}\dots w_{n}\text{ for some }n \}$
		- This is equivalent to $L^*=\{ \epsilon \}\cup L\cup LL \cup LLL\dots$, meaning this language can be split into smaller strings all of which are in $L$
		- This is analogous to the $\Sigma^*$ definition (with a language rather than an alphabet), from which it follows that this set is also **infinite**

Then **set of regular languages** over an alphabet $\Sigma$ is **recursively** defined as follows:
- $\emptyset$ and $\{ \epsilon \}$ are regular languages
- For any symbol $c\in\Sigma$, $\{ c \}$ is a regular language
- For regular languages $L,M$, all of $L\cup M,LM,L^*,M^*$ are also regular languages
Clearly regular languages are **finite or infinite** sets of strings, and many computer programs are algorithms which aim to **decide membership** in a particular regular language
- To this end, we would like a simple, computer-friendly representation of regular languages 
	- Then, we can input an arbitrary regular language and a string, and the computer determines whether the string is in the language or not
	- This is the idea behind **regular expressions** (regex)