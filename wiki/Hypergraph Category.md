#definition #example #theorem #proof

A **hypergraph category** is a [[Symmetric Monoidal Category]] $(\mathcal{C}, I, \otimes)$ in which every object $X$ carries a [[Frobenius Monoid|Frobenius structure]] $(X, \mu_X, \eta_X, \delta_X, \varepsilon_X)$, coherently with the monoidal product: the Frobenius maps of $X \otimes Y$ are built from those of $X$ and $Y$ (with a swap in the middle for $\mu_{X \otimes Y}$ and $\delta_{X \otimes Y}$), and $\eta_I = \mathrm{id}_I = \varepsilon_I$. A **hypergraph prop** is a hypergraph category that is a [[Prop]]. Its [[Wiring Diagram|wiring diagrams]] are **network diagrams**: labelled boxes, wires that may bend (as in [[Compact Closed Category|compact closed categories]]), and wires that may **split, join, terminate and initialize** — the spiders. Spiders of the same "species" (object) fuse when they share a leg; spiders of different species cannot.

> Sources: 7 Sketches §6.1, §6.3 (Definition 6.60, Examples 6.61, 6.64, 6.65, Proposition 6.66, Exercises 6.59, 6.62, 6.63, 6.67), §6.4 (Theorem 6.77), §6.5 (Proposition 6.101), §6.6; [CW87] ("well-supported compact closed category"), [Fon15; Fon16; Fon18; FS18a; FS18b]; Kittenlab Lecture 15 (cospans, undirected wiring diagrams); Catlab `ThHypergraphCategory`, `@relation`, `oapply`.

## Examples

- **$\mathbf{Cospan}_{\mathcal{C}}$** for any $\mathcal{C}$ with finite colimits (Example 6.61) — the prototype; $\mathbf{Cospan}_{\mathbf{FinSet}}$ is equivalent to the hypergraph prop presented by the Frobenius axioms (Theorem 6.58). Its wiring diagrams are [[Undirected Wiring Diagram|undirected wiring diagrams]].
- **[[Corelation|$\mathbf{Corel}$]]** (Example 6.64), **$\mathbf{LinRel}_R$** in two ways (Example 6.65), **[[Category of Relations|$\mathbf{Rel}$]]**.
- **[[Decorated Cospan|Decorated cospans]] $\mathbf{Cospan}_F$** (Theorem 6.77) — e.g. open electric circuits, open [[Petri Net|Petri nets]], Markov processes; [[Structured Cospan|structured cospans]] and [[Open Graph|open graphs]].
- Every hypergraph prop is a [[Operad Algebra|$\mathbf{Cospan}$-algebra]] and conversely (Proposition 6.101).

## Hypergraph categories are self-dual compact closed (Proposition 6.66)

Define the cup $\eta_X \mathbin{;} \delta_X : I \to X \otimes X$ and the cap $\mu_X \mathbin{;} \varepsilon_X : X \otimes X \to I$. The snake equation follows: (id ⊗ cup) ; (cap ⊗ id) $=$ (id ⊗ (η ; δ)) ; ((μ ; ε) ⊗ id) $=$ by the Frobenius law $(\mathrm{id} \otimes \eta) \mathbin{;} \mu \mathbin{;} \delta \mathbin{;} (\varepsilon \otimes \mathrm{id})$ $=$ by unitality and counitality $\mathrm{id}_X$ ([[7S Chapter 6 Exercises#Exercise 6.67|7S Exercise 6.67]] fills in the middle step). Hence $X^* = X$ and wires may be bent freely — the "well-supported compact closed" of Carboni–Walters. Compare [[Graphical Linear Algebra|Theorem 5.87]].

## Why hypergraph categories

"Network-type interconnection can be described using a hypergraph category" (§6.1): the domain/codomain split of a morphism is an artifact (circuits have one boundary), but the Frobenius structure lets one move ports freely between the two sides; the [[Operad]] $\mathbf{Cospan}$ removes the artifact entirely (§6.5.1). [[7S Chapter 6 Exercises#Exercise 6.59|7S Exercise 6.59]] infers wire labels in a hypergraph-category diagram.

````tabs
tab: Julia
```julia
using Catlab, Catlab.WiringDiagrams, Catlab.Programs
# undirected wiring diagrams are the string diagrams of hypergraph categories; @relation builds them
uwd = @relation (x, z) begin
  R(x, y)            # a box with ports x, y
  S(y, z)            # a box with ports y, z: the junction y is a spider joining two legs
end
# `oapply` evaluates a hypergraph-category algebra (e.g. finite relations, Petri nets, circuits) on a UWD
# Catlab's theory ThHypergraphCategory has mcopy (δ), delete (ε), mmerge (μ), create (η):
@present H(FreeHypergraphCategory) begin X::Ob end
X = H[:X]
mmerge(X) ⋅ mcopy(X) ⋅ (mcopy(X) ⊗ id(X))      # the spider s_{2,3}
```
tab: Haskell
```haskell
-- a hypergraph category as an SMC with a coherent Frobenius structure on every object (schematic)
class SymMonoidal cat => Hypergraph cat where
  merge  :: cat (x, x) x
  unitH  :: cat () x
  split  :: cat x (x, x)
  counitH :: cat x ()
-- cup = unitH >>> split ; cap = merge >>> counitH  (Proposition 6.66)
```
````
