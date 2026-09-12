#definition #example #program

A **coalgebra** for an [[Endofunctor]] $F$ is a pair $(a, \alpha)$ with structure map $\alpha : a \to F a$ — an algebra in the [[Opposite Category]]. A **coalgebra morphism** $(a, \alpha) \to (b, \beta)$ is $f : a \to b$ with $\beta \circ f = F f \circ \alpha$. Coalgebras form a category whose [[Terminal Object]] is the [[Terminal Coalgebra]].

```tikz
\usepackage{tikz-cd}
\begin{document}
\begin{tikzcd}
a \arrow[r, "f"] \arrow[d, "\alpha"'] & b \arrow[d, "\beta"] \\
F a \arrow[r, "F f"'] & F b
\end{tikzcd}
\end{document}
```

> Sources: DaoFP Chapter 13 ("Coalgebras": "Coalgebras are just algebras in the opposite category. End of chapter!"; §13.1 "Coalgebras from Endofunctors", §13.2 "Category of Coalgebras"), Exercises 13.2.1–13.2.3; §16 ([[Comonad]] coalgebras).

**Idea.** Where algebras *fold* (chop) recursive structures, coalgebras *unfold* (grow) them via anamorphisms: the carrier is the type of a **seed**, and $\alpha$ produces a functorful of new seeds. Neither direction creates information: a sum forgets the list; a seed must already contain everything that ends up in the tree — it is merely re-stored in a form convenient for processing.

```haskell
data TreeF x = LeafF | NodeF Int x x deriving (Show, Functor)
split :: Coalgebra TreeF [Int]              -- seed: a list; grows a binary search tree
split [] = LeafF
split (n : ns) = NodeF n left right where (left, right) = partition (<= n) ns
```

- Duality is not perfect in $\mathbf{Set}$: $0$ has no incoming arrows but $1$ has many outgoing ones, so [[Terminal Coalgebra|terminal coalgebras]] "add their own interesting twists" (infinite data, laziness).
- For the identity functor every set is a fixed point; $\varnothing$ is the least (initial algebra) and $1$ the greatest (terminal coalgebra) ([[DaoFP Exercise 13.2.2]], [[DaoFP Exercise 13.2.3]]).
- Coalgebras model state machines/dynamical systems: $\alpha : S \to O \times S^I$; a [[Discrete Dynamical System]] is a coalgebra for the identity functor.

````tabs
tab: Julia
```julia
# a coalgebra for TreeF x = LeafF | NodeF Int x x, with a list as seed
abstract type TreeF{X} end
struct LeafF{X} <: TreeF{X} end
struct NodeF{X} <: TreeF{X}; n::Int; l::X; r::X; end
fmapT(f, ::LeafF) = LeafF{Any}()
fmapT(f, t::NodeF) = NodeF{Any}(t.n, f(t.l), f(t.r))
split_coalg(ns::Vector{Int}) = isempty(ns) ? LeafF{Any}() :
  NodeF{Any}(ns[1], filter(x -> x <= ns[1], ns[2:end]), filter(x -> x > ns[1], ns[2:end]))
```
tab: Lean
```lean
import Mathlib
open CategoryTheory
#check @CategoryTheory.Endofunctor.Coalgebra          -- V, str : V ⟶ F.obj V
#check @CategoryTheory.Endofunctor.Coalgebra.Hom
#check @CategoryTheory.Endofunctor.Coalgebra.instCategory
```
tab: Haskell
```haskell
import Data.List (partition)
type Coalgebra f a = a -> f a

data TreeF x = LeafF | NodeF Int x x
  deriving (Show, Functor)

split :: Coalgebra TreeF [Int]
split [] = LeafF
split (n : ns) = NodeF n left right
  where (left, right) = partition (<= n) ns
```
````
