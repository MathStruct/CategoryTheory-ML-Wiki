#exercise #solution

**Exercise 2.36.** $\mathrm{Prop}_{\mathbb{N}}$ is the set of statements about a natural number, with $P \leq Q$ iff $P(n) \Rightarrow Q(n)$ for all $n$. Define a monoidal unit and product satisfying Definition 2.2.

## Solution

Take unit $\mathsf{true}$ ("$n$ is a natural number") and product $\wedge$: $(P \wedge Q)(n)$ true iff both are. Alternatively unit $\mathsf{false}$ ("$n$ is made of cheese") and product $\vee$. Both give [[Symmetric Monoidal Preorder|symmetric monoidal preorders]] (compare [[Closure Operator|modal operators]] in Example 1.123).
