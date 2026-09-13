#definition #example #theorem #proof

For any object $x$ of a [[Category]] $\mathcal{C}$ the **covariant representable functor** on $x$ is

$$
y_{\mathcal{C}}(x) := \mathrm{Hom}_{\mathcal{C}}(x, -) : \mathcal{C} \to \mathbf{Set},
$$

sending $y \mapsto \mathrm{Hom}(x, y)$ and $g : y \to z$ to post-composition $\mathrm{Hom}(x, g) = (g \circ -)$. A functor $F : \mathcal{C} \to \mathbf{Set}$ is **representable** if $F \cong \mathrm{Hom}(x, -)$ for some $x$, its **representing object** (or **representative**); dually for presheaves, $F \cong \mathrm{Hom}(-, x)$. "We say '$x$ is a representative for $F$' meaning we have picked a specific isomorphism."

> Sources: Kittenlab Lecture 8 ("Representable Functors", "Representatives of functors"), 10 ("Representables revisited"), 11, 12; DaoFP §8.4, §9.8 ("Representable Functors", "The guessing game", "Representable functors in programming"), Exercises 9.8.1–9.8.5; 7 Sketches Exercise 1.66 (preorder case: [[Upper Set|$\uparrow p$]]).

## Examples of representables

- $\mathrm{Hom}_{\mathsf{Gr}}(V, -)$ is the one-vertex graph, $\mathrm{Hom}_{\mathsf{Gr}}(E, -)$ the one-edge graph (Kittenlab Lecture 8); in $\mathsf{Petri}$, $\mathrm{Hom}(S, -)$ is a single species and $\mathrm{Hom}(I, -)$ one species, one transition, one arc (Lecture 10).
- $\mathrm{Hom}_{\mathbf{Set}}(\underline{2}, -) : X \mapsto X^2$ (pairs); $\mathrm{Hom}(P_n, -)$ on graphs gives the length-$n$ paths.
- For a [[Free Category|path category]] of a DAG, $y(a)(b)$ is the set of paths $a \to b$, computable by dynamic programming (Lecture 10); for the path graph, $y(i)(j) = 1$ if $i \leq j$ else $\varnothing$, so $y(i) \cong y(j)$ iff $i = j$.

## Functors represented by an object

- $U : \mathbf{Vect} \to \mathbf{Set}$ is represented by $\mathbb{R}$: $\mathrm{Hom}(\mathbb{R}, V) \cong V$ via $f \mapsto f(1)$; the naturality "toblerone" is $(g \circ f)(1) = g(f(1))$ (Lecture 10). The constant singleton functor on $\mathbf{Vect}$ is represented by $\mathbb{R}^0$.
- $\mathrm{Hom}(X, -) \times \mathrm{Hom}(Y, -)$ is represented by the [[Coproduct]] $X + Y$; in general a representing object of $\mathrm{Hom}(x,-) \times \mathrm{Hom}(y,-)$ *is* the coproduct (Lecture 8). Representing $\{f : B \to X \mid f \circ p = f \circ q\}$ gives the [[Coequalizer]] (Lecture 11); representing $\mathrm{Hom}_{\mathcal{C}^{\mathsf{D}}}(F, \Delta(-))$ gives the [[Colimit]] (Lecture 9). Dually [[Limit|limits]], [[Product|products]] ([[DaoFP Chapter 9 Exercises#Exercise 9.8.1|DaoFP Exercise 9.8.1]]).
- The singleton functor $c \mapsto \{c\}$ is representable iff $\mathcal{C}$ has an [[Initial Object]] ([[DaoFP Chapter 9 Exercises#Exercise 9.8.2|DaoFP Exercise 9.8.2]]); the constant $1$ functor is represented by the initial object, "the logarithm of 1" ([[DaoFP Chapter 9 Exercises#Exercise 9.8.4|DaoFP Exercise 9.8.4]]).
- Non-example: on $(\mathbb{Q}_{\geq 0}, \leq)$, $F(x) = [2 \leq x^2]$ has no representative since $\sqrt 2 \notin \mathbb{Q}$; on $\mathbb{R}_{\geq 0}$ it does (Lecture 10). Lists are not representable ("no logarithm of a sum"), but a list functor is a sum of representables ([[DaoFP Chapter 9 Exercises#Exercise 9.8.5|DaoFP Exercise 9.8.5]]); infinite streams are represented by $\mathbb{N}$.

## Properties

**Uniqueness.** Representing objects are unique up to isomorphism: $\mathrm{Hom}(c, -) \cong \mathrm{Hom}(c', -)$ implies $c \cong c'$ — a corollary of the [[Yoneda Lemma]] (Kittenlab Lecture 12). "This gives us a very powerful tool for constructing objects in a category: look for representatives of functors into $\mathbf{Set}$, and if they exist, they must be unique" — the basis of Kittenlab's treatment of universal properties ("composing objects" by first giving a *specification* $\mathcal{C} \to \mathbf{Set}$, Lecture 11). DaoFP: "the representing object $a$ is like a logarithm of the functor" — $x \times x \cong x^2$ is represented by $2 = 1 + 1$, and $\mathcal{C}(1, x^a) \cong \mathcal{C}(a, x)$ in a closed category. Representables are "dense" among presheaves: every presheaf is a [[Colimit]] of representables (DaoFP §9.8, §17).

**The guessing game** (DaoFP §9.8): one theorist hides an object; the other probes it with objects $a$ and receives the sets $\mathcal{C}(a, x)$ and, for arrows, functions between them. The answers define a presheaf whose representing object is the secret — unless the opponent invents a *non-representable* "fantastic beast", "often as interesting as the real ones". Kittenlab's version: identifying a person at a party by whom they talked to.

**In programming** (DaoFP): `class Representable f where type Key f; tabulate :: (Key f -> x) -> f x; index :: f x -> (Key f -> x)` — `tabulate` turns a function into a lookup table (memoization), `index` looks up. `Stream` is representable by `Nat`, `Pair x x` by `Bool` ([[DaoFP Chapter 9 Exercises#Exercise 9.8.3|DaoFP Exercise 9.8.3]]).

````tabs
tab: Julia
```julia
# Kittenlab Lecture 8: Hom(X,-) × Hom(Y,-) ≅ Hom(X+Y,-) in FinSet
struct Left{T}; val::T end
struct Right{T}; val::T end
disjoint_union(X::AbstractSet{S}, Y::AbstractSet{T}) where {S,T} =
  Set{Union{Left{S}, Right{T}}}([Left.(collect(X))..., Right.(collect(Y))...])

copair(f::FinFunction{X,Z}, g::FinFunction{Y,Z}) where {X,Y,Z} =            # Hom(X,-)×Hom(Y,-) → Hom(X+Y,-)
  FinFunction{Union{Left{X},Right{Y}},Z}(disjoint_union(f.dom, g.dom), g.codom,
    Dict(vcat([Left(x) => f(x) for x in f.dom], [Right(y) => g(y) for y in g.dom])))
unpack(xs, ys, h) = (FinFunction(xs, h.codom, Dict(x => h(Left(x)) for x in xs)),      # inverse
                     FinFunction(ys, h.codom, Dict(y => h(Right(y)) for y in ys)))

# Catlab: representable C-sets and the Yoneda bijection
using Catlab
yV = representable(Graph, :V)         # one vertex
yE = representable(Graph, :E)         # one edge
G = cycle_graph(Graph, 4)
length(homomorphisms(yE, G)) == ne(G) # true
```
tab: Lean
```lean
#check CategoryTheory.Functor.Representable     -- F : Cᵒᵖ ⥤ Type v is representable by some X
#check CategoryTheory.Functor.Corepresentable   -- F : C ⥤ Type v ≅ coyoneda.obj (op X)
#check CategoryTheory.Functor.reprX             -- the representing object
#check CategoryTheory.coyoneda                  -- x ↦ Hom(x, -)
```
tab: Haskell
```haskell
{-# LANGUAGE TypeFamilies #-}
-- DaoFP §9.8
class Representable f where
  type Key f
  tabulate :: (Key f -> x) -> f x
  index    :: f x -> (Key f -> x)

data Pair x = Pair x x
instance Representable Pair where
  type Key Pair = Bool
  tabulate g = Pair (g True) (g False)
  index (Pair a b) = \k -> if k then a else b

data Stream a = Stm a (Stream a)
data Nat = Z | S Nat
instance Representable Stream where
  type Key Stream = Nat
  tabulate g = tab Z where tab n = Stm (g n) (tab (S n))
  index stm = \n -> ind n stm
    where ind Z (Stm a _) = a
          ind (S k) (Stm _ as) = ind k as
```
````
