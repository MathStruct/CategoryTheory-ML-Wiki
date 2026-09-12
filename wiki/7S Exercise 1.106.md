#exercise #solution

**Exercise 1.106.** With $g : S \to T$ as in Example 1.102: choose a nontrivial $c$ on $S$, compute $g_!(c)$; choose $d$ coarser than $g_!(c)$ and $e$ not coarser; compute $g^*(d), g^*(e)$; check $c \leq g^*(d)$ and $c \not\leq g^*(e)$.

## Solution

Take $c = (13)(2)(4)$; then $g_!(c) = (12\,3)(4)$. Let $d = (12\,3\,4)$ (coarser) and $e = (12)(34)$ (not coarser). Then $g^*(d) = (1234)$ and $g^*(e) = (12)(34)$. Indeed $c \leq g^*(d)$, but $c \not\leq g^*(e)$ since $1 \sim_c 3$ while $1 \not\sim 3$ in $(12)(34)$ — consistent with the [[Galois Connection]] formula $g_!(c) \leq d \iff c \leq g^*(d)$.
