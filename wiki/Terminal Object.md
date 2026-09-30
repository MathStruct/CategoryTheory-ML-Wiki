#definition #theorem #proof #example

An object $Z$ of a [[Category]] $\mathcal{C}$ is **terminal** if for every object $C$ there exists a *unique* morphism $! : C \to Z$. Since this holds for all objects, terminal objects have a **universal property**. DaoFP writes the terminal object $1$ and its arrows $!_a$; in Haskell it is `()` (Unit); in logic it is truth $\top$ ("true no matter what your assumptions are").

> Sources: 7 Sketches Definition 3.79, Example 3.80, Proposition 3.84, Remark 3.85, Examples 3.93, 3.96, Exercises 3.81–3.83; DaoFP §1.2 ("Yin and Yang"), §1.3, Exercises 3.1.3–3.1.4, §9.5; Kittenlab Lecture 9 (dual: initial objects), 12 ($A \cong \mathrm{Hom}(1, A)$); CTfS Exercise 2.5.3.5, Definition 4.5.3.2, Examples 4.5.3.3–4.5.3.13, Example 5.1.2.1

## Examples

- In $\mathbf{Set}$ any singleton $\{\bullet\}$: the unique function sends everything to $\bullet$ (Example 3.80). Elements of $A$ are arrows $1 \to A$ ([[Global Element]]): "the terminal object behaves like an indivisible point; we can use it to probe other objects" (DaoFP).
- In a [[Preorder]], a terminal object is a **top element** $z$ with $c \leq z$ for all $c$ ([[7S Chapter 3 Exercises#Exercise 3.81|7S Exercise 3.81]]) — the empty [[Meet]].
- In [[Category of Categories|$\mathbf{Cat}$]], the one-object category $\underline{\mathbf{1}}$ ([[7S Chapter 3 Exercises#Exercise 3.82|7S Exercise 3.82]]); in $\mathbf{Grph}$, the graph with one vertex and one loop; in [[Category of Preorders|$\mathbf{Preord}$]] the one-point preorder.
- More examples (CTfS §4.5.3): the trivial monoid in $\mathbf{Mon}$ and the trivial group in $\mathbf{Grp}$ (also initial); $X$ itself in the power set $\mathcal P(X)$; $\mathsf{TRUE}$ in the preorder of propositions; $0$ in $(\mathbb N, \mid)$ (everything divides $0$); every object of an indiscrete category. The free monoid on $\{a, b\}$ as a one-object category has neither initial nor terminal object ([[CTfS Chapter 4 Exercises#Exercise 4.5.3.12|CTfS Exercise 4.5.3.12]]). $\mathcal{C}$ has a terminal object iff the functor $\mathcal{C} \to \mathbf 1$ has a right adjoint (CTfS Example 5.1.2.1).
- Not every category has one: the discrete two-object category ([[7S Chapter 3 Exercises#Exercise 3.83|7S Exercise 3.83]]).
- In a cocomplete locally small category, a *weakly terminal set* (a family $t_i$ such that every $c$ has some arrow to some $t_i$) yields a terminal object as a colimit — the key to the [[Adjoint Functor Theorem]] (DaoFP §9.5).

## Uniqueness (Proposition 3.84)

*All terminal objects are isomorphic.* Let $Z, Z'$ be terminal; there are unique $a : Z \to Z'$, $b : Z' \to Z$. Then $a \mathbin{;} b : Z \to Z$ must equal the unique map $\mathrm{id}_Z$; similarly $b \mathbin{;} a = \mathrm{id}_{Z'}$. $\blacksquare$ Moreover the isomorphism is *unique* ("unique up to unique isomorphism", Remark 3.85; [[DaoFP Chapter 3 Exercises#Exercise 3.1.3|DaoFP Exercise 3.1.3]], [[DaoFP Chapter 3 Exercises#Exercise 3.1.4|DaoFP Exercise 3.1.4]]), which is why we say "*the* terminal object", "*the* product", "*the* limit" — "to a category theorist, this is very nearly the same as saying all terminal objects are equal".

## Role

A terminal object is the [[Limit]] of the empty [[Diagram]] (Example 3.93; the tuple formula of [[Finite Limits in Set]] gives $\{()\}$). Every [[Limit]] is a terminal object in a [[Cone|category of cones]] — "they're all just terminal objects in different categories". Dual: [[Initial Object]]. Terminal objects are the unit of [[Product|products]] and, in a [[Cartesian Category]], the monoidal unit; any arrow *from* $1$ is [[Monomorphism|mono]] and any arrow *to* $1$ is [[Epimorphism|epi]] (DaoFP Exercises 2.4.1, 2.5.1).

````tabs
tab: Julia
**Docs:** [FinSets](https://algebraicjulia.github.io/Catlab.jl/v0.16/apis/categorical_algebra/#Catlab.CategoricalAlgebra.FinSets) · [Limits & colimits](https://algebraicjulia.github.io/Catlab.jl/v0.16/apis/categorical_algebra/#Catlab.CategoricalAlgebra.Limits) · [Graphs](https://algebraicjulia.github.io/Catlab.jl/v0.16/apis/graphs/) — Kittenlab [Lecture 9](https://algebraicjulia.github.io/Kittenlab.jl/lecture9.html)
```julia
using Catlab
T = terminal(FinSet{Int}); ob(T)     # FinSet(1)
X = FinSet(4)
delete(T, X)                         # the unique function X → 1 (a ConstantFunction)
# in Graph: the terminal graph has one vertex and one loop
terminal(Graph) |> apex
```
tab: Lean
```lean
#check CategoryTheory.Limits.IsTerminal
#check CategoryTheory.Limits.HasTerminal
#check CategoryTheory.Limits.terminal.from        -- the unique morphism X ⟶ ⊤_ C
#check CategoryTheory.Limits.IsTerminal.uniqueUpToIso
example : CategoryTheory.Limits.IsTerminal (PUnit : Type) := CategoryTheory.Limits.Types.isTerminalPunit
```
tab: Haskell
```haskell
-- DaoFP §1.2: the terminal type () and the unique arrow to it
unit :: a -> ()
unit _ = ()
-- elements of a are arrows () -> a
```
````
