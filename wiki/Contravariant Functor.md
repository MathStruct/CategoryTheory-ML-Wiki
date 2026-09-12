#definition #example

A **contravariant functor** is a [[Functor]] $F : \mathcal{C}^{\mathrm{op}} \to \mathcal{D}$: it lifts arrows in the opposite direction, $f : a \to b$ to $F(f) : F(b) \to F(a)$, and reverses composition. Ordinary functors are **covariant**. In Haskell: `class Contravariant f where contramap :: (b -> a) -> (f a -> f b)`.

> Sources: DaoFP §8.3 ("Contravariant functors"), §8.4 ($\mathcal{C}(-, b)$), §9.6 (contravariant Yoneda); Kittenlab Lecture 12, 14 ($\mathcal{P}$ via preimage); 7 Sketches §7.3.1 ([[Presheaf|presheaves]]).

- **Producers vs. consumers** (DaoFP): a covariant functor is a *producer* of `a`s (turn it into a producer of `b`s with `fmap` and `a -> b`); a contravariant functor is a *consumer* of `a`s (you need `b -> a`). `Predicate a = a -> Bool` is contravariant: `contramap f (Predicate h) = Predicate (h . f)`. "In practice, the only non-trivial contravariant functors are variations on function objects."
- **Polarity**: the return type of a function is in *positive* (covariant) position, the argument in *negative* (contravariant) position; nesting in a negative position flips polarities. `Tester a = (a -> Bool) -> Bool` has `a` in double-negative, hence positive, position and is covariant; `a -> Bool -> Bool` has `a` negative.
- Examples: the contravariant [[Hom Functor]] $\mathcal{C}(-, b)$ ("$b$ as seen by the world"), whose action is pre-composition; [[Presheaf|presheaves]] $\mathcal{C}^{\mathrm{op}} \to \mathbf{Set}$ and the [[Yoneda Embedding]] $x \mapsto \mathcal{C}(-, x)$; the [[Power Set]] functor via preimage, $\mathcal{P} : \mathbf{Set}^{\mathrm{op}} \to \mathbf{Pos}$ (Kittenlab); [[Upper Set|pullback of upper sets]] $f^*$; the functor $\mathrm{Op}(X) \to \mathbf{Set}$ of a [[Sheaf]] on a topological space.
- Composing a covariant functor after a contravariant one gives a contravariant functor ([[DaoFP Exercise 8.5.1]]).

````tabs
tab: Lean
```lean
-- a contravariant functor is a functor out of the opposite category
example {C D : Type} [CategoryTheory.Category C] [CategoryTheory.Category D] :
    Type _ := Cᵒᵖ ⥤ D
#check CategoryTheory.yoneda      -- C ⥤ (Cᵒᵖ ⥤ Type v): x ↦ Hom(-, x)
```
tab: Haskell
```haskell
class Contravariant f where
  contramap :: (b -> a) -> (f a -> f b)

newtype Predicate a = Predicate (a -> Bool)
instance Contravariant Predicate where
  contramap f (Predicate h) = Predicate (h . f)

newtype Tester a = Tester ((a -> Bool) -> Bool)   -- covariant: a is in double-negative position
instance Functor Tester where
  fmap f (Tester g) = Tester (\h -> g (h . f))
```
````
