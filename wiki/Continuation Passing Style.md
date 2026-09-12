#example #program #annotation

**CPS** (continuation passing style) rewrites a function so that instead of returning a result it passes the result to a [[Continuation]] `k`. Every function call then happens in *tail position*, so the compiler can turn recursion into a loop and no runtime stack is consumed — the reason imperative languages prefer loops (which need mutation) while pure languages must use recursion, relying on tail-call optimisation.

> Sources: DaoFP §14.6 ("Continuation Passing Style": "Tail recursion and CPS", "Using named functions", "Defunctionalization").

**Tail recursion.** `sum1 (i : is) = i + sum1 is` is not tail recursive (it adds after the call); `sum2 = go 0 where go n (i : is) = go (n + i) is` is, and is guaranteed to become a loop.

**Tree traversal in CPS.** For `data Tree = Leaf String | Node Tree String Tree`, the non-tail-recursive `show (Node l s r) = show l ++ s ++ show r` becomes, in the [[Continuation Monad]] with [[Do Notation]],
```haskell
showk :: Tree -> Cont r String
showk (Leaf s) = return s
showk (Node lft s rgt) = do { ls <- showk lft; rs <- showk rgt; return (ls ++ s ++ rs) }
show t = runCont (showk t) id
```
Desugared, `showk (Node lft s rgt) k = showk lft (\ls -> showk rgt (\rs -> k (ls ++ s ++ rs)))`: every recursive call is the last call and returns a generic `r` on which nothing further can be done.

**Named functions.** The lambdas are closures; replacing them by named functions means passing the captured environment explicitly: `showk (Node lft s rgt) k = showk lft (next (s, rgt, k))`, `next (s, rgt, k) ls = showk rgt (conc (ls, s, k))`, `conc (ls, s, k) rs = k (ls ++ s ++ rs)`, `done s = s`.

**[[Defunctionalization]].** If higher-order functions are unavailable (e.g. distributed systems), sum all the environments into a data type `data Kont = Done | Next String Tree Kont | Conc String String Kont` — a list, i.e. an explicit runtime *stack* — and approximate the counit of the [[Adjoint Functor Theorem]] by `apply :: (Kont, String) -> String`; then `showk :: Tree -> Kont -> String` uses no function arguments at all.

````tabs
tab: Julia
```julia
# tree show in CPS, then defunctionalized with an explicit stack
abstract type Tree end
struct Leaf <: Tree; s::String; end
struct Node <: Tree; l::Tree; s::String; r::Tree; end
showk(t::Leaf, k) = k(t.s)
showk(t::Node, k) = showk(t.l, ls -> showk(t.r, rs -> k(ls * t.s * rs)))
t = Node(Leaf("a"), "b", Node(Leaf("c"), "d", Leaf("e")))
showk(t, identity)                          # "abcde"
# defunctionalized: Kont = Done | Next(s, rgt, k) | Conc(ls, s, k)
abstract type Kont end
struct Done <: Kont end
struct Next <: Kont; s::String; rgt::Tree; k::Kont; end
struct Conc <: Kont; ls::String; s::String; k::Kont; end
apply(::Done, s) = s
apply(n::Next, ls) = showd(n.rgt, Conc(ls, n.s, n.k))
apply(c::Conc, rs) = apply(c.k, c.ls * c.s * rs)
showd(t::Leaf, k::Kont) = apply(k, t.s)
showd(t::Node, k::Kont) = showd(t.l, Next(t.s, t.r, k))
showd(t, Done())                            # "abcde"
```
tab: Haskell
```haskell
data Tree = Leaf String | Node Tree String Tree

showk :: Tree -> (String -> r) -> r
showk (Leaf s) k = k s
showk (Node lft s rgt) k = showk lft (\ls -> showk rgt (\rs -> k (ls ++ s ++ rs)))

data Kont = Done | Next String Tree Kont | Conc String String Kont
apply :: (Kont, String) -> String
apply (Done, s) = s
apply (Next s rgt k, ls) = showd rgt (Conc ls s k)
apply (Conc ls s k, rs) = apply (k, ls ++ s ++ rs)
showd :: Tree -> Kont -> String
showd (Leaf s) k = apply (k, s)
showd (Node lft s rgt) k = showd lft (Next s rgt k)
showTree :: Tree -> String
showTree t = showd t Done
```
````
