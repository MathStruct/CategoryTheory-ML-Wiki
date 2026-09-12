#program #example #annotation

**Defunctionalization** replaces higher-order functions by first-order data plus an `apply` function. DaoFP explains it as an instance of the [[Adjoint Functor Theorem]]: the function type $b^a$ is the right adjoint to $(- \times a)$, $\mathcal{C}(e \times a, b) \cong \mathcal{C}(e, b^a)$, and its counit is `apply :: (a -> b, a) -> b`. An object of the [[Comma Category]] $(- \times a) \downarrow b$ is a pair (environment $e$, function $e \times a \to b$) — a *closure*; a morphism $h : e \to e'$ with $f' \circ (h \times a) = f$ "reduces the environment" by projecting out irrelevant variables. Freyd's theorem builds $b^a$ as the colimit over this comma category — "a giant coproduct of all environments modulo identifications". For a specific program we only need a small **solution set**: the finitely many functions actually passed around, with their environments.

> Source: DaoFP §10.8 ("Defunctionalization"), §14.6 ("Continuation Passing Style", "Defunctionalization").

**Worked example.** Summing a list in [[Continuation|continuation-passing style]]:

```haskell
sumK :: [Int] -> (Int -> r) -> r
sumK [] k = k 0
sumK (i : is) k = sumK is (\s -> k (i + s))
sumList as = sumK as (\i -> i)
```
Name the two lambdas with explicit environments: `more (i, k) s = k (i + s)` with environment `(Int, Int -> r)`, and `done i = i` with environment `()`. The solution set is these two environments; their coproduct is the data type
```haskell
data Kont = Done | More Int Kont
```
— recursively encoding the `Int -> Int` part as `Kont`, which is a list of `Int` in disguise (the runtime stack). The counit becomes
```haskell
apply :: (Kont, Int) -> Int
apply (Done, i) = i
apply (More i k, s) = apply (k, i + s)

sumK'' :: [Int] -> Kont -> Int
sumK'' [] k = apply (k, 0)
sumK'' (i : is) k = sumK'' is (More i k)
```
with no higher-order functions or lambdas at all. Arguments that are data can be serialized and sent over the wire; the receiver needs only `apply`. The [[Yoneda Lemma]] underlies CPS itself: `a ≅ forall x. (a -> x) -> x`.

````tabs
tab: Julia
```julia
# the same defunctionalization in Julia
abstract type Kont end
struct Done <: Kont end
struct More <: Kont; i::Int; k::Kont end
apply(::Done, s) = s
apply(k::More, s) = apply(k.k, k.i + s)
sumK(xs, k) = isempty(xs) ? apply(k, 0) : sumK(xs[2:end], More(xs[1], k))
sumK([1, 2, 3, 4], Done())     # 10
```
tab: Haskell
```haskell
data Kont = Done | More Int Kont
apply :: (Kont, Int) -> Int
apply (Done, i) = i
apply (More i k, s) = apply (k, i + s)
sumK'' :: [Int] -> Kont -> Int
sumK'' [] k = apply (k, 0)
sumK'' (i : is) k = sumK'' is (More i k)
sumList'' :: [Int] -> Int
sumList'' is = sumK'' is Done
```
````
