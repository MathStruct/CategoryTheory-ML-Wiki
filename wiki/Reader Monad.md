#example #definition #program

The **reader monad** encodes read-only access to an environment `e`: `newtype Reader e a = Reader (e -> a)`, i.e. the [[Exponential Object]] $a^e$.
```haskell
instance Monad (Reader e) where
  ma >>= k = Reader (\e -> let a = runReader ma e in runReader (k a) e)
  return a = Reader (\e -> a)
```
Bind threads the same environment into both computations; `return` ignores it. A function `a -> Reader e b` is a curried `(a, e) -> b` — a *delayed* effect, a script interpreted by `runReader`.

> Sources: DaoFP §14.1 ("Environment"), §14.4, Exercise 14.4.1 (`E e a = e -> Maybe a`, a reader/maybe combination); §15.3 (the reader functor is $R_s$ of the currying adjunction), §17.1 (hom functor); CTfS Application 5.3.1.2, Examples 5.3.2.6, 5.3.3.4

- **Who did the experiment?** (CTfS Application 5.3.1.2, Example 5.3.3.4): with $e$ the set of experimenters, a Kleisli arrow $X \to Y^{e}$ is an experiment whose outcome may depend on who performs it (a psychology experiment $X = \text{age} \times \text{income} \to Y = \text{propensity for violence}$). Kleisli composition feeds one experiment's result into the next *with the same experimenter*; ordinary functions embed as "every experimenter gets the same result".
- `Reader e` is the [[Hom Functor]] $\mathcal{C}(e, -)$; its monad structure comes from the fact that $e$ is a comonoid (copy/delete) in a cartesian category — the environment can be duplicated and passed to both parts of a bind.
- Combined with `Maybe` it gives `E e a = e -> Maybe a` ([[DaoFP Chapter 14 Exercises#Exercise 14.4.1|DaoFP Exercise 14.4.1]]); combined with a product it gives the [[State Monad]].

````tabs
tab: Julia
**Docs:** plain Julia — Catlab has no dedicated API for this; related: [Catlab v0.16 docs](https://algebraicjulia.github.io/Catlab.jl/v0.16/) · [GATlab standard library](https://algebraicjulia.github.io/GATlab.jl/stable/stdlib/)
```julia
# Reader: computations are functions of an environment
ret(a) = env -> a
bind(ma, k) = env -> k(ma(env))(env)
ask = env -> env
greet = bind(ask, name -> ret("hello, $name"))
greet("Daniel")                          # "hello, Daniel"
```
tab: Lean
```lean
import Mathlib
#check @ReaderT              -- ReaderT ρ m α := ρ → m α ; Reader ρ = ReaderT ρ Id
#check @ReaderT.bind
#check @MonadReader.read
```
tab: Haskell
```haskell
newtype Reader e a = Reader (e -> a)
runReader :: Reader e a -> e -> a
runReader (Reader h) e = h e
instance Functor (Reader e) where fmap f (Reader h) = Reader (f . h)
instance Applicative (Reader e) where
  pure a = Reader (const a)
  Reader f <*> Reader g = Reader (\e -> f e (g e))
instance Monad (Reader e) where
  ma >>= k = Reader (\e -> runReader (k (runReader ma e)) e)
ask :: Reader e e
ask = Reader id
```
````
