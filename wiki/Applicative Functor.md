#definition #theorem #example #program

An **applicative functor** answers: a functor lifts one-argument functions; how do we lift functions of several arguments? `fmap` of a curried `a -> b -> c` over `f a` gives `f (b -> c)`, so what is needed is *application of a functorful of functions*:
```haskell
class Functor f => Applicative f where
  pure  :: a -> f a                     -- lift a zero-argument function (a value)
  (<*>) :: f (a -> b) -> f a -> f b     -- "splat"
liftA2 g as bs = g <$> as <*> bs
```
with laws: identity `pure id <*> v = v`, homomorphism `pure f <*> pure x = pure (f x)`, interchange `u <*> pure y = pure ($ y) <*> u`, composition `pure (.) <*> u <*> v <*> w = u <*> (v <*> w)`.

> Sources: DaoFP §14.9 ("Applicative functors", "Closed functors", "Monads and applicatives"), Exercises 14.9.1–14.9.3; §17.7 (applicatives as monoids under [[Day Convolution]]).

- **Lax closed functor**: the splat `f (a -> b) -> (f a -> f b)` is a natural transformation $F(b^a) \to (F b)^{F a}$ — the lax version of preserving [[Exponential Object|exponentials]]. In a [[Cartesian Closed Category]] lax closed = lax [[Monoidal Functor|monoidal]] (`class Monoidal f where unit :: f (); (>*<) :: f a -> f b -> f (a, b)`): `pure a = fmap (const a) unit; fs <*> as = fmap apply (fs >*< as)` and conversely `unit = pure (); as >*< bs = (,) <$> as <*> bs`.
- Every [[Monad]] (being [[Functorial Strength|strong]]) is applicative: `ap fs as = do { f <- fs; a <- as; return (f a) }`, whence `Applicative` is a superclass of `Monad` with `return = pure`. Not every applicative is a monad: the zip instance of lists (`pure = repeat`, `fs <*> as = zipWith ($) fs as`, [[DaoFP Exercise 14.9.3]]).
- Monads are more powerful — monadic code can branch on the contents of a value — but applicative composition has no dependencies between parts, so it can run in parallel (Haskell's parallel libraries; `ApplicativeDo`).

````tabs
tab: Julia
```julia
# applicative structure on lists (cartesian) and the zip alternative
pure(a) = [a]
splat(fs, as) = [f(a) for f in fs for a in as]
splat([x -> x + 1, x -> 2x], [10, 20])            # [11, 21, 20, 40]
zipsplat(fs, as) = [f(a) for (f, a) in zip(fs, as)]
zipsplat([x -> x + 1, x -> 2x], [10, 20])         # [11, 40]
liftA2(g, as, bs) = splat([b -> g(a, b) for a in as], bs)
```
tab: Lean
```lean
import Mathlib
#check @Applicative                -- class with pure and seq (<*>)
#check @Applicative.seq
#check @LawfulApplicative          -- the applicative laws
example : List ℕ := (· + ·) <$> [1, 2] <*> [10, 20]     -- [11, 21, 12, 22]
```
tab: Haskell
```haskell
class Functor f => Monoidal f where
  unit  :: f ()
  (>*<) :: f a -> f b -> f (a, b)

instance Monoidal [] where                       -- Exercise 14.9.1
  unit = [()]
  as >*< bs = [ (a, b) | a <- as, b <- bs ]

liftA3 :: Applicative f => (a -> b -> c -> d) -> f a -> f b -> f c -> f d   -- Exercise 14.9.2
liftA3 g as bs cs = g <$> as <*> bs <*> cs

newtype ZipList a = ZipList [a]
instance Functor ZipList where fmap f (ZipList as) = ZipList (map f as)
instance Applicative ZipList where               -- applicative but not a monad
  pure = ZipList . repeat
  ZipList fs <*> ZipList as = ZipList (zipWith ($) fs as)
```
````
