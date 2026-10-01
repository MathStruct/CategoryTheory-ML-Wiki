#definition #theorem #example #program

A **cartesian bicategory** (Carboni & Walters) is the abstract structure of a [[Category of Relations]]. In the form used by Bonchi, Seeber & Sobociński (Definition 19) it is a [[Symmetric Monoidal Category]] $(\mathcal B, \oplus, I)$ **enriched in posets** — between any two morphisms there may be an inclusion $R \le S$ — such that

1. every object carries a **special Frobenius bimonoid**: a comonoid $\mathrm{copy}_X : X \to X \oplus X$, $\mathrm{discard}_X : X \to I$ and a monoid $\mathrm{join}_X : X \oplus X \to X$, $\mathrm{new}_X : I \to X$, satisfying the Frobenius law ([[Frobenius Monoid]]);
2. the monoid and comonoid are **adjoint** to each other in the poset enrichment;
3. every morphism $R$ is a **lax comonoid morphism**: $R \,;\, \mathrm{copy} \le \mathrm{copy} \,;\, (R \oplus R)$ and $R \,;\, \mathrm{discard} \le \mathrm{discard}$.

The archetype is $\mathbf{Rel}$: sets, relations, $\times$, inclusion. Copy is the diagonal $x \mapsto (x, x)$, join is its converse ("these two wires carry equal values"), discard is $\exists$, new is "some value". Condition 3 holds with $\le$ rather than $=$ because a relation can relate one input to several outputs: copying *after* $R$ gives only the pairs $(y, y)$, copying *before* gives all pairs $(y_1, y_2)$.

> Sources: Carboni & Walters, *Cartesian bicategories I*, J. Pure Appl. Algebra 49 (1987); Bonchi, Seeber & Sobociński, *Graphical Conjunctive Queries*, CSL 2018, [arXiv:1804.07626](https://arxiv.org/abs/1804.07626) ([[Graphical Conjunctive Queries|notes]]) Definition 19, Example 20, Theorem 17; Fong & Spivak, *Regular and relational categories: Revisiting 'Cartesian bicategories I'*, [arXiv:1909.00069](https://arxiv.org/abs/1909.00069) ([[Regular and Relational Categories - Revisiting Cartesian Bicategories I|notes]]) Definitions 3.3, 4.14, 4.15, 8.1, 8.4, 8.6, Theorems 5.9, 6.25, 7.3; Freyd & Scedrov, *Categories, Allegories* (North-Holland 1990) for the allegory version; 7 Sketches Ch. 6 for the Frobenius structure on $\mathbf{Rel}$ and [[Hypergraph Category|hypergraph categories]].

## Maps are left adjoints

The poset enrichment lets one *recover functions from relations*. In $\mathbf{Rel}$ a relation $R$ has a right adjoint iff it is a function, and the right adjoint is then its converse $R^\circ$:

$$
\mathrm{id} \le R \,;\, R^\circ \;\;(R \text{ is total}), \qquad R^\circ \,;\, R \le \mathrm{id} \;\;(R \text{ is single-valued}).
$$

So in any cartesian bicategory one defines a **map** to be a left adjoint, and maps are exactly the morphisms that are *strict* comonoid morphisms — the ones that can be copied and discarded. The maps form an ordinary category with finite products. This is the relational version of the [[Discard and Copy Axioms]]: in a [[Markov Category]] *deterministic* means "copyable"; here *functional* means "copyable and deletable".

## Regular categories ⇄ relational po-categories

Fong and Spivak make the correspondence with logic precise. A **regular category** (Definition 3.3) has finite limits and pullback-stable image factorisations; its internal logic is **regular logic** — $=, \top, \wedge, \exists$ — exactly the logic of [[Conjunctive Query|conjunctive queries]]. Every regular category $\mathcal R$ has a bicategory of relations, built from jointly monic spans, and conversely a **relational po-category** (Definition 4.15: one in which every morphism has a *tabulation*, Definition 4.14) has a regular category of left adjoints:

> **Theorem 7.3.** The 2-functors $\mathrm{Rel} : \mathbf{RgCat} \rightleftarrows \mathbf{RlPoCat} : \mathrm{LAdj}$ form an equivalence of 2-categories.

So "a regular category" and "a cartesian bicategory with tabulations" are two presentations of the same thing — one in terms of functions with images, one in terms of relations with an order. Carboni–Walters' functionally complete bicategories of relations (Definition 8.4) and Freyd–Scedrov's unital tabular allegories (Definitions 8.6–8.7) are the same notion again.

## Why databases and compilers meet here

| in a cartesian bicategory | in a database | in a compiler |
|---|---|---|
| morphism $R : X \to Y$ | a relation / query | a nondeterministic or partial program |
| $R \le S$ | query containment | **refinement**: $R$ has fewer behaviours than $S$ |
| map (left adjoint) | a functional dependency | a deterministic, total function |
| copy / discard | joins on a shared variable / projection | using a value twice / dropping it |
| converse $R^\circ$ | swapping columns | running a relation backwards (logic programming) |

The cross-language correspondences of multi-language semantics — "a Julia `Int64` and a C++ `int64_t` are related on the non-overflowing subdomain" — are morphisms of exactly this kind, and composing two of them is relational composition, with containment as the honest notion of "at least as defined".

## Sophia

Sophia's `REFINES` edge is the order of a cartesian bicategory and its `EQUIV` edges are the symmetric part; the per-language-pair relation $R \subseteq \mathcal D_1 \times \mathcal D_2$ in a cross-language equivalence is a morphism of $\mathbf{Rel}$, and composing correspondences across three languages is relational composition. See [Equivalence and Witnesses](https://mathstruct.org/Sophia/vault/Design/Equivalence-and-Witnesses).

````tabs
tab: Julia
**Docs:** plain Julia — Catlab has no dedicated API for this; related: [Catlab v0.16 docs](https://algebraicjulia.github.io/Catlab.jl/v0.16/) · [GATlab standard library](https://algebraicjulia.github.io/GATlab.jl/stable/stdlib/)
```julia
# Rel as Boolean matrices: R[x, y] = true iff x R y. Composition is Boolean matrix product; order is ⊆.
compose(R, S) = (R * S) .> 0                  # (R ; S)[x, z] = ∃ y. R[x,y] ∧ S[y,z]
leq(R, S) = all(.!R .| S)                     # R ⊆ S
op(R) = permutedims(R)                        # the converse relation R°
Id(n) = [i == j for i in 1:n, j in 1:n]
# copy : X → X × X and discard : X → 1 (the comonoid on every object), X = {1, 2, 3}
n = 3
copy = [y == (x - 1) * n + x for x in 1:n, y in 1:n^2]    # x ↦ (x, x), pairs encoded as (a-1)n + b
tensor(R, S) = kron(R, S) .> 0               # R ⊗ S on pairs, same (a-1)n + b encoding as copy
R = Bool[1 1 0; 0 0 1; 0 0 0]                 # a relation that is neither total nor single-valued
# every relation is a *lax* comonoid morphism: R ; copy ⊆ copy ; (R ⊗ R)
leq(compose(R, copy), compose(copy, tensor(R, R)))     # true
compose(R, copy) == compose(copy, tensor(R, R))        # false: 1 R 1 and 1 R 2 give (1,2) on the right only
# maps = left adjoints: R ⊣ R° iff id ⊆ R ; R° (total) and R° ; R ⊆ id (single-valued) iff R is a function
isleftadjoint(R) = leq(Id(size(R, 1)), compose(R, op(R))) && leq(compose(op(R), R), Id(size(R, 2)))
f = Bool[0 1 0; 0 0 1; 0 0 1]                 # the function 1↦2, 2↦3, 3↦3
(isleftadjoint(R), isleftadjoint(f))          # (false, true)
compose(f, copy) == compose(copy, tensor(f, f))        # true: functions are strict comonoid morphisms
```
tab: Lean
```lean
import Mathlib
-- Relations as predicates; composition is ∃, the order is pointwise implication.
def comp {α β γ : Type*} (R : α → β → Prop) (S : β → γ → Prop) : α → γ → Prop :=
  fun x z => ∃ y, R x y ∧ S y z
def conv {α β : Type*} (R : α → β → Prop) : β → α → Prop := fun y x => R x y

-- a function, viewed as a relation, is a left adjoint: id ≤ f ; f°  and  f° ; f ≤ id
example {α β : Type*} (f : α → β) (x : α) : comp (fun a b => f a = b) (conv fun a b => f a = b) x x :=
  ⟨f x, rfl, rfl⟩
example {α β : Type*} (f : α → β) (y y' : β)
    (h : comp (conv fun a b => f a = b) (fun a b => f a = b) y y') : y = y' := by
  obtain ⟨x, hx, hx'⟩ := h
  exact hx.symm.trans hx'
```
````
