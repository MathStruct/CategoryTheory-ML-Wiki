#definition #example #theorem #proof

A **monoid** $(M, \ast, e)$ consists of a [[Set]] $M$, a function $\ast : M \times M \to M$ (**multiplication**), and an element $e \in M$ (**unit** / **identity element**) such that, in infix notation,

$$
m \ast e = m, \qquad e \ast m = m, \qquad (m \ast n) \ast p = m \ast (n \ast p)
$$

for all $m, n, p \in M$. It is **commutative** if also $m \ast n = n \ast m$.

> Sources: 7 Sketches Example 2.6, §5.4.2 (monoid objects), Exercise 2.8; Kittenlab Lecture 5, 7; DaoFP §5.3 ("Monoids"), §10.9 ("The category of monoids", "Free monoid"), §14.7 ("Monad as a monoid"), §18.1 (Cayley's theorem, Tannakian reconstruction); CTfS §3.1 (Definition 3.1.1.1, Examples 3.1.1.3–3.1.1.22, §3.1.4 monoid homomorphisms: Examples 3.1.4.2–3.1.4.4, Proposition 3.1.4.5, Application 3.1.4.3, Exercises 3.1.4.6–3.1.4.7), Slogan 4.2.1.2

## Examples

- Strings $A^\ast$ over an alphabet $A$ under concatenation with unit $[\,]$ (Kittenlab): the [[Free Monoid]] on $A$.
- $\mathbb{N}, \mathbb{Z}, \mathbb{Q}, \mathbb{R}, \mathbb{C}$ under $+$ (unit $0$) or $\cdot$ (unit $1$).
- $n \times n$ matrices under matrix multiplication, elementwise product, or elementwise sum.
- Subsets of $A$ under $\cap$ (unit $A$) or $\cup$ (unit $\varnothing$).
- Endomorphisms $\mathrm{Hom}(x, x)$ of any object of a [[Category]] under composition.
- Any commutative monoid $M$ gives a [[Symmetric Monoidal Preorder]] $(\mathrm{Disc}_M, =, e, \ast)$ on the [[Discrete Preorder]] ([[7S Chapter 2 Exercises#Exercise 2.8|7S Exercise 2.8]]).

## Monoids as agents acting (Category Theory for Scientists §3.1)

"A common way to interpret phenomena we see around us is to say that agents are acting on objects": a monoid is a set of actions together with a formula saying how a sequence of actions is itself an action; a [[Group]] additionally lets every action be undone. The actions themselves act on a set of states — a [[Monoid Action]], e.g. a [[Finite State Machine]]. Monoids given by generators and relations (buttons that can be pressed, a keyboard with a backspace key, a 32-character buffer, a clock) are [[Presentation of a Monoid|presented monoids]].

- **Smallest examples** ([[CTfS Chapter 3 Exercises#Exercise 3.1.1.7|CTfS Exercise 3.1.1.7]]): the trivial monoid $\{e\}$ is the smallest (the empty set has no unit); every monoid with $\leq 2$ elements is commutative, and the smallest non-commutative one has 3 elements, e.g. $\{e, a, b\}$ with $xy = y$ for $x, y \in \{a, b\}$. $3 \times 3$ matrices under multiplication are the linear-algebra example of non-commutativity.
- **Homomorphisms** (CTfS §3.1.4): the inclusion $(\mathbb N, 0, +) \to (\mathbb Z, 0, +)$ and $n \mapsto 5n$; the homomorphisms $\mathbb N \to \mathbb Z$ are exactly $n \mapsto mn$, $m \in \mathbb Z$ (not $5n - 1$ or $n^2$); the only homomorphism $\mathbb Z \to \mathbb N$ is $0$ (CTfS Proposition 3.1.4.5); $x \mapsto e^x$ is a homomorphism $(\mathbb R_{\geq 0}, 0, +) \to (\mathbb R_{> 0}, 1, \ast)$, while $(\mathbb R_{\geq 0}, +) \to (\mathbb N, +)$ and $(\mathbb R_{> 0}, \ast) \to (\mathbb R_{\geq 0}, +)$ admit only the trivial one ([[CTfS Chapter 3 Exercises#Exercise 3.1.4.7|CTfS Exercise 3.1.4.7]]). Every pair of monoids has the *trivial* homomorphism $M \to \mathbf 1 \to M'$, since the trivial monoid is both initial and terminal in $\mathbf{Mon}$.
- **Biology** (CTfS Application 3.1.4.3): lists of RNA triplets map homomorphically to lists of nucleotides and, by translation, to polypeptides; but there is no homomorphism from all nucleotide lists to polypeptides — a list of two nucleotides codes for nothing — so one restricts to the submonoid of lists whose length is a multiple of three.

## Monoids are one-object categories (Kittenlab Lecture 5)

**Proposition.** A monoid is precisely the same thing as a [[Category]] with a single object.

*Proof.* If $\mathcal{C}$ has one object $x$ then $\mathrm{Hom}(x,x)$ is a monoid with $\ast = \circ$ and $e = 1_x$. Conversely a monoid $M$ gives a category with one object $x$, $\mathrm{Hom}(x,x) = M$, $\circ = \ast$, $1_x = e$. $\blacksquare$

Preorders and monoids are the two "extremes" of categories: lots of objects and few morphisms, versus one object and many morphisms. A [[Functor]] between monoids-as-categories is a **monoid homomorphism**, a function $F : M \to N$ with $F(a \ast b) = F(a) \ast F(b)$ and $F(e) = e$. A [[Natural Transformation]] between homomorphisms $f, g : G \to H$ of groups is an $h \in H$ with $f(x) = h\, g(x)\, h^{-1}$ — conjugation (Kittenlab Lecture 7).

## The category of monoids (DaoFP §10.9)

$\mathbf{Mon}$ has monoids as objects and homomorphisms as morphisms. Functors: $\mathbf{Mon} \to \mathbf{Cat}$ (view as one-object category), the forgetful $U : \mathbf{Mon} \to \mathbf{Set}$, and the [[Free Monoid]] $F : \mathbf{Set} \to \mathbf{Mon}$, $A \mapsto A^\ast$, with $F \dashv U$ ([[Free-Forgetful Adjunction]]). The [[List Monad]] is the monad $UF$ of this adjunction.

## Monoids in a monoidal category

A monoid is a **monoid object** in $(\mathbf{Set}, \times, 1)$: maps $\mu : M \times M \to M$, $\eta : 1 \to M$ satisfying associativity and unit diagrams. Replacing $(\mathbf{Set}, \times, 1)$ by any [[Monoidal Category]] gives [[Monoid Object|monoid objects]] (7 Sketches §5.4.2, DaoFP §5.3); e.g. a [[Monad]] is a monoid in the category of endofunctors (DaoFP §14.7), and a [[Frobenius Monoid]] is a monoid-comonoid pair. A monoid is also a [[Prop]]-like structure: the [[Free Prop]] on one generator with equations. Cayley's theorem: every monoid embeds in the monoid of endofunctions of its underlying set (DaoFP §18.1, an instance of the [[Yoneda Lemma]]).

````tabs
tab: Julia
**Docs:** [ThCategory (GATlab)](https://algebraicjulia.github.io/GATlab.jl/stable/stdlib/#GATlab.Stdlib.StdTheories.ThCategory) · [Theories & presentations](https://algebraicjulia.github.io/Catlab.jl/v0.16/apis/theories/) — Kittenlab [Lecture 5](https://algebraicjulia.github.io/Kittenlab.jl/lecture5.html), [Lecture 7](https://algebraicjulia.github.io/Kittenlab.jl/lecture7.html)
```julia
# Kittenlab Lecture 5
abstract type Monoid{T} end
# mul(m::Monoid{T}, x::T, y::T)::T
# ident(m::Monoid{T})::T

struct ConcatMonoid{T} <: Monoid{Vector{T}}
  alphabet::Set{T}
end
function mul(m::ConcatMonoid{T}, xs::Vector{T}, ys::Vector{T}) where {T}
  @assert all(x ∈ m.alphabet for x in xs) && all(y ∈ m.alphabet for y in ys)
  [xs; ys]
end
ident(::ConcatMonoid{T}) where {T} = T[]
```
Catlab version (run in a fresh Julia session — Catlab exports its own `compose`, `id`, `FinFunction`, …):
```julia
# Catlab: a monoid is a one-object category; the free monoid on generators a, b
using Catlab
@present M(FreeCategory) begin
  x::Ob
  (a, b)::Hom(x, x)
end
compose(M[:a], M[:b], M[:a])   # the word "aba"; id(M[:x]) is the empty word
```
tab: Lean
```lean
#check Monoid            -- class Monoid (M : Type u) extends Semigroup M, MulOneClass M
#check CommMonoid
#check AddMonoid
example : Monoid (List Char) := inferInstance   -- free monoid: lists under ++
#check MonoidHom           -- M →* N
#check CategoryTheory.MonCat   -- the category of monoids
-- a monoid as a one-object category:
#check CategoryTheory.SingleObj  -- SingleObj M, with `SingleObj.star`
```
tab: Haskell
```haskell
-- Prelude / Data.Monoid
class Semigroup a where (<>) :: a -> a -> a
class Semigroup a => Monoid a where mempty :: a
-- laws: mempty <> x = x = x <> mempty; (x <> y) <> z = x <> (y <> z)

-- the free monoid on a: lists
-- instance Monoid [a] where mempty = []; (<>) = (++)

-- a monoid homomorphism (unenforced): h mempty = mempty, h (x <> y) = h x <> h y
newtype MonHom m n = MonHom (m -> n)
```
````
