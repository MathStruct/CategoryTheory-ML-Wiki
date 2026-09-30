#definition #example

A **Cartesian left additive category** is a [[Cartesian Category]] in which every hom-set is a commutative monoid $(\mathcal C(A,B), +, 0)$, precomposition preserves the addition ($x(f+g) = xf + xg$, $x0 = 0$), and the projections are *additive* (they also preserve sums under postcomposition). Equivalently, every object carries a canonical commutative monoid $+_A : A \times A \to A$, $0_A : 1 \to A$.

A **Cartesian differential category** (CDC) is such a category with a **differential combinator** sending each $f : A \to B$ to its *derivative*

$$
D[f] : A \times A \longrightarrow B, \qquad D[f](a, v) \;\approx\; \text{"}J_f(a)\,v\text{"},
$$

satisfying seven axioms [CDC.1–7]: $D$ is additive; $D[f]$ is additive in the direction $v$; identities and projections are linear; $D$ of a pairing is the pairing of the $D$'s; the **chain rule** $D[fg] = \langle \pi_0 f, D[f]\rangle D[g]$; $D[f]$ is linear in $v$ (a second-derivative condition); and the **symmetry of mixed partial derivatives**. A map is **linear** if $D[f] = \pi_1 f$; linear maps form a subcategory.

> Sources: Blute, Cockett & Seely, *Cartesian differential categories*, Theory Appl. Categ. 22 (2009) no. 23 — Definitions 1.1.1, 1.2.1, 2.1.1; Cockett, Cruttwell, Gallagher, Lemay, MacAdam, Plotkin & Pronk, *Reverse derivative categories* [arXiv:1910.07065](https://arxiv.org/abs/1910.07065) ([[Reverse Derivative Categories|notes]]) Definitions 1, 4, 9, Examples 2, 5; Cruttwell, *Cartesian differential categories revisited* [arXiv:1208.4070](https://arxiv.org/abs/1208.4070) ([[Cartesian Differential Categories Revisited|notes]]); Cruttwell et al. [arXiv:2103.01931](https://arxiv.org/abs/2103.01931) ([[Categorical Foundations of Gradient-Based Learning|notes]]) Appendix A (Definitions A.2–A.4).

## Examples

- **$\mathbf{Smooth}$**: objects $\mathbb R^n$, smooth maps; $D[f](a, v) = J_f(a)\, v$, the Jacobian–vector product. This is **forward-mode automatic differentiation** (a `jvp` / pushforward).
- **Polynomials** $\mathbf{Poly}_R$ over a commutative semiring (or ring) $R$: formal differentiation of polynomial maps.
- **Boolean circuits / $\mathbf{Poly}_{\mathbb Z_2}$**: the derivative of a polynomial over $\mathbb Z_2$ — the discrete "gradient" used to learn Boolean circuits (Wilson & Zanasi, [arXiv:2101.10488](https://arxiv.org/abs/2101.10488) ([[Reverse Derivative Ascent - A Categorical Approach to Learning Boolean Circuits|notes]])).
- Any category of **linear maps** with biproducts, where $D[f] = \pi_1 f$.
- **Tangent categories** generalise CDCs from vector-space-like objects to manifolds (Cockett & Cruttwell).

## Forward versus reverse

A CDC axiomatises forward-mode differentiation. The reverse mode needed for backpropagation — $R[f] : A \times B \to A$, $R[f](a, \bar b) = J_f(a)^\top \bar b$ — is *more* structure: every [[Reverse Derivative Category]] is a CDC (Cockett et al., Theorem 16), and a CDC is a reverse derivative category **exactly when its linear maps carry a (contextual) dagger** that transposes them (Theorem 42). The dagger is the transpose $J^\top$; forward mode has no way to produce it.

````tabs
tab: Julia
**Docs:** plain Julia — Catlab has no dedicated API for this; related: [Catlab v0.16 docs](https://algebraicjulia.github.io/Catlab.jl/v0.16/) · [GATlab standard library](https://algebraicjulia.github.io/GATlab.jl/stable/stdlib/)
```julia
# Forward-mode differentiation with dual numbers: D[f](a, v) = J_f(a) v.
struct Dual; v::Float64; d::Float64; end
Base.:+(x::Dual, y::Dual) = Dual(x.v + y.v, x.d + y.d)
Base.:*(x::Dual, y::Dual) = Dual(x.v * y.v, x.v * y.d + x.d * y.v)
Base.sin(x::Dual) = Dual(sin(x.v), cos(x.v) * x.d)
Base.:*(c::Real, x::Dual) = Dual(c * x.v, c * x.d)
D(f) = (a, v) -> f(Dual(a, v)).d                   # the differential combinator
f(x) = sin(x * x)
g(x) = 3.0 * x
# [CDC.5] chain rule: D[f∘g](a, v) = D[f](g(a), D[g](a, v))
a, v = 0.7, 1.3
D(f ∘ g)(a, v) ≈ D(f)(g(a), D(g)(a, v))            # true
# [CDC.2]/linearity in the direction v
D(f)(a, 2v) ≈ 2 * D(f)(a, v)                        # true
```
tab: Lean
```lean
import Mathlib
-- In Mathlib the differential of f : E → F at a is the continuous linear map `fderiv ℝ f a`;
-- D[f](a, v) = fderiv ℝ f a v, and the chain rule is `fderiv_comp`.
#check @fderiv
#check @fderiv_comp
#check @HasFDerivAt.comp
```
tab: Haskell
```haskell
-- Dual numbers: forward-mode AD, i.e. the differential combinator D of Smooth
data Dual = Dual Double Double deriving Show
instance Num Dual where
  Dual a a' + Dual b b' = Dual (a + b) (a' + b')
  Dual a a' * Dual b b' = Dual (a * b) (a * b' + a' * b)
  fromInteger n = Dual (fromInteger n) 0
  negate (Dual a a') = Dual (negate a) (negate a')
  abs = undefined; signum = undefined

d :: (Dual -> Dual) -> Double -> Double -> Double      -- D[f](a, v)
d f a v = let Dual _ y = f (Dual a v) in y
-- d (\x -> x * x * 3) 2 1 == 12
```
````
