#definition #theorem #example #program

**Day convolution** transports a monoidal structure $(\mathcal{C}, \otimes, I)$ to the category of co-presheaves $[\mathcal{C}, \mathbf{Set}]$, in analogy with the convolution $(f \star g)(x) = \int f(y)\, g(x - y)\, dy$ — the [[Coend]] replaces the integral and the hom-functor $\mathcal{C}(a \otimes b, x)$ plays the Dirac delta enforcing "$a \otimes b = x$":

$$
(F \star G)\, x = \int^{a, b} \mathcal{C}(a \otimes b, x) \times F a \times G b .
$$

It is associative up to isomorphism and its unit is $\mathcal{C}(I, -)$ (by the [[Ninja Yoneda Lemma|co-Yoneda lemma]]: "ONE of DAY is the YONEDA of ONE"); it is symmetric if $\otimes$ is. In a [[Cartesian Closed Category]] currying simplifies it to $\int^b F(x^b) \times G b$, and in Haskell
```haskell
data Day f g x where
  Day :: ((a, b) -> x) -> f a -> g b -> Day f g x     -- combine two containers given a way to combine values
```

> Sources: DaoFP §17.7 ("Day Convolution", "Applicative functors as monoids", "Free Applicatives"), Exercises 17.7.1–17.7.3; §14.9.

**Applicative functors as monoids.** A [[Monoid Object]] in $([\mathcal{C}, \mathbf{Set}], \star, \mathcal{C}(I, -))$ is a functor $F$ with $\eta : \mathcal{C}(1, -) \to F$, i.e. `pure :: a -> f a`, and $\mu : F \star F \to F$, which by co-continuity, currying and Yoneda is an element of $\int_{a,b} \mathbf{Set}(F a \times F b, F(a \times b))$, i.e. `(>*<) :: f a -> f b -> f (a, b)` — exactly a lax [[Monoidal Functor]] / [[Applicative Functor]].

**Free applicative.** The free monoid for $\star$ is the [[Initial Algebra]] of $\Phi_F G = \mathcal{C}(I, -) + F \star G$, giving the "list of functorfuls"
```haskell
data FreeA f x where
  DoneA :: x -> FreeA f x
  MoreA :: ((a, b) -> x) -> f a -> FreeA f b -> FreeA f x
```
whose `Monoidal` instance is list concatenation (`DoneA x >*< fry = fmap (x,) fry`; `MoreA abx fa frb >*< fry = MoreA (reassoc abx) fa (frb >*< fry)`), from which `pure = DoneA`, `ff <*> fx = fmap app (ff >*< fx)` ([[DaoFP Exercise 17.7.3]]). Compare the [[Free Monad]] for functor composition.

````tabs
tab: Julia
```julia
# Day convolution of two "containers" (vectors) with a combining function: an existential triple
struct Day; combine::Function; fa::Vector; gb::Vector; end
# the canonical map Day f g x -> Vector x when f = g = Vector (cartesian applicative)
collapse(d::Day) = [d.combine((a, b)) for a in d.fa for b in d.gb]
collapse(Day(((a, b),) -> a + b, [1, 2], [10, 20]))     # [11, 21, 12, 22]
```
tab: Lean
```lean
import Mathlib
-- no Day convolution in Mathlib; the Haskell existential becomes a Σ-type
structure Day (f g : Type → Type) (x : Type) where
  {a b : Type}
  combine : a × b → x
  fa : f a
  gb : g b
```
tab: Haskell
```haskell
{-# LANGUAGE GADTs, TupleSections #-}
data Day f g x where
  Day :: ((a, b) -> x) -> f a -> g b -> Day f g x
instance Functor (Day f g) where                        -- Exercise 17.7.1
  fmap h (Day abx fa gb) = Day (h . abx) fa gb
assoc :: Day f (Day g h) x -> Day (Day f g) h x         -- Exercise 17.7.2
assoc (Day abx fa (Day cdb gc hd)) = Day (\((a, c), d) -> abx (a, cdb (c, d))) (Day (,) fa gc) hd

data FreeA f x where
  DoneA :: x -> FreeA f x
  MoreA :: ((a, b) -> x) -> f a -> FreeA f b -> FreeA f x
instance Functor f => Functor (FreeA f) where            -- Exercise 17.7.3
  fmap h (DoneA x) = DoneA (h x)
  fmap h (MoreA abx fa frb) = MoreA (h . abx) fa frb
reassoc :: ((a, b) -> x) -> (a, (b, y)) -> (x, y)
reassoc abx (a, (b, y)) = (abx (a, b), y)
```
````
