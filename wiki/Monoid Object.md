#definition #example

A **monoid** in a [[Monoidal Category]] $(\mathcal{C}, \otimes, I)$ (a **monoid object**) is an object $m$ with morphisms
$$\mu : m \otimes m \to m \quad (\text{multiplication}), \qquad \eta : I \to m \quad (\text{unit})$$
such that the unit laws $\mu \circ (\eta \otimes \mathrm{id}_m) = \lambda_m$, $\mu \circ (\mathrm{id}_m \otimes \eta) = \rho_m$ and the associativity law $\mu \circ (\mu \otimes \mathrm{id}) = \mu \circ (\mathrm{id} \otimes \mu) \circ \alpha$ hold — the monoid laws "formulated in bulk, without recourse to elements", using only functoriality of $\otimes$, the unit and associativity isomorphisms; "we never had to use projections", so the definition works for any tensor product, even non-symmetric. A **comonoid** is the dual: $\delta : m \to m \otimes m$, $\varepsilon : m \to I$.

```tikz
\usepackage{tikz-cd}
\begin{document}
\begin{tikzcd}[column sep=small]
(m \otimes m) \otimes m \arrow[rr, "\alpha"] \arrow[d, "\mu \otimes \mathrm{id}"'] & & m \otimes (m \otimes m) \arrow[d, "\mathrm{id} \otimes \mu"] & I \otimes m \arrow[r, "\eta \otimes \mathrm{id}"] \arrow[dr, "\lambda"'] & m \otimes m \arrow[d, "\mu"] & m \otimes I \arrow[l, "\mathrm{id} \otimes \eta"'] \arrow[dl, "\rho"] \\
m \otimes m \arrow[dr, "\mu"'] & & m \otimes m \arrow[dl, "\mu"] & & m & \\
 & m & & & &
\end{tikzcd}
\end{document}
```

> Sources: DaoFP §5.3 ("Monoids"), §10.9 ("The category of monoids" $\mathbf{Mon}(\mathcal{C})$), §14.7 ("Monad as a monoid"), §17.7 ("Applicative functors as monoids"); 7 Sketches §5.4.2 ("Aside: monoid objects in a monoidal category", Definition 5.x, Exercises), §6.3.1 ([[Frobenius Monoid]]); Kittenlab Lecture 5 ([[Monoid|ordinary monoids]]).

## Examples

- In $(\mathbf{Set}, \times, 1)$: ordinary [[Monoid|monoids]] — `mappend :: (m, m) -> m`, `mempty :: () -> m` (DaoFP). In $(\mathbf{Set}, +, 0)$: every set uniquely, $\mu = [\mathrm{id}, \mathrm{id}]$.
- In $([\mathcal{C}, \mathcal{C}], \circ, \mathrm{Id})$: [[Monad|monads]] — "a monad is a monoid in the category of endofunctors" (DaoFP §14.7). In $([\mathcal{C}, \mathbf{Set}], \star_{\mathrm{Day}})$: lax monoidal (applicative) functors (DaoFP §17.7). In $\mathbf{Vect}$: algebras; comonoids are coalgebras; both: bialgebras, [[Hopf Algebra|Hopf algebras]].
- In a [[Prop]] presented by generators $\mu : 2 \to 1$, $\eta : 0 \to 1$ with the monoid equations (7 Sketches §5.4.2): the *free prop on a monoid*, whose algebras in an SMC are exactly monoid objects — e.g. in $\mathbf{Mat}(R)$ the matrices $(1\ 1)$ and $()$ ; the [[Signal Flow Graph|signal flow]] icons for "add" and "zero".
- A [[Frobenius Monoid]] is a monoid and comonoid on the same object satisfying the Frobenius law; a [[Cartesian Category]] is an SMC where every object is a cocommutative comonoid naturally ([[Discard and Copy Axioms]]).
- Monoid morphisms $f : (M_1, \eta_1, \mu_1) \to (M_2, \eta_2, \mu_2)$ satisfy $f \circ \eta_1 = \eta_2$ and $f \circ \mu_1 = \mu_2 \circ (f \otimes f)$, forming $\mathbf{Mon}(\mathcal{C})$ (DaoFP §10.9), with a forgetful functor to $\mathcal{C}$ and, when $\mathcal{C}$ is nice, a [[Free Monoid|free]] left adjoint.

````tabs
tab: Julia
```julia
# Catlab: a monoid object presented in the free (symmetric) monoidal category
using Catlab
@present MonoidOb(FreeSymmetricMonoidalCategory) begin
  M::Ob
  μ::Hom(M ⊗ M, M)
  η::Hom(munit(), M)
  (μ ⊗ id(M)) ⋅ μ == (id(M) ⊗ μ) ⋅ μ
  (η ⊗ id(M)) ⋅ μ == id(M)
  (id(M) ⊗ η) ⋅ μ == id(M)
end
```
tab: Lean
```lean
#check Mon_                    -- Mon_ C : monoid objects in a monoidal category C (one, mul, one_mul, mul_one, mul_assoc)
#check Comon_
#check CategoryTheory.Monad    -- monads are monoids in [C, C]
```
tab: Haskell
```haskell
-- DaoFP §5.3: a monoid in (Hask, (,), ()) with operations "in bulk"
class MonoidObj m where
  mappend :: (m, m) -> m      -- μ
  mempty  :: () -> m          -- η
-- laws: mappend (mempty (), x) = x = mappend (x, mempty ()); mappend (mappend (x, y), z) = mappend (x, mappend (y, z))
```
````
