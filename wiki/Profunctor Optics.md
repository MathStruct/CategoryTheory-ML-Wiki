#theorem #definition #example #program

**Profunctor optics** represent an optic (lens, prism, traversal, iso, ...) as a function polymorphic over a class of [[Profunctor|profunctors]]; composition of optics is then ordinary function composition — like representing rotations by matrices so that composing them is matrix multiplication. The master formula, from [[Tannakian Reconstruction]] over a free/forgetful adjunction $F \dashv U$ between a category $\mathcal{T}$ of structured profunctors and all profunctors, with $\Phi = U F$:

$$
\mathcal{O}\langle s, t\rangle\langle a, b\rangle = \int_{P : \mathcal{T}} \mathbf{Set}\big(P\langle a, b\rangle,\ P\langle s, t\rangle\big) \cong \big(\Phi\,(\mathcal{C}^{\mathrm{op}} \times \mathcal{C})(\langle a, b\rangle, -)\big)\langle s, t\rangle .
$$

> Sources: DaoFP §18.2 ("Profunctor Lenses": "Iso", "Profunctor lenses", "Profunctor lenses in Haskell"), §18.3 ("General Optics": "Prisms", "Traversals"), §18.4 ("Mixed Optics"), Exercises 18.2.1, 18.4.1; §17.9 ([[Existential Lens]]).

| optic | $\mathcal{T}$ | existential / concrete form | profunctor form |
|---|---|---|---|
| **Iso** (adapter) | all profunctors | $\mathcal{C}(s, a) \times \mathcal{C}(b, t)$: `(s -> a, b -> t)` | `forall p. Profunctor p => p a b -> p s t` |
| **Lens** | [[Tambara Module|Tambara]] for $\times$ (`Cartesian`) | $\int^c \mathcal{C}(s, c \times a) \times \mathcal{C}(c \times b, t)$; `get, set` | `forall p. Cartesian p => p a b -> p s t` |
| **Prism** | Tambara for $+$ (`Cocartesian`) | $\int^c \mathcal{C}(s, c + a) \times \mathcal{C}(c + b, t) \cong \mathcal{C}(s, t + a) \times \mathcal{C}(b, t)$; `match :: s -> Either t a`, `build :: b -> t` | `forall p. Cocartesian p => p a b -> p s t` |
| **Traversal** | generalized Tambara for $c \bullet a = \sum_m c_m \times a^m$ | $\mathbf{Set}(s, \sum_n \mathbf{Set}(b^n, t) \times a^n)$; `s -> ([b] -> t, [a])` (sizes must match — really needs [[Dependent Type|dependent types]]) | `forall p. Traversing p => p a b -> p s t` |

- **Iso**: `toIsoP (f, g) = dimap f g`; conversely a function that maps $P\langle a,b\rangle \to P\langle s,t\rangle$ for *every* profunctor can only be a closure over a pair `(s -> a, b -> t)` — recovered by feeding the profunctor `Adapter a b s t = (s -> a, b -> t)` at the identities ([[DaoFP Exercise 18.2.1]]).
- **Lens**: `toLensP (LensE from to) = dimap from to . alpha`; back via the Cartesian profunctor `FlipLens a b s t = (s -> a, s -> b -> t)` fed with `FlipLens id (\_ b -> b)`. Composition: `lens3 = lens2 . lens1`.
- **Prism**: `s` either contains the focus `a` or a residue `c`; mapping out of a sum turns the coend into $\mathcal{C}(s, t + a) \times \mathcal{C}(b, t)$ by co-Yoneda. `toPrismP (Prism from to) = dimap from to . alpha'`.
- **Traversal**: residues $c_n$ with $n$ holes form a functor $\mathbb{N} \to \mathcal{C}$; the actions compose by [[Day Convolution]] on $[\mathbb{N}, \mathcal{C}]$, and the general Tambara derivation goes through unchanged. **Mixed optics** use an actegory acting on two categories.

````tabs
tab: Julia
```julia
# profunctor lens on the function profunctor: p a b -> p s t with p = (->), i.e. an "over"
alpha(f) = ((c, a),) -> (c, f(a))
dimap(l, r, h) = r ∘ h ∘ l
# LensE (c, a) (c, b) a b for the second component of a pair, as a profunctor lens
lensP(h) = dimap(identity, identity, alpha(h))
over_second = lensP(x -> x * 10)
over_second(("tag", 4))                # ("tag", 40)
# lenses compose by function composition
over_inner = lensP(lensP(x -> x + 1))
over_inner(("a", ("b", 1)))            # ("a", ("b", 2))
```
tab: Lean
```lean
import Mathlib
-- the profunctor representation instantiated at p := (· → ·) gives "modify the focus"
def overSnd (h : α → β) : γ × α → γ × β := fun ⟨c, a⟩ => (c, h a)
example : overSnd (· * 10) ("tag", 4) = ("tag", 40) := rfl
```
tab: Haskell
```haskell
{-# LANGUAGE RankNTypes, GADTs #-}
type IsoP s t a b = forall p. Profunctor p => p a b -> p s t
toIsoP :: (s -> a, b -> t) -> IsoP s t a b
toIsoP (f, g) = dimap f g

type LensP s t a b = forall p. Cartesian p => p a b -> p s t
toLensP :: LensE s t a b -> LensP s t a b
toLensP (LensE from to) = dimap from to . alpha

data FlipLens a b s t = FlipLens (s -> a) (s -> b -> t)
instance Profunctor (FlipLens a b) where
  dimap f g (FlipLens get set) = FlipLens (get . f) (fmap g . set . f)
instance Cartesian (FlipLens a b) where
  alpha (FlipLens get set) = FlipLens (get . snd) (\(x, s) b -> (x, set s b))
fromLensP :: LensP s t a b -> (s -> a, s -> b -> t)
fromLensP pp = (get', set') where FlipLens get' set' = pp (FlipLens id (\_ b -> b))

data Prism s t a b where
  Prism :: (s -> Either c a) -> (Either c b -> t) -> Prism s t a b
toMatch :: Prism s t a b -> (s -> Either t a)
toMatch (Prism from to) s = case from s of
  Left c  -> Left (to (Left c))
  Right a -> Right a
toBuild :: Prism s t a b -> (b -> t)
toBuild (Prism _ to) b = to (Right b)
type PrismP s t a b = forall p. Cocartesian p => p a b -> p s t
toPrismP :: Prism s t a b -> PrismP s t a b
toPrismP (Prism from to) = dimap from to . alpha'
```
````
