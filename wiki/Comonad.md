#definition #theorem #example #program

A **comonad** on $\mathcal{C}$ is a [[Monad]] in $\mathcal{C}^{\mathrm{op}}$: an [[Endofunctor]] $W$ with natural transformations

$$
\varepsilon : W \to \mathrm{Id} \ (\texttt{extract}), \qquad \delta : W \to W \circ W \ (\texttt{duplicate}),
$$

satisfying the counit laws $(\varepsilon \circ W) \cdot \delta = \mathrm{id}_W = (W \circ \varepsilon) \cdot \delta$ and coassociativity $(\delta \circ W) \cdot \delta = (W \circ \delta) \cdot \delta$ — a *comonoid* in the monoidal category of endofunctors. Where monads handle **side effects** via Kleisli arrows $a \to m\, b$, comonads handle **context** ("ntext") via **co-Kleisli arrows** $w\, a \to b$: arrows *out of* a contextualized argument.

```haskell
class Functor w => Comonad w where
  (=<=)     :: (w b -> c) -> (w a -> b) -> (w a -> c)   -- co-Kleisli composition
  extract   :: w a -> a
  duplicate :: w a -> w (w a)                          -- dual of join
  extend    :: (w a -> b) -> w a -> w b                -- dual of bind
-- g =<= f = g . fmap f . duplicate ;  extend f = fmap f . duplicate ;  duplicate = extend id
```

> Sources: DaoFP Chapter 16 ("Comonads": §16.1 "Comonads in Programming", "The Stream comonad", §16.2 "Comonads Categorically", "Comonoids", §16.3 "Comonads from Adjunctions", "Costate comonad", "Comonad coalgebras", "Lenses"), Exercises 16.0.1–16.3.1; §15.2 (dual of monads from adjunctions).

## Examples

- **Environment** `((,) e)`: `g =<= f = \ea -> g (fst ea, f ea)`, `extract = snd` — co-Kleisli arrows `(e, a) -> b` compose by passing the same environment ([[DaoFP Chapter 16 Exercises#Exercise 16.0.1|DaoFP Exercise 16.0.1]]). (Currying the same arrows gives the [[Reader Monad]].)
- **Stream** `data Stream a = Cons a (Stream a)`: `extract` is the head, `duplicate` produces the stream of all tails; `extend f` applies a co-Kleisli arrow needing arbitrary *look-ahead* at every position — a **convolution**. `smooth = extend avg` with `avg` averaging the first five elements is a low-pass filter. Comonads structure computations on spatially or temporally extended data (signal/image processing, PDE simulations, Conway's Game of Life). Bidirectional streams and Gaussian filters: [[DaoFP Chapter 16 Exercises#Exercise 16.1.2|DaoFP Exercise 16.1.2]], [[DaoFP Chapter 16 Exercises#Exercise 16.1.3|DaoFP Exercise 16.1.3]].
- **Signal** `Sig (Double -> a) Double` (a continuous stream plus the current time) and generally the [[Store Comonad]] $L_s R_s c = c^s \times s$ from the currying adjunction; its coalgebras are [[Lens|lenses]].
- Every [[Adjunction]] $L \dashv R$ gives the comonad $(L R, \varepsilon, L \eta R)$ ([[Monads from Adjunctions]]).

## Comonoids

A **comonoid** in a monoidal category is $(w, \delta : w \to w \otimes w, \varepsilon : w \to I)$. In a [[Cartesian Category]] *every* object is a comonoid via the diagonal $\Delta_a$ and $! : a \to 1$ — in Haskell `split w = (w, w)`, `destroy w = ()` — which is why we use variables twice or not at all without thinking. Resources with lifetimes (file handles, memory) should *not* be duplicable or discardable: this is the setting of **linear types** (Rust, Linear Haskell, C++ `unique_ptr`), i.e. a non-cartesian [[Monoidal Closed Category]]. Compare the [[Discard and Copy Axioms]] of resource theories and the special commutative [[Frobenius Monoid|Frobenius]] structure of [[Hypergraph Category|hypergraph categories]].

## Comonad coalgebras

A coalgebra $(a, \phi : a \to W a)$ compatible with the comonad satisfies $\varepsilon_a \circ \phi = \mathrm{id}$ and $W \phi \circ \phi = \delta_a \circ \phi$; these form the (co-)[[Eilenberg-Moore Category]] $\mathcal{C}^W$, with a co-Kleisli subcategory $\mathcal{C}_W$; either reproduces $W$ via an adjunction. For the store comonad the coalgebras are lawful [[Lens|lenses]].

````tabs
tab: Julia
```julia
# the environment comonad (e, a): co-Kleisli composition and extract
extract((e, a)) = a
cokleisli(g, f) = ea -> g((ea[1], f(ea)))
f = ((e, a),) -> a + e; g = ((e, b),) -> b * e
cokleisli(g, f)((10, 1))            # (1 + 10) * 10 = 110
# a finite "stream" comonad on vectors: duplicate = all suffixes, extend = convolution
duplicate(v) = [v[i:end] for i in eachindex(v)]
extend(f, v) = f.(duplicate(v))
avg3(v) = sum(v[1:min(3, end)]) / min(3, length(v))
extend(avg3, [1.0, 2.0, 6.0, 4.0])  # running average with look-ahead
```
tab: Lean
```lean
import Mathlib
open CategoryTheory
#check @CategoryTheory.Comonad               -- ε : W ⟶ 𝟭, δ : W ⟶ W ⋙ W, laws
#check @CategoryTheory.Comonad.Coalgebra     -- comonad coalgebras
#check @CategoryTheory.Adjunction.toComonad  -- L ⋙ R is a comonad
#check @CategoryTheory.Comonad.forget
```
tab: Haskell
```haskell
class Functor w => Comonad w where
  extract   :: w a -> a
  duplicate :: w a -> w (w a)
  extend    :: (w a -> b) -> w a -> w b
  extend f = fmap f . duplicate

instance Comonad ((,) e) where
  extract = snd
  duplicate (e, a) = (e, (e, a))

data Stream a = Cons a (Stream a) deriving Functor
instance Comonad Stream where
  extract (Cons a _) = a
  duplicate s@(Cons _ as) = Cons s (duplicate as)

stmTake :: Int -> Stream a -> [a]
stmTake 0 _ = []
stmTake n (Cons a as) = a : stmTake (n - 1) as
avg :: Stream Double -> Double
avg = (/ 5) . sum . stmTake 5
smooth :: Stream Double -> Stream Double
smooth = extend avg                       -- a low-pass filter by convolution

class Comonoid w where                    -- every type is a comonoid in Hask
  split   :: w -> (w, w)
  destroy :: w -> ()
```
````
