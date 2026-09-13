#solution #proof

**Solution to [[7S Exercise 1.16|Exercise 1.16]].**

1. If $A_p = A'_{p'_1} = A'_{p'_2}$ then $A'_{p'_1} \cap A'_{p'_2} = A'_{p'_1} \neq \varnothing$; by the partition axiom distinct labels have disjoint parts, so $p'_1 = p'_2$.
2. Pick $a \in A'_{p'}$ (parts are nonempty). Since $A = \bigcup_p A_p$ there is $p$ with $a \in A_p$, and by assumption $A_p = A'_{p''}$ for some $p''$. Then $a \in A'_{p'} \cap A'_{p''}$, so $p' = p''$ and $A_p = A'_{p'}$.

Hence "same partition up to relabeling" is a well-defined notion.

> Sources: 7 Sketches, Exercise 1.16 and Solution A.1.
