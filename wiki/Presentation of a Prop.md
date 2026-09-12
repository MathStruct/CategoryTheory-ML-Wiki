#definition #theorem #example

A **presentation** $(G, s, t, E)$ for a [[Prop]] consists of a signature $(G, s, t)$ and a set $E \subseteq \mathrm{Expr}(G) \times \mathrm{Expr}(G)$ of **equations** (traditionally "relations") between prop expressions of equal arity. The prop $\mathcal{G}$ **presented** by it has morphisms $\mathrm{Expr}(G)$ quotiented by the equations in $E$ and the axioms of symmetric strict monoidal categories. Compared with [[Presentation of a Category|presenting categories]], the things equated are prop expressions rather than paths.

> Sources: 7 Sketches §5.2.5 (Rough Definition 5.33, Remark 5.34, Exercise 5.35), §5.4.1 (Theorem 5.60), §5.4.2 (Remark 5.74), §6.3.1, §6.5.3; Catlab `@present` / `@theory`.

**Universal property** (Remark 5.34): prop functors $\mathcal{G} \to \mathcal{C}$ correspond to functions $f : G \to \mathrm{Mor}(\mathcal{C})$ respecting arities such that $f(e_1) = f(e_2)$ in $\mathcal{C}$ for every $(e_1, e_2) \in E$, where $f(e)$ applies $f$ to each generator and composes in $\mathcal{C}$.

**Examples.**
- [[Prop of Matrices|$\mathbf{Mat}(R)$]] is presented by the signal-flow generators $G_R$ and the equations of Theorem 5.60 ([[Graphical Linear Algebra]]) — a *sound and complete* graphical calculus for matrices.
- "The theory of monoids": generators $\mu : 2 \to 1$, $\eta : 0 \to 1$ with associativity and unit equations; its models in $\mathcal{C}$ are [[Monoid Object|monoid objects]] (Remark 5.74). Adding $\sigma \mathbin{;} \mu = \mu$ gives commutative monoids; the mirror images give comonoids; both with the Frobenius law give the theory of [[Frobenius Monoid|special commutative Frobenius monoids]], presenting $\mathbf{Cospan}_{\mathbf{FinSet}}$ (7 Sketches Theorem 6.x); further equations give bialgebras and [[Hopf Algebra|Hopf algebras]].
- Lawvere's **algebraic theories** and Catlab's generalized algebraic theories (`@theory`) generalize this "syntax + equations, semantics = functors" pattern ([[Functorial Semantics]]).

````tabs
tab: Julia
```julia
# Catlab: a presentation with equations — the theory of commutative monoids as a prop
using Catlab
@present CommMonoid(FreeSymmetricMonoidalCategory) begin
  X::Ob
  μ::Hom(X ⊗ X, X); η::Hom(munit(), X)
  (μ ⊗ id(X)) ⋅ μ == (id(X) ⊗ μ) ⋅ μ
  (η ⊗ id(X)) ⋅ μ == id(X)
  braid(X, X) ⋅ μ == μ
end
equations(CommMonoid)
```
tab: Haskell
```haskell
-- a presentation: generators with arities plus pairs of equal expressions
data Presentation g = Presentation { arity :: g -> (Int, Int), eqs :: [(Expr g, Expr g)] }
```
````
