#definition #example #theorem

A **forgetful functor** $U$ "forgets" structure: its action on hom-sets is not surjective, because arrows in the source must preserve structure that is absent in the target (typically $\mathbf{Set}$, "the epitome of structurelessness"; $U(m)$ is the *underlying set*). A **free functor** is a left adjoint $F \dashv U$: $\mathbf{Mon}(FX, m) \cong \mathbf{Set}(X, Um)$. "The picture of an adjunction is not symmetric; nowhere is this better illustrated than in free/forgetful adjunctions."

> Sources: DaoFP §10.9 ("Free/Forgetful Adjunctions", "The category of monoids", "Free monoid", "Free monoid in programming"), Exercises 10.9.1–10.9.2, §15.3; 7 Sketches Example 3.74; Kittenlab Lecture 5 (the functors $F : \mathbf{Set} \to \mathbf{Mon}$, $U : \mathbf{Mon} \to \mathbf{Set}$), Lecture 7 (the unit $\eta_X : X \to UFX$).

## The free monoid (DaoFP)

To match every function $f : X \to Um$ with a monoid homomorphism $g : FX \to m$, $FX$ must be *much larger* than $X$: start with the **generators** $X$ (where $g = f$), add a fresh unit $e$ (mapped to the unit of $m$ — a generator cannot serve, that would constrain $f$), add all products of generators as new elements (with $g(a \cdot b) = g(a) \cdot g(b)$), and only simplify by the monoid laws. The result: $FX = X^*$, **strings over the alphabet $X$**, unit the empty string, multiplication concatenation — automatically associative and unital ([[Free Monoid]]). Free functors "generate structure freely — with no additional constraints — and lazily: instead of performing operations they just record them", creating a domain-specific program executed later by an interpreter (`foldMap`). Unit: $x \mapsto [x]$; counit: fold/evaluate a list of elements of $m$ ([[DaoFP Chapter 10 Exercises#Exercise 10.9.1|DaoFP Exercise 10.9.1]]).

## Other examples (7 Sketches Example 3.74)

Free [[Group|group]], free ring, free vector space (left adjoints to forgetting); [[Free Category|free category]] and free [[Preorder|preorder]] on a [[Graph]] (left adjoints to the underlying graph); [[Discrete Category|discrete]] preorder/graph/metric space/category/topological space (left adjoints to underlying set), while [[Codiscrete Category|codiscrete]] things are *right* adjoints; abelianization $G \mapsto G/[G, G]$ (left adjoint to $\mathbf{Ab} \hookrightarrow \mathbf{Grp}$); [[Free Prop]] on a signature (7 Sketches §5.2.4); [[Free Monad]] on a functor (DaoFP §14.8); [[Reflexive Transitive Closure]] $\mathrm{Cl} \dashv U$ for preorders on a set (7 Sketches §1.4.5).

The [[Monad]] $UF$ of the free monoid adjunction is the [[List Monad]] (DaoFP §15.3); in general every free/forgetful adjunction generates a monad, and the [[Eilenberg-Moore Category]] of that monad recovers the algebraic structures.

````tabs
tab: Julia
```julia
# the free monoid on a set of generators as vectors; the universal property via foldMap
free_monoid_hom(f, mul, e) = xs -> foldl((acc, x) -> mul(acc, f(x)), xs; init = e)
g = free_monoid_hom(x -> x^2, +, 0)      # Set → (ℕ, +, 0), extended from the generators
g([1, 2, 3])                              # 14
```
tab: Lean
```lean
#check FreeMonoid            -- FreeMonoid α ≃ List α
#check FreeMonoid.lift       -- (α → M) ≃ (FreeMonoid α →* M): the adjunction bijection
#check CategoryTheory.Adjunction  -- MonCat.adj : free ⊣ forget (Mathlib.Algebra.Category.MonCat.Adjunctions)
#check MonCat.adj
```
tab: Haskell
```haskell
-- DaoFP §10.9: lists are the free monoid; foldMap is the interpreter (the adjunction bijection)
class Monoid m where
  mappend :: m -> m -> m
  mempty  :: m

instance Monoid [a] where
  mempty  = []
  mappend = (++)

foldMap :: Monoid m => (a -> m) -> ([a] -> m)
foldMap f = foldr mappend mempty . fmap f

-- Exercise 10.9.2: interpret a list of Ints additively and multiplicatively
newtype Sum' = Sum' Int; newtype Prod' = Prod' Int
instance Monoid Sum'  where mempty = Sum' 0;  mappend (Sum' a) (Sum' b) = Sum' (a + b)
instance Monoid Prod' where mempty = Prod' 1; mappend (Prod' a) (Prod' b) = Prod' (a * b)
```
````
