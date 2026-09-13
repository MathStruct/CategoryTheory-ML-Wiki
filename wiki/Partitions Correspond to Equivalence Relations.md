#theorem #proof

**Proposition 1.19.** Let $A$ be a [[Set]]. There is a one-to-one correspondence between the ways to [[Partition|partition]] $A$ and the [[Equivalence Relation|equivalence relations]] on $A$.

> Source: 7 Sketches Proposition 1.19, Exercise 1.20.

## Proof

*Partition $\Rightarrow$ equivalence relation.* Given $\{A_p\}_{p \in P}$, define $a \sim b$ to mean $a$ and $b$ are in the same part: there is $p \in P$ with $a, b \in A_p$. Reflexivity, symmetry and transitivity are immediate ("$a$ is in the same part as itself", etc.).

*Equivalence relation $\Rightarrow$ partition.* Given $\sim$, call $X \subseteq A$ **$(\sim)$-closed** if $x \in X$ and $x' \sim x$ imply $x' \in X$, and **$(\sim)$-connected** if it is nonempty and $x \sim y$ for all $x, y \in X$. The parts are exactly the $(\sim)$-closed, $(\sim)$-connected subsets. That these form a partition is [[7S Chapter 1 Exercises#Exercise 1.20|7S Exercise 1.20]]: each part is nonempty by connectedness; two distinct parts are disjoint (if $a \in A_p \cap A_q$ then closedness and connectedness force $A_p = A_q$); and every $a$ lies in the part $\{a' \mid a' \sim a\}$.

The two constructions are mutually inverse. $\blacksquare$

The set of parts is the [[Quotient Set]] $A/\!\sim$. In Kittenlab's terms, $A/\!\sim$ is the set of *connected components* of the relation, computed by union-find.
