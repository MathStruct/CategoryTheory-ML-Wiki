#theorem #proof

**Proposition 1.78.** Let $P$ be a [[Preorder]]. [[Monotone Map|Monotone maps]] $P \to \mathbb{B}$ are in one-to-one correspondence with [[Upper Set|upper sets]] of $P$.

> Source: 7 Sketches Proposition 1.78, Exercise 1.79; Kittenlab Lecture 14 (subsets as characteristic functions).

*Proof.* Let $f : P \to \mathbb{B}$ be monotone. The subset $f^{-1}(\mathsf{true}) \subseteq P$ is an upper set: if $f(p) = \mathsf{true}$ and $p \leq q$ then $\mathsf{true} = f(p) \leq f(q)$, and in $\mathbb{B}$ only $\mathsf{true}$ is above $\mathsf{true}$, so $f(q) = \mathsf{true}$.

Conversely, for an upper set $U$ define $f_U(p) = \mathsf{true}$ iff $p \in U$. It is monotone: if $p \leq q$ then either $p \in U$, so $q \in U$ and $f(p) = \mathsf{true} = f(q)$, or $p \notin U$, so $f(p) = \mathsf{false} \leq f(q)$.

The two constructions are mutually inverse. $\blacksquare$

This is the preorder case of the correspondence between [[Subobject|subobjects]] and maps into a [[Subobject Classifier]]: $\mathbb{B}$ classifies subsets of sets (Kittenlab Lecture 14) and upper sets of preorders. Pulling back an upper set along a monotone map is precomposing its classifier ([[7S Chapter 1 Exercises#Exercise 1.79|7S Exercise 1.79]]). It is also the $\mathbb{B}$-enriched [[Yoneda Lemma]]: monotone maps $P \to \mathbb{B}$ are $\mathbf{Bool}$-[[Enriched Functor|functors]], i.e. $\mathbb{B}$-valued [[C-Set|copresheaves]].
