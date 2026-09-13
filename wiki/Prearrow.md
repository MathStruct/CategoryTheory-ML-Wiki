#definition #example

A **pre-arrow** is a [[Monad]] in the [[Bicategory of Profunctors]] $\mathbf{Prof}$: a [[Profunctor]] `p` with a composition `(>>>) :: p a x -> p x b -> p a b` (the multiplication $\mu : P \diamond P \to P$) and an embedding of ordinary functions `arr :: (a -> b) -> p a b` (the unit $\eta : \mathcal{C}(-, =) \to P$), satisfying associativity and unit laws. Haskell's `Arrow` class is a pre-arrow that is also a [[Tambara Module]] (`first :: p a b -> p (a, c) (b, c)`).

> Sources: DaoFP §17.8 ("Prearrows as monads in Prof"), §18 (Tambara modules and arrows).

- Examples: functions `(->)`; [[Kleisli Category|Kleisli arrows]] `Kleisli m a b = a -> m b` of a monad; `Star f` and `Costar f`.

````tabs
tab: Haskell
```haskell
newtype Kleisli m a b = Kleisli (a -> m b)
instance Monad m => Profunctor (Kleisli m) where
  dimap f g (Kleisli h) = Kleisli (fmap g . h . f)
instance Monad m => PreArrow (Kleisli m) where
  Kleisli f >>> Kleisli g = Kleisli (\a -> f a >>= g)
  arr f = Kleisli (return . f)
```
````
