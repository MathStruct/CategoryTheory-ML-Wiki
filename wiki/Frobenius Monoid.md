#definition #theorem #example

Let $X$ be an object of a [[Symmetric Monoidal Category]] $(\mathcal{C}, I, \otimes)$. A **Frobenius structure** on $X$ is a 4-tuple $(\mu, \eta, \delta, \varepsilon)$ where $(X, \mu : X \otimes X \to X, \eta : I \to X)$ is a commutative [[Monoid Object]] (the *merger* and *initializer*), $(X, \delta : X \to X \otimes X, \varepsilon : X \to I)$ is a cocommutative comonoid (the *splitter* and *terminator*), satisfying the six (co)associativity, (co)unitality and (co)commutativity equations (6.51) together with

- the **Frobenius law**: $(\mathrm{id} \otimes \mu) \circ (\delta \otimes \mathrm{id}) = \delta \circ \mu = (\mu \otimes \mathrm{id}) \circ (\mathrm{id} \otimes \delta)$ (splitting then merging equals merging then splitting, in either of the two zig-zag ways), and
- the **special law**: $\mu \circ \delta = \mathrm{id}_X$ (split then merge is the identity).

An object so equipped is a **special commutative Frobenius monoid** (Carboni–Walters: *separable commutative Frobenius algebra*), or just **Frobenius monoid**.

> Sources: 7 Sketches §6.3.1 (Eq. 6.51, Definitions 6.52, 6.54, Theorem 6.55, Example 6.56, Theorem 6.58, Exercise 6.57), §6.3.3, Examples 6.61, 6.64, 6.65; [CW87; Car91].

## Spiders (Definition 6.54, Theorem 6.55)

Define the **spider** $s_{m,n} : X^{\otimes m} \to X^{\otimes n}$ as $(m - 1)$ mergers followed by $(n-1)$ splitters (with $\eta$/$\varepsilon$ when $m = 0$ or $n = 0$), drawn as a dot with $m$ legs on the left and $n$ on the right. **Theorem 6.55.** Any map $X^{\otimes m} \to X^{\otimes n}$ built from spiders and symmetries by composition and $\otimes$ whose string diagram is *connected* equals $s_{m,n}$. So "a Frobenius monoid is a *spiderable* wire": the result of combining $\mu, \eta, \delta, \varepsilon, \sigma$ is just "how many in's and how many out's" — two spiders sharing a leg fuse into one. Consequently two such morphisms are equal iff their diagrams connect the same ports ([[7S Chapter 6 Exercises#Exercise 6.57|7S Exercise 6.57]]): the Frobenius structure captures **connectivity**.

**Theorem 6.58.** The [[Prop]] presented by generators $\mu, \eta, \delta, \varepsilon$ (arities $2 \to 1$, $0 \to 1$, $1 \to 2$, $1 \to 0$) and the nine Frobenius equations is equivalent, as a symmetric monoidal category, to $\mathbf{Cospan}_{\mathbf{FinSet}}$: ideal wires, connectivity, [[Cospan|cospans]] and Frobenius structures are "all intimately related". Cospans form the theory of hypergraph categories.

## Examples

- In $\mathbf{Cospan}_{\mathcal{C}}$ every object: $\mu_X = (X + X \xrightarrow{[\mathrm{id}, \mathrm{id}]} X \xleftarrow{\mathrm{id}} X)$, $\eta_X = (\varnothing \to X \xleftarrow{\mathrm{id}} X)$, $\delta_X = (X \xrightarrow{\mathrm{id}} X \xleftarrow{[\mathrm{id},\mathrm{id}]} X + X)$, $\varepsilon_X = (X \xrightarrow{\mathrm{id}} X \leftarrow \varnothing)$ (Example 6.61; the special law is the pushout square $X + X \rightrightarrows X$, [[7S Chapter 6 Exercises#Exercise 6.63|7S Exercise 6.63]]; pictures in [[7S Chapter 6 Exercises#Exercise 6.62|7S Exercise 6.62]]).
- In [[Corelation|$\mathbf{Corel}$]]: $\mu_X, \delta_X$ identify the copies of each element, $\eta_X, \varepsilon_X$ relate nothing (Example 6.64).
- In $\mathbf{LinRel}_R$: *two* Frobenius structures, black (copy/discard) and white (add/zero) (Example 6.65) — the two spider colours of [[Graphical Linear Algebra]]; in $\mathbf{Mat}(R)$ the same generators form a bialgebra instead ([[Prop of Matrices]]).
- In $\mathbf{FinVect}$: a finite-dimensional algebra with a nondegenerate invariant form; group algebras.
- Frobenius monoids give a self-dual [[Compact Closed Category|compact closed]] structure: cup $\eta \mathbin{;} \delta$, cap $\mu \mathbin{;} \varepsilon$ (Proposition 6.66, [[7S Chapter 6 Exercises#Exercise 6.67|7S Exercise 6.67]]).

````tabs
tab: Julia
```julia
# Catlab: the theory of hypergraph categories / Frobenius structure via `ThHypergraphCategory`
using Catlab
@present F(FreeHypergraphCategory) begin
  X::Ob
end
X = F[:X]
mmerge(X), create(X), mcopy(X), delete(X)     # μ, η, δ, ε
# spider s_{2,3}: merge two wires then split into three
mmerge(X) ⋅ mcopy(X) ⋅ (mcopy(X) ⊗ id(X))
# the special law μ ∘ δ = id holds in FinSet-cospans: check via pushout (Exercise 6.63)
X2 = FinSet(2); m = FinFunction([1, 1], 1)
apex(pushout(m, m))                           # FinSet(1): the pushout of [id,id] with itself
```
tab: Lean
```lean
-- Mathlib: monoid objects `Mon_ C`, comonoid objects `Comon_ C`; Frobenius laws must be stated by hand
#check Mon_
#check Comon_
structure FrobeniusLaws {C : Type} [CategoryTheory.Category C] [CategoryTheory.MonoidalCategory C]
    (M : Mon_ C) (N : Comon_ C) (h : M.X = N.X) : Prop where
  frobenius : True   -- (id ⊗ μ) ∘ (δ ⊗ id) = δ ∘ μ  (placeholder)
  special : True     -- μ ∘ δ = id
```
tab: Haskell
```haskell
-- a Frobenius structure on a type (laws unenforced): merge, unit, split, counit
data Frobenius x = Frobenius
  { merge :: (x, x) -> x, unit :: () -> x, split :: x -> (x, x), counit :: x -> () }
-- a spider s_{m,n}: fold merges then unfold splits
spider :: Frobenius x -> [x] -> Int -> [x]
spider fr ins n = replicate n (foldr1 (\a b -> merge fr (a, b)) ins)   -- valid when the diagram is connected
```
````
