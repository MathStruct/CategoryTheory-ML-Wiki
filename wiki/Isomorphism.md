#definition #example #theorem #proof

An **isomorphism** in a [[Category]] is a morphism $f : A \to B$ such that there exists $g : B \to A$ with $f \mathbin{;} g = \mathrm{id}_A$ and $g \mathbin{;} f = \mathrm{id}_B$ (i.e. $g \circ f = \mathrm{id}_A$, $f \circ g = \mathrm{id}_B$). We call $f, g$ **inverses**, write $g = f^{-1}$, and say $A$ and $B$ are **isomorphic**, $A \cong B$. Isomorphic objects are "interchangeable": for any other object $c$, maps into/out of $A$ correspond bijectively to maps into/out of $B$.

> Sources: 7 Sketches Definition 3.28, Examples 3.29, 3.34, Exercises 3.30–3.33; Kittenlab Lecture 2 (finite sets), 5; DaoFP Chapter 3 ("Isomorphisms", "Naturality", "Reasoning with Arrows"), §9.1; [[Isomorphism of Preorders]] and [[Bijection]] are special cases.

## Examples

- In $\mathbf{Set}$, isomorphisms are [[Bijection|bijections]]: $\{a,b,c\} \cong \underline{3}$ via $a \mapsto 2, b \mapsto 1, c \mapsto 3$ (inverse $1 \mapsto b$, $2 \mapsto a$, $3 \mapsto c$); there are $3! = 6$ such isomorphisms ([[7S Exercise 3.30]]). [[Cardinality]] is the isomorphism class (Cantor).
- In a [[Preorder]], $a \cong b$ iff $a \leq b$ and $b \leq a$ ([[Equivalent Elements of a Preorder]]).
- Every identity is an isomorphism, its own inverse ([[7S Exercise 3.31]]).
- A [[Monoid]] in which every morphism is an isomorphism is a [[Group]]: $\mathbb{N}$ is not a group ($s$ has no inverse), $\mathbb{Z}/2$ is ([[7S Exercise 3.32]]).
- In a [[Free Category]], only identities are isomorphisms ([[7S Exercise 3.33]]).
- **Retraction** (Example 3.34): $f : \underline{2} \to \underline{3}$, $g : \underline{3} \to \underline{2}$ with $f \mathbin{;} g = \mathrm{id}_2$ but $g \mathbin{;} f \neq \mathrm{id}_3$ are "almost but not quite" inverses — a [[Section and Retraction|section/retraction pair]].
- In programming, isomorphic types have the same external behaviour and can be swapped (except for performance): Kittenlab's `Vec𝔽([1,2,3,3])` and `Vec𝔽([3,2,1])`, `Coproduct{S,T}` vs `TaggedUnion{S,T}` (Lecture 11).
- Between [[Functor|functors]]: a [[Natural Isomorphism]]. Between categories: [[Equivalence of Categories]] (isomorphism of categories is too strict).

## Reasoning with arrows (DaoFP Chapter 3)

"We do not compare objects for equality" (that would be "evil"); we compare arrows. If $f : a \cong b$, then post-composition $(f \circ -)$ is a *bijection* $\mathcal{C}(x, a) \to \mathcal{C}(x, b)$ for every observer $x$, with inverse $(f^{-1} \circ -)$ — a "buddy system" between arrows. Changing perspective $x \to y$ via pre-composition $(- \circ g)$ commutes with changing focus: $(- \circ g) \circ (f \circ -) = (f \circ -) \circ (- \circ g)$, the first appearance of a **naturality condition** (here automatic by associativity).

**Theorem (Yoneda-style).** Conversely, suppose for every $x$ we have a bijection $\alpha_x : \mathcal{C}(x, a) \to \mathcal{C}(x, b)$ satisfying naturality $\alpha_y \circ (- \circ g) = (- \circ g) \circ \alpha_x$ for all $g : y \to x$. Then $a \cong b$. *Proof (the Yoneda trick).* Set $f := \alpha_a(\mathrm{id}_a)$. Naturality with $x = a$, $h = \mathrm{id}_a$ gives $\alpha_y(g) = \alpha_y(\mathrm{id}_a \circ g) = \alpha_a(\mathrm{id}_a) \circ g = f \circ g$, so $\alpha_y = (f \circ -)$ for every $y$; likewise $\alpha^{-1} = (f^{-1} \circ -)$ with $f^{-1} := \alpha_b^{-1}(\mathrm{id}_b)$. $\blacksquare$ "Even though $\alpha_x$ was defined individually for every $x$, it turned out to be completely determined by its value at a single identity arrow. This is the power of naturality!" Dually for outgoing arrows $\beta_x : \mathcal{C}(a, x) \to \mathcal{C}(b, x)$ ([[DaoFP Exercise 3.3.1]]). "To show an isomorphism, it is often easier to define a natural transformation between ten thousand arrows than to find a pair of arrows between two objects." See [[Yoneda Lemma]], [[Representable Functor]].

**Uniqueness of universal objects.** Any two [[Terminal Object|terminal objects]] are isomorphic by a *unique* isomorphism ([[DaoFP Exercise 3.1.3]], [[DaoFP Exercise 3.1.4]], 7 Sketches Proposition 3.84); likewise for all [[Limit|limits]], [[Colimit|colimits]], [[Representable Functor|representing objects]] — hence "the" product, "the" limit (Remark 3.85, Remark 1.82).

````tabs
tab: Julia
```julia
# Kittenlab Lecture 2: an isomorphism of finite sets and its inverse
B, B′ = Vec𝔽([1, 2, 3]), Vec𝔽([:a, :b, :c])
f = 𝔽Mor(B, B′, Dict(1 => :a, 2 => :b, 3 => :c))
g = 𝔽Mor(B′, B, Dict(:a => 1, :b => 2, :c => 3))
# compose(f, g) == identity(B) and compose(g, f) == identity(B′)

# Catlab
using Catlab
f = FinFunction([2, 1, 3], 3)
is_iso(f)                                       # true
compose(f, f) == id(FinSet(3))                  # f is its own inverse
```
tab: Lean
```lean
#check CategoryTheory.Iso        -- structure Iso X Y: hom, inv, hom_inv_id, inv_hom_id; notation X ≅ Y
#check CategoryTheory.IsIso      -- the property of a morphism
#check @CategoryTheory.Iso.symm
-- in Type, isomorphisms are equivalences
#check @CategoryTheory.Iso.toEquiv
-- "isomorphic iff the hom-functors are naturally isomorphic": the Yoneda embedding is fully faithful
#check CategoryTheory.Yoneda.fullyFaithful
```
tab: Haskell
```haskell
-- an isomorphism as a pair of inverse arrows (laws unenforced)
data Iso a b = Iso { fwd :: a -> b, bwd :: b -> a }
-- fwd . bwd = id, bwd . fwd = id

-- DaoFP: reconstructing f from a natural family of bijections via the Yoneda trick
fromNatural :: (forall x. (x -> a) -> (x -> b)) -> (a -> b)
fromNatural alpha = alpha id
```
````
