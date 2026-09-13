#definition #example #program

The **list type** $L_a$ (list of $a$) is defined by two introduction rules

$$
\mathsf{Nil} : 1 \to L_a, \qquad \mathsf{Cons} : a \times L_a \to L_a,
$$

("a list is either empty or a thing followed by a list of things") and the elimination rule: given $\mathit{init} : 1 \to c$ and $\mathit{step} : a \times c \to c$ there is a unique $h : L_a \to c$ with $h \circ \mathsf{Nil} = \mathit{init}$ and $h \circ \mathsf{Cons} = \mathit{step} \circ (\mathrm{id}_a \times h)$. Such an $h$ is a **fold** (list *catamorphism*); in Haskell `foldr step init`, with `[a]`, `[]` and `(:)` as built-in syntax.

```tikz
\usepackage{tikz-cd}
\begin{document}
\begin{tikzcd}
1 \arrow[r, "\mathsf{Nil}"] \arrow[dr, "\mathit{init}"'] & L_a \arrow[d, "h"] & a \times L_a \arrow[l, "\mathsf{Cons}"'] \arrow[d, "\mathrm{id}_a \times h"] \\
 & c & a \times c \arrow[l, "\mathit{step}"]
\end{tikzcd}
\end{document}
```

> Sources: DaoFP §7.2 ("Lists", "Elimination Rule"), §7.3 ("Functoriality": `map`, `badMap`), §12.4 ("Lists as initial algebras"), §15.3 ("Free monoid and the list monad"), Exercises 7.2.1–7.2.3; Kittenlab Lecture 5 (`ConcatMonoid`); 7 Sketches §5.2.4 (free monoid).

- **Functoriality** (§7.3): for $f : a \to b$, `map f` is the fold with $\mathit{init} = \mathsf{Nil}_b$ and $\mathit{step} = \mathsf{Cons}_b \circ (f \times \mathrm{id})$. The alternative $\mathit{step} = \mathsf{snd}$ (`badMap`, which drops all elements) type-checks but fails the functor law `map id = id` — see [[Functor]].
- $L_a$ is the [[Free Monoid]] on $a$ and the [[Initial Algebra]] of $F(x) = 1 + a \times x$; `foldr` is the catamorphism. $L_1 \cong N$ ([[Natural Numbers Object]], [[DaoFP Chapter 7 Exercises#Exercise 7.2.1|DaoFP Exercise 7.2.1]]).
- Not every mapping out of a list is a fold, but every *Haskell* function `[a] -> c` written by pattern matching on `[]` and `(:)` is ([[DaoFP Chapter 7 Exercises#Exercise 7.2.2|DaoFP Exercise 7.2.2]], [[DaoFP Chapter 7 Exercises#Exercise 7.2.3|DaoFP Exercise 7.2.3]]).
- `sum = foldr plus Z` on lists of naturals; the [[List Monad]] uses concatenation as `join`.

````tabs
tab: Julia
```julia
# foldr as the list recursor
recList(init, step) = as -> isempty(as) ? init : step(as[1], recList(init, step)(as[2:end]))
recList(0, +)([1, 2, 3])                      # 6
mapList(f) = recList(Any[], (a, bs) -> [f(a); bs])
mapList(x -> x^2)([1, 2, 3])                  # [1, 4, 9]
third(as) = length(as) >= 3 ? Some(as[3]) : nothing
```
tab: Lean
```lean
import Mathlib
#check @List.rec                -- the eliminator
#check @List.foldr              -- (α → β → β) → β → List α → β
#check @List.map
#check @List.map_id             -- the functor law badMap violates
#check @List.get?               -- l.get? 2 : Option α, the "third element" of Exercise 7.2.3
```
tab: Haskell
```haskell
data List a where
  Nil  :: List a
  Cons :: (a, List a) -> List a

recList :: c -> ((a, c) -> c) -> (List a -> c)       -- the list recursor
recList init step = \as -> case as of
  Nil          -> init
  Cons (a, as) -> step (a, recList init step as)

foldr' :: (a -> c -> c) -> c -> [a] -> c              -- built-in lists
foldr' step init = \as -> case as of
  []     -> init
  a : as -> step a (foldr' step init as)

mapList :: (a -> b) -> List a -> List b
mapList f = recList Nil (\(a, bs) -> Cons (f a, bs))
```
````
