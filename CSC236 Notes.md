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