#theorem #proof #annotation

**Freyd's adjoint functor theorem.** Let $L : \mathcal{D} \to \mathcal{C}$ be a [[Functor]] from a (small-)cocomplete, locally small category that preserves [[Colimit|colimits]]. If for every $c \in \mathcal{C}$ there is a **solution set** — a set-indexed family $(d_i, f_i : Ld_i \to c)_{i \in I}$ such that every $f : Ld \to c$ factors as $f = f_i \circ Lh$ for some $i$ and $h : d \to d_i$ (a weakly terminal *set* in the [[Comma Category]] $L \downarrow c$) — then $L$ has a right adjoint. Dually, a limit-preserving functor from a complete category with solution sets has a left adjoint.

> Sources: DaoFP §10.8 ("Freyd's adjoint functor theorem", "Freyd's theorem in a preorder", "Solution set condition", "Defunctionalization"), §9.5 ("The existence of the terminal object"); preorder case: 7 Sketches Theorem 1.115 ([[Adjoint Functor Theorem for Preorders]]).

*Idea of proof.* Since left adjoints preserve colimits, $L$ must. To build the right adjoint we need, for every $c$, a [[Universal Arrow]] from $L$ to $c$, i.e. a [[Terminal Object]] of $L \downarrow c$. **Preorder case (all colimits exist, $\mathcal{D}$ a preorder):** the comma category $L \downarrow c$ is a cocone in $\mathcal{C}$ with apex $c$; project its base back to $\mathcal{D}$ via $\pi_c : (d, f) \mapsto d$ and take $t_c := \mathrm{colim}\,\pi_c$. Since $L$ preserves colimits, $Lt_c = \mathrm{colim}(L \circ \pi_c)$, so there is a unique cocone morphism $\varepsilon_c : Lt_c \to c$; any $(d, f)$ is part of the diagram, giving the wire $h : d \to t_c$ with $f = \varepsilon_c \circ Lh$, unique because $\mathcal{D}$ is a preorder. **General case:** comma categories are large, so we cannot take their colimit; but in a cocomplete locally small category *a weakly terminal set yields a terminal object* (take the coproduct $\coprod_i t_i$, then the colimit of the full subcategory on the $t_i$; DaoFP §9.5). Applying this in $L \downarrow c$ to the solution set produces the universal arrow. $\blacksquare$

**Programming: [[Defunctionalization]].** The function type is the right adjoint $(-)^a$ to $(- \times a)$; the adjoint functor theorem says it can be *approximated* by a solution set of environments. A finite program has finitely many function definitions, which (with their captured environments) form the solution set; this replaces higher-order functions by data plus an `apply` — the technique behind serializing continuations in distributed systems.

````tabs
tab: Lean
```lean
#check CategoryTheory.SolutionSetCondition
#check CategoryTheory.isRightAdjoint_of_preservesLimits_of_solutionSetCondition  -- Freyd's theorem (right adjoint form)
```
tab: Haskell
```haskell
-- see [[Defunctionalization]] for the worked example (sumK with continuations replaced by data Kont)
```
````
