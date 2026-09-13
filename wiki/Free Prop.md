#definition #theorem #example

A **prop signature** $(G, s, t)$ is a set $G$ of **generators** with functions $s, t : G \to \mathbb{N}$ giving each generator its **in-arity** and **out-arity** (the *arity* of a prop morphism is the pair $(m, n)$). A **$G$-labeling** of a [[Port Graph]] $\Gamma = (V, \mathrm{in}, \mathrm{out}, \iota)$ is a function $\ell : V \to G$ with $s(\ell(v)) = \mathrm{in}(v)$ and $t(\ell(v)) = \mathrm{out}(v)$. The **free prop** $\mathrm{Free}(G)$ has as morphisms $m \to n$ all $G$-labeled $(m, n)$-port graphs, with the composition and monoidal product of $\mathbf{PG}$ (labels carried along). Boxes are drawn with their labels; a generator may be used many times or not at all.

> Sources: 7 Sketches §5.2.3–5.2.4 (Definitions 5.25, 5.30, Examples 5.19–5.22, 5.27, 5.31, Proposition 5.29, Exercises 5.20–5.24, 5.28, 5.32, 5.35), Remark 5.34; §5.3.2 ($\mathbf{SFG}_R$); Catlab `@present` with a single generating object.

## Universal property (Proposition 5.29)

For any prop $\mathcal{C}$, prop functors $\mathrm{Free}(G) \to \mathcal{C}$ are in bijection with functions $G \to \mathrm{Mor}(\mathcal{C})$ sending each $g$ to a morphism $s(g) \to t(g)$. "Freeness has to do with maps *out*": the free structure is the minimally constrained one containing the specified data, so every assignment of generators extends uniquely. The same pattern: the free [[Preorder]] on a relation is its [[Reflexive Transitive Closure]] (Example 5.19, [[7S Chapter 5 Exercises#Exercise 5.20|7S Exercise 5.20]]: $f$ is monotone iff it preserves the generating relation; [[7S Chapter 5 Exercises#Exercise 5.21|7S Exercise 5.21]]: the property is about maps out, not in); the [[Free Category]] on a graph (Example 5.22, [[7S Chapter 5 Exercises#Exercise 5.23|7S Exercise 5.23]]: functors $\mathrm{Free}(G) \to \mathcal{C}$ are graph homomorphisms $G \to U(\mathcal{C})$, an [[Adjunction]] $\mathrm{Free} \dashv U$); the [[Free Monoid]] on a set as the free category on a one-vertex graph ([[7S Chapter 5 Exercises#Exercise 5.24|7S Exercise 5.24]]). "A higher-level justification understands freeness as a left adjoint" ([[Free-Forgetful Adjunction]]).

## Prop expressions (Definition 5.30)

A syntactic description: **$G$-generated prop expressions** are built inductively from $\mathrm{id}_0 : 0 \to 0$, $\mathrm{id}_1 : 1 \to 1$, the symmetry $\sigma : 2 \to 2$ and the generators, closed under $\alpha + \beta$ and $\alpha \mathbin{;} \beta$. Different expressions may denote the same morphism ($f \mathbin{;} \mathrm{id}_1 = f$); $\mathrm{Free}(G)$ is $\mathrm{Expr}(G)$ modulo the axioms of symmetric strict monoidal categories — equivalently, two expressions are equal iff they represent the same labeled port graph. Translating a port graph into an expression: sweep a vertical line left to right and write down the sum of all boxes, symmetries and identity wires in each "action column", then compose the columns: e.g. $(g + \mathrm{id}_1) \mathbin{;} (\mathrm{id}_1 + \sigma) \mathbin{;} (\mathrm{id}_1 + g) \mathbin{;} (h + \mathrm{id}_1)$ for Eq. (5.26); [[7S Chapter 5 Exercises#Exercise 5.32|7S Exercise 5.32]] goes the other way.

## Examples

- $\mathrm{Free}(\varnothing) = \mathbf{Bij}$: with no generators $V = \varnothing$ and only permutations remain (Example 5.27).
- $\mathrm{Free}(\{\rho_{m,n}\}_{m,n}) = \mathbf{PG}$ ([[7S Chapter 5 Exercises#Exercise 5.28|7S Exercise 5.28]]).
- $\mathbf{SFG}_R = \mathrm{Free}(G_R)$ with $G_R = \{\text{copy}, \text{discard}, \text{add}, \text{zero}\} \cup \{a \mid a \in R\}$: [[Signal Flow Graph|signal flow graphs]].
- Adding equations gives a [[Presentation of a Prop]]; $\mathrm{Free}(G) $ is the prop presented by $(G, \varnothing)$ up to the "subtle difference" between a set and its quotient by the trivial relation ([[7S Chapter 5 Exercises#Exercise 5.35|7S Exercise 5.35]]).

````tabs
tab: Julia
```julia
# Catlab: a free symmetric monoidal category on a signature with one generating object = a free prop
using Catlab
@present G(FreeSymmetricMonoidalCategory) begin
  X::Ob
  f::Hom(X, X); g::Hom(X ⊗ X, X ⊗ X); h::Hom(X ⊗ X, X)
end
X, f, g, h = G[:X], G[:f], G[:g], G[:h]
σ = braid(X, X)
e = (g ⊗ id(X)) ⋅ (id(X) ⊗ σ) ⋅ (id(X) ⊗ g) ⋅ (h ⊗ id(X))       # the expression for Eq. (5.26), 3 → 2
# prop expressions can be rendered as wiring diagrams / port graphs:
using Catlab.WiringDiagrams
to_wiring_diagram(e)
```
tab: Haskell
```haskell
-- DaoFP-style free structure: prop expressions as a syntax tree (Definition 5.30)
data Expr g = Id0 | Id1 | Swap | Gen g | Par (Expr g) (Expr g) | Seq (Expr g) (Expr g)
-- the free prop is Expr g modulo the SMC axioms; a prop functor is determined by a map g -> hom
eval :: Prop' hom => (g -> hom) -> Expr g -> hom
eval _ Id0 = idP 0
eval _ Id1 = idP 1
eval _ Swap = swapP 1 1
eval k (Gen g) = k g
eval k (Par a b) = plusP (eval k a) (eval k b)
eval k (Seq a b) = compP (eval k a) (eval k b)
```
````
