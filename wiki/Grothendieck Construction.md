#definition #theorem #example

Given an **indexed category** — a pseudofunctor $F : \mathcal C^{\mathrm{op}} \to \mathbf{Cat}$ assigning a category $F(c)$ to each object and a *reindexing* functor $f^* := F(f) : F(c') \to F(c)$ to each $f : c \to c'$ — the **Grothendieck construction** $\int F$ is the category whose

- objects are pairs $(c, x)$ with $c \in \mathcal C$ and $x \in F(c)$;
- morphisms $(c, x) \to (c', x')$ are pairs $(f, \varphi)$ with $f : c \to c'$ in $\mathcal C$ and $\varphi : x \to f^* x'$ in $F(c)$;
- composition is $(g, \psi) \circ (f, \varphi) = (g \circ f,\ f^*\psi \circ \varphi)$ (using $f^* g^* \cong (gf)^*$).

The projection $p : \int F \to \mathcal C$, $(c, x) \mapsto c$, is a **Grothendieck fibration**: every morphism $f : c \to p(e')$ has a *cartesian lift* ending at $e'$, the universal way of pulling $e'$ back along $f$. The theorem is that this is an equivalence:

$$
\{\text{pseudofunctors } \mathcal C^{\mathrm{op}} \to \mathbf{Cat}\} \;\simeq\; \{\text{fibrations over } \mathcal C\},
$$

with the fibre $p^{-1}(c)$ recovering $F(c)$. For a *set*-valued functor this is the [[Category of Elements]], and the fibres are discrete.

> Sources: Grothendieck, SGA 1 exposé VI; Jacobs, *Categorical Logic and Type Theory* ch. 1; Spivak, *Generalized Lens Categories via functors $\mathcal C^{\mathrm{op}} \to \mathsf{Cat}$* [arXiv:1908.02202](https://arxiv.org/abs/1908.02202) ([[Generalized Lens Categories via Functors from Cop to Cat|notes]]); St Clere Smithe, *Bayesian Updates Compose Optically* [arXiv:2006.01631](https://arxiv.org/abs/2006.01631) ([[Bayesian Updates Compose Optically|notes]]) §3; Braithwaite, Hedges & St Clere Smithe [arXiv:2305.06112](https://arxiv.org/abs/2305.06112) ([[The Compositional Structure of Bayesian Inference|notes]]) Definitions 8–9, 12; Cockett et al., *Reverse derivative categories* [arXiv:1910.07065](https://arxiv.org/abs/1910.07065) ([[Reverse Derivative Categories|notes]]) Definitions 25–28.

## Grothendieck lenses: the *fibrewise opposite*

Spivak's observation (1908.02202) is that taking the Grothendieck construction of the **fibrewise opposite** $F^{\mathrm{op}} : c \mapsto F(c)^{\mathrm{op}}$ yields categories of bidirectional morphisms. A morphism $(c, x) \to (c', x')$ in $\int F^{\mathrm{op}}$ is a forward map $f : c \to c'$ together with a **backward** map $f^\sharp : f^* x' \to x$. Special cases:

| indexed category $F$ | $\int F^{\mathrm{op}}$ | backward part |
|---|---|---|
| $F(A) = \mathcal C/\!\!/A$, the **simple fibration**: objects of $\mathcal C$, maps $A \times X \to Y$ | cartesian [[Lens|lenses]] $\mathbf{Lens}(\mathcal C)$ | $f^\sharp : A \times B' \to A'$ |
| $F(X) = \mathbf{Stat}(X)$: maps are functions $\mathcal C(I, X) \to \mathcal C(A, B)$ | [[Bayesian Lens|Bayesian lenses]] | a *state-dependent* kernel $\mathcal C(I, X) \to \mathcal C(B, A)$ |
| state-indexed families | dependent Bayesian lenses | kernels whose type depends on the state |
| the linear-maps fibration of a differential category | reverse-derivative lenses | $R[f](a, -)$ linear in the second argument |

So **"lens" is a Grothendieck construction whose fibres are opposite categories**: the forward pass lives in the base, the backward pass lives in the fibre over the point the forward pass was computed at. That is why a lens's backward map always takes the forward input as an extra argument — it is indexed by it.

## Fibrations and sections

A **section** of $p : \mathcal E \to \mathcal C$ is a functor $s : \mathcal C \to \mathcal E$ with $p s = 1$. For a lens-shaped fibration, a section is "a choice of backward pass for every forward map", and functoriality of $s$ is a **chain rule**:

- reverse differentiation $R : \mathcal C \to \mathbf{Lens}(\mathcal C)$ is a (strict) section ([[Reverse Derivative Category]], Cruttwell et al. Proposition 2.7);
- Bayesian inversion $T : \mathcal C \to \mathbf{BLens}(\mathcal C)$ is a section *up to almost-sure equality* (Braithwaite et al., Proposition 11; [[Bayesian Inversion]]);
- gradient assignment for parameterized statistical games is only a **lax section** (AutoBayes Remark 30; [[Lax Functor]]).

## Examples

- $F = \mathcal C(-, \mathbf 2)$-style predicates: the **subobject fibration** of a topos, whose fibres are the posets of predicates on each object ([[Subobject Classifier]]).
- The **codomain fibration** $\mathrm{cod} : \mathcal C^{\to} \to \mathcal C$ has fibres the [[Slice Category|slices]] $\mathcal C/c$; reindexing is pullback. This is the setting of [[Dependent Type|dependent types]] and [[Base Change Functor|base change]].
- A monoid action $M \times S \to S$, viewed as a functor $\mathbf BM \to \mathbf{Set}$, has as Grothendieck construction the action groupoid/category ([[Monoid Action]]).

````tabs
tab: Julia
**Docs:** plain Julia — Catlab has no dedicated API for this; related: [Catlab v0.16 docs](https://algebraicjulia.github.io/Catlab.jl/v0.16/) · [GATlab standard library](https://algebraicjulia.github.io/GATlab.jl/stable/stdlib/)
```julia
# ∫F for a set-valued indexed family: objects are pairs (c, x ∈ F(c)).
# Here F = fibres of a function π : E → C, i.e. the category of elements of a discrete fibration.
C = [:boat, :car]
F = Dict(:boat => [:sail, :motor], :car => [:petrol, :electric, :hybrid])
total = [(c, x) for c in C for x in F[c]]          # objects of ∫F
length(total)                                       # 5
# The simple fibration: a lens (A,A′) → (B,B′) is a base map f and a fibre map f♯ : A × B′ → A′.
struct SimpleLens{G,P}; get::G; put::P; end
compose(ℓ2::SimpleLens, ℓ1::SimpleLens) =
    SimpleLens(ℓ2.get ∘ ℓ1.get, (a, c′) -> ℓ1.put(a, ℓ2.put(ℓ1.get(a), c′)))
square = SimpleLens(x -> x^2, (x, dy) -> 2x * dy)   # the reverse derivative of x ↦ x²
sine   = SimpleLens(sin, (x, dy) -> cos(x) * dy)
ℓ = compose(sine, square)                           # x ↦ sin(x²) with its backward pass
ℓ.put(1.3, 1.0) ≈ cos(1.3^2) * 2 * 1.3              # the chain rule, as fibre composition: true
```
tab: Lean
```lean
import Mathlib
open CategoryTheory
#check Grothendieck                 -- ∫ F for F : C ⥤ Cat (covariant convention)
#check @Grothendieck.forget         -- the projection ∫ F ⥤ C
#check Functor.Elements             -- the set-valued case: the category of elements
```
tab: Haskell
```haskell
-- The simple fibration's Grothendieck construction: lenses with a fibre-indexed backward map.
data Lens a a' b b' = Lens { get :: a -> b, put :: a -> b' -> a' }

(|>) :: Lens a a' b b' -> Lens b b' c c' -> Lens a a' c c'
Lens f f' |> Lens g g' = Lens (g . f) (\a c' -> f' a (g' (f a) c'))

square, sine :: Lens Double Double Double Double
square = Lens (^ 2) (\x dy -> 2 * x * dy)
sine   = Lens sin (\x dy -> cos x * dy)
-- put (square |> sine) 1.3 1.0 == cos (1.3^2) * 2 * 1.3
```
````
