#example #definition #program #theorem

The **writer monad** encodes logging: `newtype Writer w a = Writer (a, w)` where the log type `w` is a [[Monoid]] — needed to append logs and to provide the trivial empty log.
```haskell
instance Monoid w => Monad (Writer w) where
  (Writer (a, w)) >>= k = let Writer (b, w') = k a in Writer (b, mappend w w')
  return a = Writer (a, mempty)
```

> Sources: DaoFP §14.1, §14.4 ("Logging"), §15.3 ("M-sets and the writer monad"); §16 (the dual costate/[[Comonad]]).

**From an adjunction** ([[Monads from Adjunctions]]). An **$M$-set** is a set $S$ with a left action $a : M \times S \to S$ of a monoid $M$, $a_1 = \mathrm{id}$, $a_{m_1} \circ a_{m_2} = a_{m_1 \cdot m_2}$; $M$-sets and equivariant maps ($f \circ a_m = b_m \circ f$) form $\mathbf{MSet}$. The forgetful $U : \mathbf{MSet} \to \mathbf{Set}$ has left adjoint $F S = S \times M$ with free action $\phi_n(x, m) = (x, n \cdot m)$: an equivariant $f : F S \to (R, b)$ is determined by its values on $(x, 1)$, namely $f(x, m) = b_m(u x)$, giving $\mathbf{MSet}(F S, Q) \cong \mathbf{Set}(S, U Q)$. Unit $\eta_S(x) = (x, 1)$ is `return`; counit $\varepsilon_Q(x, m) = a_m x$; and $\mu = U \varepsilon F$ is $((x, m), n) \mapsto (x, n \cdot m)$ — `join (Writer (Writer (x, m), n)) = Writer (x, mappend n m)`.

````tabs
tab: Julia
```julia
# Writer over the String monoid: values paired with logs
ret(a) = (a, "")
bind((a, w), k) = ((b, w′) = k(a); (b, w * w′))
step1(x) = (x + 1, "inc; ")
step2(x) = (2x, "double; ")
bind(bind(ret(3), step1), step2)      # (8, "inc; double; ")
```
tab: Lean
```lean
import Mathlib
-- an M-set is a MulAction; the free M-set on S is S × M with the free (right-regular) action
#check @MulAction
#check @MulAction.toFun
-- Writer as a state-free logging monad:
def Writer (w α : Type) := α × w
def Writer.bind [Monoid w] (m : Writer w α) (k : α → Writer w β) : Writer w β :=
  let (b, w') := k m.1; (b, m.2 * w')
```
tab: Haskell
```haskell
newtype Writer w a = Writer (a, w)
runWriter :: Writer w a -> (a, w)
runWriter (Writer p) = p
instance Functor (Writer w) where fmap f (Writer (a, w)) = Writer (f a, w)
instance Monoid w => Applicative (Writer w) where
  pure a = Writer (a, mempty)
  Writer (f, w) <*> Writer (a, w') = Writer (f a, w <> w')
instance Monoid w => Monad (Writer w) where
  Writer (a, w) >>= k = let Writer (b, w') = k a in Writer (b, w <> w')
tell :: w -> Writer w ()
tell w = Writer ((), w)
```
````
