#example #definition #program #theorem

The **list monad** encodes nondeterminism (many-worlds: return all results at once):
```haskell
instance Monad [] where
  as >>= k = concat (fmap k as)      -- join = concat
  return a = [a]                     -- determinism is trivial nondeterminism
```
Nested loops of imperative languages become binds: `as` aggregates the inner loop, `k` is the outer loop body. Thanks to laziness a Haskell list behaves like an iterator/generator.

> Sources: DaoFP §14.1 ("Nondeterminism"), §14.4, §14.5 (`pairs`), §15.3 ("Free monoid and the list monad"), §14.8 ("the list monad is not free because its join irreversibly smashes the lists together"), §14.9 (the two `Applicative` instances: cartesian and zip); Kittenlab Lecture 7 ([[Free-Forgetful Adjunction]]); 7 Sketches §5.2.4.

**From the free monoid adjunction** ([[Monads from Adjunctions]], [[Free Monoid]]): $F \dashv U$ between $\mathbf{Set}$ and $\mathbf{Mon}$; $\eta_X : X \to U F X$ sends $x$ to the singleton `[x]` (`return`); the counit $\varepsilon_M : F U M \to M$ is the monoid morphism `foldMap id = foldr mappend mempty`, one direction of $\mathbf{Set}(a, U m) \cong \mathbf{Mon}(F a, m)$; whiskering, $\mu = U \varepsilon F$ instantiates $\varepsilon$ at the free monoid `([], (++))`, giving `join = foldr (++) [] = concat`.

- `pairs as bs = do { a <- as; b <- bs; return (a, b) }` enumerates all pairs ([[Do Notation]], [[DaoFP Exercise 14.5.2]]).
- Two [[Applicative Functor]] structures: the monadic one applies every function to every argument; the *zip* one (`pure = repeat`, `fs <*> as = zipWith ($) fs as`) is applicative but not a monad ([[DaoFP Exercise 14.9.3]]). The `Monoidal` instance is the cartesian product of lists ([[DaoFP Exercise 14.9.1]]).

````tabs
tab: Julia
```julia
ret(a) = [a]
bind(as, k) = reduce(vcat, (k(a) for a in as); init=Any[])   # concat ∘ map
pairs(as, bs) = bind(as, a -> bind(bs, b -> ret((a, b))))
pairs(1:2, 'a':'b')                    # [(1,'a'), (1,'b'), (2,'a'), (2,'b')]
# Catlab/Kittenlab: the free monoid on a set is the list monoid, μ = concat
```
tab: Lean
```lean
import Mathlib
#check @List.bind            -- l.bind f = (l.map f).join
#check @List.join
#check @List.singleton       -- return; Lean's List is an instance of Monad
example : List (ℕ × Char) := do let a ← [1, 2]; let b ← ['a', 'b']; pure (a, b)
```
tab: Haskell
```haskell
pairs :: [a] -> [b] -> [(a, b)]
pairs as bs = do
  a <- as
  b <- bs
  return (a, b)

epsilon :: Monoid m => [m] -> m          -- the counit of F ⊣ U
epsilon = foldr mappend mempty
joinL :: [[a]] -> [a]
joinL = foldr (++) []                    -- = concat
```
````
