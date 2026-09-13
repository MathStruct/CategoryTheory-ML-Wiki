#definition

A [[Monotone Map]] $f : P \to Q$ **preserves meets** if $f(a \wedge b) \cong f(a) \wedge f(b)$ for all $a, b \in P$, and **preserves joins** if $f(a \vee b) \cong f(a) \vee f(b)$ for all $a, b$. (More generally, for all subsets: $f(\bigwedge A) \cong \bigwedge f(A)$.)

> Sources: 7 Sketches Definition 1.92, Proposition 1.111, Theorem 1.115, Exercise 1.94; DaoFP §10.7 (adjoints preserve (co)limits).

- Failure to preserve joins is a [[Generative Effect]] (Definition 1.93). Adam's thesis restricts to maps that preserve meets but not joins: they "behave well when restricting to subsystems, but throw up surprises when joining systems".
- For any monotone map, $f(a) \vee f(b) \leq f(a \vee b)$ and $f(a \wedge b) \leq f(a) \wedge f(b)$ automatically ([[7S Chapter 1 Exercises#Exercise 1.94|7S Exercise 1.94]]); preservation is the reverse inequality.
- [[Right Adjoints Preserve Meets]] and left adjoints preserve joins; conversely, when the domain has all meets/joins, preservation characterizes adjointness ([[Adjoint Functor Theorem for Preorders]]).
- Example 1.113: right adjoints need *not* preserve joins.
- Categorical generalization: [[Right Adjoints Preserve Limits]] (DaoFP §10.7), and a [[Functor]] preserving [[Colimit|colimits]] is "co-continuous".
