#theorem #proof

**Proposition 1.111.** Let $f : P \to Q$ be left adjoint to $g : Q \to P$ in a [[Galois Connection]]. If $A \subseteq Q$ has a [[Meet]] $\bigwedge A$, then $g(A) := \{g(a) \mid a \in A\}$ has a meet in $P$ and
$$g\Big(\bigwedge A\Big) \cong \bigwedge g(A).$$
That is, **right adjoints preserve meets**. Dually, **left adjoints preserve joins**: if $A \subseteq P$ has a [[Join]] then $f(\bigvee A) \cong \bigvee f(A)$.

> Sources: 7 Sketches Proposition 1.111, Exercise 1.112, Example 1.113; general version: [[Right Adjoints Preserve Limits]] (DaoFP §10.7).

*Proof.* Let $m := \bigwedge A$. Since $g$ is monotone, $g(m) \leq g(a)$ for all $a \in A$: $g(m)$ is a lower bound for $g(A)$. Let $b$ be any other lower bound, so $b \leq g(a)$ for all $a \in A$. By the adjunction, $f(b) \leq a$ for all $a$, so $f(b)$ is a lower bound for $A$ and hence $f(b) \leq m$. Using the adjunction again, $b \leq g(m)$. So $g(m)$ is the greatest lower bound. $\blacksquare$

The join claim is [[7S Exercise 1.112]] (same argument with the order reversed).

**Consequences.** Left adjoints never have a [[Generative Effect]]. Right adjoints need *not* preserve joins (Example 1.113: $P = \{1, 2 \leq 3.9 \leq 4\}$, $Q = \{1, 2 \leq 4\}$, $g$ the label-preserving inclusion $Q \to P$ is right adjoint to $f$ with $f(3.9) = 4$, yet $g(1 \vee 2) = g(4) = 4 \neq 3.9 = g(1) \vee g(2)$; [[7S Exercise 1.114]]). The converse — meet-preservation implies right adjoint when all meets exist — is the [[Adjoint Functor Theorem for Preorders]].

````tabs
tab: Lean
```lean
#check @GaloisConnection.u_iInf   -- u (⨅ i, f i) = ⨅ i, u (f i)
#check @GaloisConnection.l_iSup   -- l (⨆ i, f i) = ⨆ i, l (f i)
#check @GaloisConnection.u_inf
#check @GaloisConnection.l_sup
```
````
