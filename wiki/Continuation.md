#definition #example #annotation

A **continuation** is "the rest of the computation": a handler `k :: a -> r` waiting for a value of type `a`. By the [[Yoneda Lemma]] a value $x : a$ can be replaced by the function `\k -> k x :: (a -> r) -> r` that feeds $x$ to any handler — calling the handler is treated as a side effect, giving the [[Continuation Monad]] `Cont r a = (a -> r) -> r`. Programs written so that every function receives its continuation as an argument are in [[Continuation Passing Style]].

> Sources: DaoFP §14.1 ("Continuation"), §14.4, §14.6 ("Continuation Passing Style", "Tail recursion and CPS", "Using named functions", "Defunctionalization"), §15.3; §10.4 (adjoint functor theorem, [[Defunctionalization]]).

- In a CCC the continuation endofunctor is $K_r a = r^{r^a}$: covariant, $a$ appears in a doubly contravariant position.
- Continuations model concurrent tasks and callbacks in imperative languages; compilers use the CPS transformation; and CPS turns any recursion into *tail* recursion, since the continuation "encapsulates the rest of the computation, so it's always the last call in a function".
- With [[Free Monad|free monads]], the continuation parameter `k` of a command functor (`Push Int k`, `Top (Int -> k)`) is what makes the free monad a tree of commands.

````tabs
tab: Julia
```julia
# a value as a continuation-taker (Yoneda): x ↦ (k -> k(x))
cps(x) = k -> k(x)
cps(42)(string)                    # "42"
```
tab: Haskell
```haskell
toCPS :: a -> ((a -> r) -> r)
toCPS x = \k -> k x
fromCPS :: ((a -> a) -> a) -> a        -- run with the identity continuation
fromCPS f = f id
```
````
