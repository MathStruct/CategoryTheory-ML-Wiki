#definition #example

A **bifunctor** is a [[Functor]] $F : \mathcal{C} \times \mathcal{D} \to \mathcal{E}$ out of a [[Product Category]]: it maps a pair of objects to an object and a pair of arrows to an arrow, functorially in each variable. In Haskell: `class Bifunctor f where bimap :: (a -> a') -> (b -> b') -> (f a b -> f a' b')`.

> Sources: DaoFP §8.3 ("Bifunctors"), Exercise 8.3.4; 7 Sketches §4.4.3 (the monoidal product $\otimes : \mathcal{C} \times \mathcal{C} \to \mathcal{C}$ is a functor).

Examples: the [[Product]] `(,)` with `bimap g h (a, b) = (g a, h b)`; the [[Coproduct]] `Either`; `MoreThanA a b = More a (Maybe b)`; the tensor product $\otimes$ of any [[Monoidal Category]]; the [[Hom Functor]] is a *mixed-variance* bifunctor $\mathcal{C}^{\mathrm{op}} \times \mathcal{C} \to \mathbf{Set}$, i.e. a [[Profunctor]]. Functoriality of sums and products is discussed in DaoFP §4.4, §5.1 ("Functoriality").

````tabs
tab: Lean
```lean
-- a bifunctor is a functor out of a product category, or a curried functor C ⥤ D ⥤ E
#check CategoryTheory.Functor.prod'   -- hmm: see `CategoryTheory.uncurry`, `CategoryTheory.curry`
#check CategoryTheory.curry           -- (C × D ⥤ E) ⥤ (C ⥤ D ⥤ E)
```
tab: Haskell
```haskell
class Bifunctor f where
  bimap :: (a -> a') -> (b -> b') -> (f a b -> f a' b')

instance Bifunctor (,) where
  bimap g h (a, b) = (g a, h b)

instance Bifunctor Either where
  bimap g _ (Left a)  = Left (g a)
  bimap _ h (Right b) = Right (h b)

data MoreThanA a b = More a (Maybe b)
instance Bifunctor MoreThanA where
  bimap g h (More a mb) = More (g a) (fmap h mb)
```
````
