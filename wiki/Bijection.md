#definition #theorem #proof

A [[Function]] $f : A \to B$ is **bijective** (a **bijection**, drawn $A \xrightarrow{\sim} B$) if it is both [[Surjection|surjective]] and [[Injection|injective]].

> Sources: 7 Sketches Definition 1.22; Kittenlab Lecture 2 (theorem and proof); DaoFP §3.1.

**Theorem (Kittenlab).** A function $f : A \to B$ has an inverse — i.e. is an [[Isomorphism]] in $\mathbf{Set}$ — if and only if it is surjective and injective.

*Proof.* If $f$ is surjective and injective then for each $b \in B$ there is exactly one $a$ with $f(a) = b$ (at least one by surjectivity, at most one by injectivity); define $g(b)$ to be it. Then $g \circ f = 1_A$ and $f \circ g = 1_B$. Conversely, if $g$ is an inverse then $f$ is surjective because $f(g(b)) = b$, and injective because $a \neq a'$ implies $g(f(a)) \neq g(f(a'))$, hence $f(a) \neq f(a')$. $\blacksquare$

DaoFP stresses that in a general category an arrow that is both [[Monomorphism|mono]] and [[Epimorphism|epi]] need *not* be an isomorphism (e.g. $\mathbb{Z} \hookrightarrow \mathbb{Q}$ in monoids/rings, or dense inclusions in $\mathbf{Top}$); $\mathbf{Set}$ is special.

Two [[Finite Set|finite sets]] are isomorphic iff they have the same [[Cardinality]].

````tabs
tab: Julia
```julia
using Catlab
f = FinFunction([2, 3, 1], 3)
is_iso(f)               # true
inv = FinFunction([3, 1, 2], 3)
compose(f, inv) == id(FinSet(3))   # true
```
tab: Lean
```lean
#check @Function.Bijective            -- Injective ∧ Surjective
#check @Function.bijective_iff_has_inverse
#check (Equiv : Type u → Type v → Type _)   -- bundled bijection α ≃ β
```
tab: Haskell
```haskell
-- a bijection bundled with its inverse (laws not enforced)
data Iso a b = Iso { to :: a -> b, from :: b -> a }
-- to . from = id, from . to = id
```
````
