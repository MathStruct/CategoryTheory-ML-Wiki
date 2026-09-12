#definition #example #theorem

A **Heyting algebra** is a [[Partial Order|poset]] $H$ with finite [[Meet|meets]] $\wedge$, finite [[Join|joins]] $\vee$, top $\mathsf{true}$, bottom $\mathsf{false}$, and an **implication** $\Rightarrow$ characterized by
$$r \leq (p \Rightarrow q) \iff r \wedge p \leq q,$$
i.e. $(- \wedge p) \dashv (p \Rightarrow -)$ is a [[Galois Connection]]. Negation is $\neg p := p \Rightarrow \mathsf{false}$. A Heyting algebra is exactly a thin [[Bicartesian Closed Category]]; it is a *Boolean algebra* when moreover $\neg\neg p = p$ (equivalently $p \vee \neg p = \mathsf{true}$).

> Sources: 7 Sketches §7.4.3 ("the poset $|\Omega^S|$ of predicates on $S$ forms what's called a Heyting algebra"), §7.4.2, Exercises 7.11, 7.59, 7.60, Remark 7.33; DaoFP §6 (bicartesian closed categories).

- **Examples**: [[Booleans|$\mathbb{B}$]]; any [[Power Set]]; the open sets $\mathrm{Op}(X)$ of a [[Topological Space]] with $U \Rightarrow V = \bigcup\{R \mid R \cap U \subseteq V\}$ ([[Internal Logic of a Topos]]) — a *frame*, since it also has arbitrary joins, and hence a [[Quantale]] with $\otimes = \cap$ (Remark 7.33); the [[Subobject|subobjects]] $\mathrm{Sub}(S)$ of any object in a [[Topos]], equivalently the [[Predicate|predicates]] $S \to \Omega$; the [[Upper Set|upper sets]] of a preorder.
- The [[Cartesian Closed Category|cartesian closed]] preorders are exactly the meet-semilattices with implication; a [[Quantale]] with $v \leq I$, $v \otimes w \leq v, w$ and $x \leq v, w \Rightarrow x \leq v \otimes w$ is a complete Heyting algebra ([[7S Exercise 7.11]]), but not every cartesian closed preorder has all joins ($\mathbb{N}^{\mathrm{op}} \times \mathbb{N}^{\mathrm{op}}$ has no bottom).
- [[Closure Operator|Closure operators]] on a Heyting algebra preserving $\wedge$ are *nuclei* — the [[Modality|modalities]] of topos logic.

````tabs
tab: Julia
```julia
using Catlab
# Sub(G) of a C-set is a Heyting algebra: meet, join, top, bottom, implies, negate
G = cycle_graph(Graph, 4)
A = Subobject(G, V=[1, 2], E=[1]); B = Subobject(G, V=[2, 3], E=[2])
sh(s) = (c = components(s); (collect(c[:V]), collect(c[:E])))
sh(implies(A, B))                         # ([2, 3, 4], [2, 3]): the largest r with r ∧ A ≤ B
sh(implies(A, B) ∧ A)                     # ([2], []) ⊆ B
sh(top(G)), sh(bottom(G))
```
tab: Lean
```lean
import Mathlib
#check @HeytingAlgebra                    -- class with ⇨ (himp) and ᶜ
#check @le_himp_iff                       -- a ≤ b ⇨ c ↔ a ⊓ b ≤ c
#check @HeytingAlgebra.toBooleanAlgebra   -- not a thing: Boolean needs a ⊔ aᶜ = ⊤
example (X : Type) [TopologicalSpace X] : HeytingAlgebra (TopologicalSpace.Opens X) := inferInstance
```
tab: Haskell
```haskell
class Heyting h where
  top, bot :: h
  (/\), (\/), (==>) :: h -> h -> h
neg :: Heyting h => h -> h
neg p = p ==> bot
instance Heyting Bool where
  top = True; bot = False
  (/\) = (&&); (\/) = (||); p ==> q = not p || q
```
````
