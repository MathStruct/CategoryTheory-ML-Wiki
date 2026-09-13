#definition #example #program

An **algebra** for an [[Endofunctor]] $F : \mathcal{C} \to \mathcal{C}$ is a pair $(c, \alpha)$ of a **carrier** object $c$ and a **structure map** (evaluator) $\alpha : F c \to c$. Given algebras $(a, \alpha)$, $(b, \beta)$, an **algebra morphism** is an arrow $f : a \to b$ with

$$
f \circ \alpha = \beta \circ F f,
$$

i.e. the square below commutes. Algebras and their morphisms form the category $\mathbf{Alg}(F)$ (composition and identities are algebra morphisms because $F$ preserves composition and identities). Its [[Initial Object]] is the [[Initial Algebra]].

```tikz
\usepackage{tikz-cd}
\begin{document}
\begin{tikzcd}
F a \arrow[r, "F f"] \arrow[d, "\alpha"'] & F b \arrow[d, "\beta"] \\
a \arrow[r, "f"'] & b
\end{tikzcd}
\end{document}
```

> Sources: DaoFP Chapter 12 ("Algebras": §12.1 "Algebras from Endofunctors", §12.2 "Category of Algebras"), Exercises 12.2.1–12.2.2; §14–15 (a [[Monad]] algebra is an [[Eilenberg-Moore Category|Eilenberg–Moore algebra]]); 7 Sketches §5.4 (operad/monoid algebras, [[Operad Algebra]]).

**Idea.** A recursive data structure — an expression tree `data Expr = Val Int | Plus Expr Expr` — separates into two orthogonal concerns: the *machinery of recursion* and the *pluggable components*. The components are a "container with holes", the functor `data ExprF x = ValF Int | PlusF x x`; an algebra is the recipe for the *last step* of evaluation, assuming the subtrees are already evaluated: `eval (ValF n) = n; eval (PlusF m n) = m + n` with carrier `Int`, or `pretty` with carrier `String` and concatenation. Infinitely many evaluators exist "and we shouldn't be judgemental". The structure map is *not* polymorphic: it is a specific function for a specific carrier.

- `show :: Int -> String` is **not** an algebra morphism from `eval` to `pretty`: on a `PlusF 2 3` node, `show (eval ..) = "5"` but `pretty (fmap show ..) = "2 + 3"` ([[DaoFP Chapter 12 Exercises#Exercise 12.2.1|DaoFP Exercise 12.2.1]]). By contrast `log` *is* a morphism from the multiplicative to the additive algebra of `FloatF` ([[DaoFP Chapter 12 Exercises#Exercise 12.2.2|DaoFP Exercise 12.2.2]]).
- Any algebra $(a, \alpha)$ lifts to an algebra $(F a, F \alpha)$ — the self-similarity behind Lambek's lemma.
- Dual: [[Coalgebra of an Endofunctor]]. Monoids, groups, rings are algebras for the appropriate polynomial functors *with equations*; an algebra for a [[Monad]] adds compatibility with $\eta, \mu$.

````tabs
tab: Julia
```julia
# an algebra for ExprF x = ValF Int | PlusF x x, evaluated over an explicit tree
abstract type ExprF{X} end
struct ValF{X} <: ExprF{X}; n::Int; end
struct PlusF{X} <: ExprF{X}; l::X; r::X; end
fmap(f, e::ValF) = ValF{Any}(e.n)
fmap(f, e::PlusF) = PlusF{Any}(f(e.l), f(e.r))
eval_alg(e::ValF) = e.n                         # Algebra ExprF Int
eval_alg(e::PlusF) = e.l + e.r
pretty_alg(e::ValF) = string(e.n)              # Algebra ExprF String
pretty_alg(e::PlusF) = e.l * " + " * e.r
```
tab: Lean
```lean
import Mathlib
open CategoryTheory
#check @CategoryTheory.Endofunctor.Algebra          -- structure: a, str : F.obj a ⟶ a
#check @CategoryTheory.Endofunctor.Algebra.Hom      -- f with str ≫ f = F.map f ≫ str
#check @CategoryTheory.Endofunctor.Algebra.instCategory
```
tab: Haskell
```haskell
data ExprF x = ValF Int | PlusF x x
  deriving Functor
type Algebra f c = f c -> c

eval :: Algebra ExprF Int
eval (ValF n)    = n
eval (PlusF m n) = m + n

pretty :: Algebra ExprF String
pretty (ValF n)    = show n
pretty (PlusF s t) = s ++ " + " ++ t
```
````
