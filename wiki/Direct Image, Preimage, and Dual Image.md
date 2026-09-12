#example #definition #theorem #program

Let $f : A \to B$ be a [[Function]] — think of $A$ as apples, $B$ as buckets, and $f$ as putting each apple in a bucket. Three [[Monotone Map|monotone maps]] between the [[Power Set|power sets]] are induced automatically, forming two [[Galois Connection|Galois connections]] $f_! \dashv f^* \dashv f_*$:

| map | formula | apples & buckets |
|---|---|---|
| **preimage / pullback** $f^* : \mathcal{P}(B) \to \mathcal{P}(A)$ | $f^*(B') = f^{-1}(B') = \{a \mid f(a) \in B'\}$ | all apples in the chosen buckets |
| **direct image** $f_! : \mathcal{P}(A) \to \mathcal{P}(B)$ (left adjoint) | $f_!(A') = \{b \mid \exists a \in A'.\ f(a) = b\}$ | buckets containing at least one chosen apple |
| **dual image** $f_* : \mathcal{P}(A) \to \mathcal{P}(B)$ (right adjoint) | $f_*(A') = \{b \mid \forall a.\ f(a) = b \Rightarrow a \in A'\}$ | buckets *all* of whose apples are chosen (empty buckets count) |

> Sources: 7 Sketches Example 1.117, Exercise 1.118; Kittenlab Lecture 14 ("Pullback", "Direct image"); DaoFP §11.3–11.4 (dependent sum and product as adjoints to substitution), §7.4.4 of 7 Sketches (quantification).

Kittenlab (Lecture 14) phrases the same in terms of characteristic functions: $f^*(\chi) = \chi \circ f$ ("pullback", also "preimage"), and $f_*(\chi)(y) = [\exists x.\ \chi(x) \wedge f(x) = y]$ (its $f_*$ is 7 Sketches' $f_!$). Both preserve the ordering of subsets, making $\mathcal{P}$ a contravariant and a covariant [[Functor]] $\mathbf{Set} \to \mathbf{Pos}$.

*Proof that $f^*$ is monotone* (Kittenlab): if $\chi \subseteq \chi'$ and $f^*(\chi)(x)$ holds then $\chi(f(x))$, hence $\chi'(f(x))$, hence $f^*(\chi')(x)$. $\blacksquare$

**Why it matters.** "We did not invent these mappings: they were induced by $f$. It is one of the pleasures of category theory that adjoints so often turn out to have interesting semantic interpretations." The adjoints $f_! \dashv f^* \dashv f_*$ are the [[Quantification|existential and universal quantifiers]] $\exists_f \dashv f^* \dashv \forall_f$ of [[Topos|topos]] logic (7 Sketches §7.4.4) and, at the level of [[Category|categories]], the [[Dependent Sum]] $\Sigma_f \dashv f^* \dashv \Pi_f$ [[Dependent Product]] adjoints to the [[Base Change Functor|base-change]] (DaoFP Ch. 11); for [[Database Schema|databases]], $\Sigma_F \dashv \Delta_F \dashv \Pi_F$ ([[Data Migration Functor]]).

**Example** ([[7S Exercise 1.118]] solution): $X = \{a_1, c_1, c_2\} \to Y = \{a, b, c\}$ projecting down. $f^*\{a,b\} = \{a_1\}$, $f^*\{c\} = \{c_1, c_2\}$; $f_!\{a_1, c_1\} = \{a, c\}$; $f_*\varnothing = \{b\}$ (the empty bucket), $f_*\{a_1, c_1\} = \{a, b\}$.

````tabs
tab: Julia
```julia
# Kittenlab Lecture 14 on subsets of {1..n} as BitVectors
struct FinSet′; n::Int end
struct FinFunction′; dom::FinSet′; codom::FinSet′; values::Vector{Int} end
const FinSubset = BitVector

preimage(f::FinFunction′, U::FinSubset) = FinSubset([U[y] for y in f.values])      # f^*

function direct_image(f::FinFunction′, U::FinSubset)                                 # f_!
  V = FinSubset(zeros(Bool, f.codom.n))
  for i in 1:f.dom.n
    U[i] && (V[f.values[i]] = true)
  end
  V
end

function dual_image(f::FinFunction′, U::FinSubset)                                   # f_*
  FinSubset([all(U[i] for i in 1:f.dom.n if f.values[i] == y) for y in 1:f.codom.n])
end

f = FinFunction′(FinSet′(3), FinSet′(3), [1, 3, 3])   # a₁ ↦ a, c₁,c₂ ↦ c
direct_image(f, FinSubset([true, true, false]))        # {a, c}
dual_image(f, FinSubset([false, false, false]))        # {b}: the empty bucket

# Catlab
using Catlab
g = FinFunction([1, 3, 3], 3)
preimage(g, 3)          # [2, 3]
```
tab: Lean
```lean
#check @Set.image        -- f '' A  = f_! A
#check @Set.preimage     -- f ⁻¹' B = f^* B
-- the two Galois connections
#check @Set.image_preimage   -- GaloisConnection (Set.image f) (Set.preimage f)
#check @Set.preimage_kernImage -- GaloisConnection (Set.preimage f) (Set.kernImage f)  (f_* = kernImage)
```
tab: Haskell
```haskell
type Subset a = a -> Bool

preimage :: (a -> b) -> Subset b -> Subset a
preimage f chi = chi . f                                        -- f^*

directImage :: Eq b => [a] -> (a -> b) -> Subset a -> Subset b
directImage as f chi b = or  [chi a | a <- as, f a == b]        -- f_! : ∃

dualImage :: Eq b => [a] -> (a -> b) -> Subset a -> Subset b
dualImage as f chi b = and [chi a | a <- as, f a == b]          -- f_* : ∀
```
````
