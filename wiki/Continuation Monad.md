#example #definition #program #theorem

The **continuation monad** `newtype Cont r a = Cont ((a -> r) -> r)` packages a value as "something that takes a handler for the value" ([[Continuation]]):
```haskell
instance Monad (Cont r) where
  ma >>= fk = Cont (\k -> runCont ma (\a -> runCont (fk a) k))
  return a  = Cont (\k -> k a)
```
Bind requires "backward thinking" (inversion of control, "don't call us, we'll call you"): to produce a `Cont r b` we need a function of `k :: b -> r`; run `ma` with the continuation that, given `a`, runs `fk a` with `k`. Fortunately it is implemented once; [[Do Notation]] hides the rest.

> Sources: DaoFP §14.1 ("Continuation"), §14.4, §14.6 ("Continuation Passing Style"), §15.3 ("The continuation monad"); §17 (the [[Yoneda Lemma]] behind continuations).

**From an adjunction** ([[Monads from Adjunctions]]): $L_Z : \mathbf{Set}^{\mathrm{op}} \to \mathbf{Set}$, $X \mapsto \mathbf{Set}(X, Z)$ and $R_Z : \mathbf{Set} \to \mathbf{Set}^{\mathrm{op}}$, $X \mapsto \mathbf{Set}(X, Z)$ are adjoint (contravariant functors handled by choosing $\mathbf{Set}^{\mathrm{op}}$ as one endpoint), and $R_Z L_Z X = Z^{Z^X}$ is `(x -> r) -> r` — covariant because $X$ sits in a doubly negative position. In a CCC this is the endofunctor $K_r a = r^{r^a}$.

See [[Continuation Passing Style]] for the CPS transformation turning recursion into tail recursion, and [[Defunctionalization]] for replacing the resulting closures by data.

````tabs
tab: Julia
```julia
# continuations as functions of a handler
ret(a) = k -> k(a)
bind(ma, fk) = k -> ma(a -> fk(a)(k))
runCont(m, k) = m(k)
add1 = x -> ret(x + 1)
runCont(bind(ret(41), add1), identity)      # 42
```
tab: Lean
```lean
import Mathlib
-- Cont r α := (α → r) → r ; it is a monad (Lean core has no Cont, so define it)
def Cont (r α : Type) := (α → r) → r
def Cont.pure (a : α) : Cont r α := fun k => k a
def Cont.bind (m : Cont r α) (f : α → Cont r β) : Cont r β := fun k => m (fun a => f a k)
```
tab: Haskell
```haskell
newtype Cont r a = Cont ((a -> r) -> r)
runCont :: Cont r a -> (a -> r) -> r
runCont (Cont f) k = f k
instance Functor (Cont r) where fmap f c = Cont (\k -> runCont c (k . f))
instance Applicative (Cont r) where
  pure a = Cont (\k -> k a)
  cf <*> ca = Cont (\k -> runCont cf (\f -> runCont ca (k . f)))
instance Monad (Cont r) where
  ma >>= fk = Cont (\k -> runCont ma (\a -> runCont (fk a) k))
```
````
