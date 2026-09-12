#exercise #solution #proof

**Exercise 1.16.** Suppose $\{A_p\}_{p \in P}$ and $\{A'_{p'}\}_{p' \in P'}$ are [[Partition|partitions]] of $A$ such that for each $p$ there is $p'$ with $A_p = A'_{p'}$. 1. Show there is at most one such $p'$. 2. Show that for each $p'$ there is a $p$ with $A_p = A'_{p'}$.

> 7 Sketches §1.2.1.

## Solution

1. If $A_p = A'_{p'_1} = A'_{p'_2}$ then $A'_{p'_1} \cap A'_{p'_2} = A'_{p'_1} \neq \varnothing$; by the partition axiom distinct labels have disjoint parts, so $p'_1 = p'_2$.
2. Pick $a \in A'_{p'}$ (parts are nonempty). Since $A = \bigcup_p A_p$ there is $p$ with $a \in A_p$, and by assumption $A_p = A'_{p''}$ for some $p''$. Then $a \in A'_{p'} \cap A'_{p''}$, so $p' = p''$ and $A_p = A'_{p'}$.

Hence "same partition up to relabeling" is a well-defined notion.
