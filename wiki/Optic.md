#definition #example

Let a [[Monoidal Category]] $\mathcal M$ act on categories $\mathcal C$ and $\mathcal D$ ([[Actegory|actegories]] $\bullet$ and $\bullet'$). An **optic** $(A, A') \to (B, B')$ is an element of the [[Coend|coend]]

$$
\mathbf{Optic}_{\bullet,\bullet'}\bigl((A, A'), (B, B')\bigr) \;=\; \int^{M \in \mathcal M} \mathcal C(A,\ M \bullet B) \times \mathcal D(M \bullet' B',\ A').
$$

Concretely: a **forward** map $l : A \to M \bullet B$ that produces the focus $B$ together with a **residual** $M$, and a **backward** map $r : M \bullet' B' \to A'$ that consumes the residual and an update $B'$. The coend quotients by "sliding" a map of residuals from one side to the other, so the residual is private — only $l$ and $r$ together are observable. Composition runs the forwards in order, the backwards in reverse, and pairs the residuals $M \otimes N$.

> Sources: Riley, *Categories of Optics* [arXiv:1809.00738](https://arxiv.org/abs/1809.00738) ([[Categories of Optics|notes]]) Definition 2.0.1 (optics for a monoidal category), Definition 2.2.1 (actions), §4 (lenses, prisms 4.2.1, setters 4.5.1); Clarke, Elkins, Gibbons, Loregian, Milewski, Pillmore & Román, *Profunctor Optics, a Categorical Update* [arXiv:2001.07488](https://arxiv.org/abs/2001.07488) ([[Profunctor Optics - a Categorical Update|notes]]) (mixed optics, Tambara theory); Capucci et al. [arXiv:2105.06332](https://arxiv.org/abs/2105.06332) ([[Towards Foundations of Categorical Cybernetics|notes]]) Definition 8, Remark 9; DaoFP §17.9, §18.

## Examples: the residual decides the optic

| action $M \bullet X$ | optic | residual | reading |
|---|---|---|---|
| $M \times X$ in a cartesian $\mathcal C$ | **lens** | the rest of the record | get/put; [[Lens]], [[Existential Lens]] |
| $M + X$ | **prism** | the other constructors | match/build a variant |
| $X^M$ (exponential) | **grate** | an environment | zip-like access |
| $\mathcal C$ acting on its Kleisli category | **Kleisli optic** | a context with effects | effectful updates |
| $M \otimes X$ in a [[Markov Category]] | **stochastic optic** | a sampled latent | Bayesian updating (St Clere Smithe, [arXiv:2006.01631](https://arxiv.org/abs/2006.01631) ([[Bayesian Updates Compose Optically|notes]])) |

In a **cartesian** category with $\bullet = \times$, the coend can be computed by the [[Yoneda Lemma|co-Yoneda lemma]]: the universal residual is $M = A$ itself, and an optic reduces to a lens $(f : A \to B,\ f^\sharp : A \times B' \to A')$. In a non-cartesian setting (Markov kernels, linear maps, quantum channels) the reduction fails — the residual cannot be taken to be a *copy* of the input — and optics are the correct generalisation of lenses. This is the formal reason [[Bayesian Lens|Bayesian updates]] "compose optically" rather than as plain lenses.

## Profunctor representation

Optics for an action correspond exactly to natural transformations between profunctors that are *Tambara modules* for that action: `Optic (A,A′) (B,B′) ≅ ∀ p. Tambara p ⇒ p B B′ → p A A′` ([[Tambara Module]], [[Profunctor Optics]]). This is what makes optic libraries in Haskell composable with ordinary function composition.

## Optics in categorical cybernetics

Capucci et al. build agents, learners and games as $\mathbf{Para}(\mathbf{Optic}(\mathcal C))$ — [[Para Construction|parametrised]] optics:

- [[Parametric Lens|parametric lenses]] in $\mathbf{Smooth}$ are gradient-based learners;
- [[Open Game|open games]] add a selection functor (best responses) on top;
- [[Bayesian Lens|Bayesian lenses]] live in a stochastic optic category.

````tabs
tab: Julia
**Docs:** plain Julia — Catlab has no dedicated API for this; related: [Catlab v0.16 docs](https://algebraicjulia.github.io/Catlab.jl/v0.16/) · [GATlab standard library](https://algebraicjulia.github.io/GATlab.jl/stable/stdlib/)
```julia
# An optic as (forward with residual, backward consuming it); composition pairs residuals.
struct Optic{L,R}; fwd::L; bwd::R; end            # fwd: a ↦ (m, b);  bwd: (m, b′) ↦ a′
compose(o2::Optic, o1::Optic) = Optic(
    a -> ((m1, b) = o1.fwd(a); (m2, c) = o2.fwd(b); ((m1, m2), c)),
    ((m, c′),) -> o1.bwd((m[1], o2.bwd((m[2], c′)))))
# a lens is an optic whose residual is the whole input (cartesian case)
lens(get, put) = Optic(a -> (a, get(a)), ((a, b′),) -> put(a, b′))
fst_lens = lens(p -> p[1], (p, x) -> (x, p[2]))     # focus on the first component
sq       = lens(x -> x^2, (x, dy) -> 2x * dy)       # reverse derivative of x²
ℓ = compose(sq, fst_lens)
m, y = ℓ.fwd((3.0, :tag))
y == 9.0 && ℓ.bwd((m, 1.0)) == (6.0, :tag)           # true
# a prism: residual is "which branch", here Either-like via Union
prism = Optic(x -> x isa Int ? (:hit, x) : (:miss, x),
              ((m, b′),) -> b′)                       # rebuild from the focus
prism.fwd(4)                                          # (:hit, 4)
```
tab: Lean
```lean
import Mathlib
-- A lens-shaped optic in Type with an explicit residual type (the coend is a quotient of this).
structure Optic (A A' B B' : Type) where
  Res : Type
  fwd : A → Res × B
  bwd : Res × B' → A'

def Optic.comp {A A' B B' C C' : Type} (o₁ : Optic A A' B B') (o₂ : Optic B B' C C') :
    Optic A A' C C' where
  Res := o₁.Res × o₂.Res
  fwd a := let (m₁, b) := o₁.fwd a; let (m₂, c) := o₂.fwd b; ((m₁, m₂), c)
  bwd := fun ((m₁, m₂), c') => o₁.bwd (m₁, o₂.bwd (m₂, c'))
```
tab: Haskell
```haskell
{-# LANGUAGE ExistentialQuantification #-}
-- The existential optic for (Hask, (,)): the residual m is hidden.
data Optic a a' b b' = forall m. Optic (a -> (m, b)) ((m, b') -> a')

(|>) :: Optic a a' b b' -> Optic b b' c c' -> Optic a a' c c'
Optic l1 r1 |> Optic l2 r2 =
  Optic (\a -> let (m1, b) = l1 a; (m2, c) = l2 b in ((m1, m2), c))
        (\((m1, m2), c') -> r1 (m1, r2 (m2, c')))

lens :: (a -> b) -> (a -> b' -> a') -> Optic a a' b b'
lens get put = Optic (\a -> (a, get a)) (\(a, b') -> put a b')

view :: Optic a a' b b' -> a -> b
view (Optic l _) = snd . l
```
````
