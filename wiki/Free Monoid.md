#definition #example #theorem

The **free monoid** on a set $A$ (an *alphabet*) is $A^*$, the set of finite **strings** (lists) over $A$, with concatenation as multiplication and the empty string $[\,]$ as unit. It is the [[Monoid]] "generated freely" by $A$: the [[Free-Forgetful Adjunction|left adjoint]] $F : \mathbf{Set} \to \mathbf{Mon}$ to the forgetful functor $U$, so every function $f : A \to U(m)$ extends uniquely to a homomorphism $A^* \to m$ (`foldMap f`).

> Sources: Kittenlab Lecture 5 (`ConcatMonoid`), 7 ($\eta : 1_{\mathbf{Set}} \Rightarrow UF$); DaoFP §10.9, §12.4 ("Lists as initial algebras"), §15.3 ("Free monoid and the list monad"); 7 Sketches Example 3.13 (the one-generator case $\mathbb{N}$ as a free category), §5.2.4 (free props).

- On one generator: $\{s\}^* \cong (\mathbb{N}, +, 0)$, the [[Free Category]] on a single loop (7 Sketches Example 3.13).
- In Haskell the free monoid on `a` is `[a]`; it is also the [[Initial Algebra]] of the functor $1 + a \times X$ (DaoFP §12.4), and $UF$ is the [[List Monad]] whose unit is the singleton $x \mapsto [x]$ and whose multiplication is `concat` (DaoFP §15.3).
- A free monoid "remembers to do the multiplication later"; `foldMap` is an interpreter, and the same list can be interpreted additively or multiplicatively ([[DaoFP Exercise 10.9.2]]).
- Generalizations: the [[Free Category]] on a graph (typed strings), the [[Free Prop]] on a signature (strings of boxes in series and parallel), and free [[Monoid Object|monoid objects]] in a [[Monoidal Category]].

````tabs
tab: Julia
```julia
# Kittenlab Lecture 5
struct ConcatMonoid{T} <: Monoid{Vector{T}}
  alphabet::Set{T}
end
mul(m::ConcatMonoid{T}, xs::Vector{T}, ys::Vector{T}) where {T} = [xs; ys]
ident(::ConcatMonoid{T}) where {T} = T[]
# unit of the adjunction: η(x) = [x]; naturality: map(f, [x]) == [f(x)]
```
tab: Lean
```lean
#check FreeMonoid            -- FreeMonoid α, with `FreeMonoid.of : α → FreeMonoid α` (the unit)
#check FreeMonoid.lift       -- the universal property
example : FreeMonoid Char ≃* ... := sorry   -- FreeMonoid α is definitionally List α
```
tab: Haskell
```haskell
-- the free monoid on a is [a]; the unit of the adjunction is the singleton
eta :: a -> [a]
eta x = [x]
-- the universal extension of f :: a -> m
extend :: Monoid m => (a -> m) -> [a] -> m
extend f = foldr (\x acc -> f x <> acc) mempty
```
````
