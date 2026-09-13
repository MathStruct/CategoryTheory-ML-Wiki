#solution

Solutions to the exercises of 7 Sketches, Chapter 6: [[7S Chapter 6 Exercises]]. Index: [[Map of Content]].

## Solution 6.3

#example — [[7S Chapter 6 Exercises#Exercise 6.3|Exercise 6.3]]

1. The [[Discrete Preorder]] on $A$ (only $a \leq a$ and $b \leq b$): neither $a \leq b$ nor $b \leq a$, so no element has a morphism to every other.
2. The [[Walking Arrow]] $a \leq b$: $a$ is the unique initial object.
3. The [[Codiscrete Preorder]] ($a \leq b$ and $b \leq a$): both $a$ and $b$ are initial. Note that they are isomorphic, as [[7S Chapter 6 Exercises#Exercise 6.10|7S Exercise 6.10]] predicts.

> Sources: 7 Sketches, Exercise 6.3 and Solution A.6.

## Solution 6.6

#example — [[7S Chapter 6 Exercises#Exercise 6.6|Exercise 6.6]]

The objects of a free category are the vertices and the morphisms are paths, so an initial object is a vertex with exactly one path to every vertex (including itself).
1. Yes: $a$, whose only path to itself is the empty path.
2. Yes: $a$ has a unique path to $a$, $b$, $c$.
3. No: there is no path from $a$ to $b$ nor from $b$ to $a$.
4. No: $a$ has infinitely many paths to itself (the loop iterated $n$ times), so the hom-set $\mathcal{C}(a, a)$ is not a singleton.

> Sources: 7 Sketches, Exercise 6.6 and Solution A.6.

## Solution 6.7

#proof — [[7S Chapter 6 Exercises#Exercise 6.7|Exercise 6.7]]

1. $f(1_R) = 1_S$ and $f(r_1 *_R r_2) = f(r_1) *_S f(r_2)$.
2. The [[Natural Numbers]] rig $(\mathbb{N}, 0, +, 1, *)$. Given any rig $R$, a homomorphism $f : \mathbb{N} \to R$ must send $0 \mapsto 0_R$, $1 \mapsto 1_R$, and by additivity

$$
f(m) = f(1 + \cdots + 1) = 1_R +_R \cdots +_R 1_R \quad (m \text{ summands}),
$$

so $f$ is determined. This formula also preserves multiplication: by distributivity, $(1_R + \cdots + 1_R)$ ($m$ times) $*_R$ $(1_R + \cdots + 1_R)$ ($n$ times) expands to the sum of $mn$ copies of $1_R$, which is $f(m * n)$. Hence there is exactly one rig homomorphism $\mathbb{N} \to R$, i.e. $\mathbb{N}$ is initial.

````tabs
tab: Lean
```lean
import Mathlib
-- ℕ is the initial semiring: the unique ring hom is the canonical cast.
#check @Nat.castRingHom            -- (R : Type) [NonAssocSemiring R] : ℕ →+* R
#check @RingHom.eq_natCast         -- every f : ℕ →+* R equals Nat.cast
```
tab: Haskell
```haskell
-- the unique rig homomorphism from Nat into any semiring
fromNat :: Num r => Integer -> r
fromNat 0 = 0
fromNat n = 1 + fromNat (n - 1)
```
````

> Sources: 7 Sketches, Exercise 6.7 and Solution A.6.

## Solution 6.8

#annotation — [[7S Chapter 6 Exercises#Exercise 6.8|Exercise 6.8]]

The initial object $\varnothing \in \mathcal{C}$ is the universal thing. Since the property quantifies over *all* objects of $\mathcal{C}$, every object $c$ counts as a "comparable object". The [[Universal Property]] then reads: for every $c \in \mathcal{C}$ there is a unique morphism $\varnothing \to c$.

> Sources: 7 Sketches, Exercise 6.8 and Solution A.6.

## Solution 6.10

#proof — [[7S Chapter 6 Exercises#Exercise 6.10|Exercise 6.10]]

Since $c_1$ is initial there is a unique $f : c_1 \to c_2$; since $c_2$ is initial there is a unique $g : c_2 \to c_1$. Now $c_1 \to c_1$ has a unique morphism because $c_1$ is initial, and both $\mathrm{id}_{c_1}$ and $f \mathbin{;} g$ are such morphisms, so $f \mathbin{;} g = \mathrm{id}_{c_1}$. Symmetrically $g \mathbin{;} f = \mathrm{id}_{c_2}$. Hence $f$ is an isomorphism, and it is the *unique* isomorphism between them: initial objects are unique up to unique isomorphism.

````tabs
tab: Lean
```lean
import Mathlib
open CategoryTheory Limits
#check @Limits.initialIsoIsInitial   -- IsInitial X → (⊥_ C ≅ X)
#check @Limits.IsInitial.uniqueUpToIso
```
````

> Sources: 7 Sketches, Exercise 6.10 and Solution A.6.

## Solution 6.13

#proof — [[7S Chapter 6 Exercises#Exercise 6.13|Exercise 6.13]]

A preorder is a category with at most one morphism between any two objects, so every diagram commutes. Unfolding the definition of coproduct: $p + q$ is an element with $p \leq p + q$ and $q \leq p + q$ (the inclusions), such that whenever $p \leq x$ and $q \leq x$ we have $p + q \leq x$ (the unique copairing). That is exactly the least upper bound $p \vee q$.

Dually [[Product]]s are [[Meet]]s, and the [[Initial Object]] is the bottom element.

> Sources: 7 Sketches, Exercise 6.13 and Solution A.6.

## Solution 6.16

#example — [[7S Chapter 6 Exercises#Exercise 6.16|Exercise 6.16]]

| $A \sqcup B$ | apple$_1$ | banana$_1$ | pear$_1$ | cherry$_1$ | orange$_1$ | apple$_2$ | tomato$_2$ | mango$_2$ |
|---|---|---|---|---|---|---|---|---|
| $[f, g]$ | a | b | p | c | o | e | o | o |

The [[Coproduct]] in $\mathbf{Set}$ is the disjoint union, so the two apples are distinct elements with different images.

````tabs
tab: Julia
```julia
using Catlab
A = FinSet(5); B = FinSet(3); T = FinSet(26)            # letters as 1..26
letter(c) = Int(c) - Int('a') + 1
f = FinFunction(letter.(['a','b','p','c','o']), 26)     # first letters
g = FinFunction(letter.(['e','o','o']), 26)             # last letters
cp = coproduct(A, B)
h = copair(cp, f, g)                                    # [f, g] : 8 → 26
collect(h)                                              # [1, 2, 16, 3, 15, 5, 15, 15]
```
tab: Haskell
```haskell
copair :: (a -> t) -> (b -> t) -> Either a b -> t
copair = either
-- either (head) (last) :: Either String String -> Char
```
````

> Sources: 7 Sketches, Exercise 6.16 and Solution A.6.

## Solution 6.17

#proof — [[7S Chapter 6 Exercises#Exercise 6.17|Exercise 6.17]]

1–2. These are exactly the two commuting triangles in the diagram defining the copairing $[f, g]$.
3. Both $[f, g] \mathbin{;} h$ and $[f \mathbin{;} h, g \mathbin{;} h]$ are morphisms $A + B \to D$ whose precompositions with $\iota_A, \iota_B$ are $f \mathbin{;} h$ and $g \mathbin{;} h$ (by 1–2). The [[Universal Property]] says such a morphism is unique, so they are equal.
4. $\mathrm{id}_{A+B}$ satisfies $\iota_A \mathbin{;} \mathrm{id} = \iota_A$ and $\iota_B \mathbin{;} \mathrm{id} = \iota_B$; by uniqueness of the copairing, $[\iota_A, \iota_B] = \mathrm{id}_{A+B}$.

````tabs
tab: Lean
```lean
import Mathlib
open CategoryTheory Limits
#check @Limits.coprod.inl_desc   -- coprod.inl ≫ coprod.desc f g = f
#check @Limits.coprod.inr_desc
#check @Limits.coprod.desc_comp  -- coprod.desc f g ≫ h = coprod.desc (f ≫ h) (g ≫ h)
#check @Limits.coprod.desc_inl_inr
```
tab: Haskell
```haskell
-- either f g . Left  == f
-- either f g . Right == g
-- h . either f g == either (h . f) (h . g)
-- either Left Right == id
```
````

> Sources: 7 Sketches, Exercise 6.17 and Solution A.6.

## Solution 6.18

#proof — [[7S Chapter 6 Exercises#Exercise 6.18|Exercise 6.18]]

1. On objects take the coproduct; on a morphism $(f, g) : (A, B) \to (C, D)$ set $f + g := [f \mathbin{;} \iota_C,\ g \mathbin{;} \iota_D] : A + B \to C + D$. Identities are preserved: $\mathrm{id}_A + \mathrm{id}_B = [\iota_A, \iota_B] = \mathrm{id}_{A+B}$ by [[7S Chapter 6 Exercises#Exercise 6.17|7S Exercise 6.17]] (4). Composition is preserved because both $(f + g) \mathbin{;} (h + k)$ and $(f \mathbin{;} h) + (g \mathbin{;} k)$ equal $[f \mathbin{;} h \mathbin{;} \iota_E,\ g \mathbin{;} k \mathbin{;} \iota_F]$ by uniqueness of copairing.
2. Let $!_A : \varnothing \to A$ be the unique map. The copairing $[\mathrm{id}_A, !_A] : A + \varnothing \to A$ is inverse to $\iota_A$: $\iota_A \mathbin{;} [\mathrm{id}_A, !_A] = \mathrm{id}_A$, and $[\mathrm{id}_A, !_A] \mathbin{;} \iota_A = [\iota_A, !_A \mathbin{;} \iota_A] = [\iota_A, \iota_\varnothing] = \mathrm{id}_{A + \varnothing}$ (using that $!_A \mathbin{;} \iota_A$ and $\iota_\varnothing$ are both maps out of the initial object). Symmetrically for $[!_A, \mathrm{id}_A] : \varnothing + A \to A$.
3. (a) $\alpha = \big[\,[\iota_A,\ \iota_B \mathbin{;} \iota_{B+C}],\ \iota_C \mathbin{;} \iota_{B+C}]$ with inverse $[\iota_A \mathbin{;} \iota_{A+B},\ [\iota_B \mathbin{;} \iota_{A+B}, \iota_C]]$. (b) $\sigma = [\iota_A', \iota_B']$ where $\iota_A' : A \to B + A$, $\iota_B' : B \to B + A$ are the inclusions of the *other* coproduct; its inverse is the analogous map $B + A \to A + B$, and $\sigma \mathbin{;} \sigma^{-1} = \mathrm{id}$ by [[7S Chapter 6 Exercises#Exercise 6.17|7S Exercise 6.17]] (3–4).

The same argument dualised shows that finite [[Product]]s give a symmetric monoidal structure ([[Cartesian Category]]).

````tabs
tab: Julia
```julia
using Catlab
f = FinFunction([1, 2], 3); g = FinFunction([1], 2)
fg = oplus(f, g)                       # f + g : 3 → 5 in the prop FinSet
collect(fg)                            # [1, 2, 4]
```
tab: Lean
```lean
import Mathlib
open CategoryTheory
-- Mathlib packages exactly this: coproducts give a monoidal structure.
#check @CategoryTheory.monoidalOfHasFiniteCoproducts
```
tab: Haskell
```haskell
import Data.Bifunctor (bimap)          -- bimap f g :: Either a b -> Either c d
assoc :: Either (Either a b) c -> Either a (Either b c)
assoc = either (either Left (Right . Left)) (Right . Right)
swap :: Either a b -> Either b a
swap = either Right Left
```
````

> Sources: 7 Sketches, Exercise 6.18 and Solution A.6.

## Solution 6.24

#proof — [[7S Chapter 6 Exercises#Exercise 6.24|Exercise 6.24]]

1. A span $B \leftarrow A \to C$ in $\mathbf{Disc}_S$ consists of identities, so $A = B = C$, and the square of identities on $A$ is a pushout: any cocone consists of two equal maps $A \to T$ (both identities, so $T = A$), and the identity is the unique mediating map.
2. Exactly when $|S| = 1$. If $S = \varnothing$ there is no object at all; if $s \neq s'$ are two objects there is no morphism $s \to s'$.

> Sources: 7 Sketches, Exercise 6.24 and Solution A.6.

## Solution 6.26

#example #program — [[7S Chapter 6 Exercises#Exercise 6.26|Exercise 6.26]]

The pushout is the set of connected components of $\underline{5} \sqcup \underline{3}$ under the relation generated by $f(a) \sim g(a)$: $1 \sim 1'$, $3 \sim 1'$, $5 \sim 2'$, $5 \sim 3'$. The classes are $\{1, 1', 3\}$, $\{2\}$, $\{4\}$, $\{5, 2', 3'\}$: the pushout is $\underline{4}$, with $\underline{5} \to \underline{4}$ given by $(1, 2, 1, 3, 4)$ and $\underline{3} \to \underline{4}$ given by $(1, 4, 4)$.

````tabs
tab: Julia
```julia
using Catlab
f = FinFunction([1, 3, 5, 5], 5)
g = FinFunction([1, 1, 2, 3], 3)
P = pushout(f, g)
ob(P)                     # FinSet(4)
collect.(legs(P))         # ([1, 2, 1, 3, 4], [1, 4, 4])
```
tab: Haskell
```haskell
-- pushout of finite functions via union-find on the disjoint union
import Data.List (nub)
pushout :: Int -> Int -> [Int] -> [Int] -> [[Int]]   -- classes of 5 ⊔ 3, elements of Y offset
pushout nx ny f g = classes (zip f (map (+ nx) g)) [1 .. nx + ny]
  where classes rel xs = nub [ closure [x] | x <- xs ]
          where step cs = nub (cs ++ [ b | (a,b) <- rel, a `elem` cs ] ++ [ a | (a,b) <- rel, b `elem` cs ])
                closure cs = let cs' = step cs in if length cs' == length cs then cs else closure cs'
-- pushout 5 3 [1,3,5,5] [1,1,2,3] gives 4 classes
```
````

> Sources: 7 Sketches, Exercise 6.26 and Solution A.6; Kittenlab lecture on colimits (union-find).

## Solution 6.28

#proof — [[7S Chapter 6 Exercises#Exercise 6.28|Exercise 6.28]]

1. The square $X \leftarrow \varnothing \to Y$, $X \to X + Y \leftarrow Y$ commutes because there is only one map $\varnothing \to X + Y$; so $f \mathbin{;} \iota_X = g \mathbin{;} \iota_Y$.
2. Given $x : X \to T$, $y : Y \to T$ (the square with $\varnothing$ commutes automatically), the universal property of the coproduct gives a unique $[x, y] : X + Y \to T$ with $\iota_X \mathbin{;} [x, y] = x$ and $\iota_Y \mathbin{;} [x, y] = y$.
3. Conversely, if the pushout $X +_\varnothing Y$ exists then for any $x, y$ as above, the outer square commutes (again because $\varnothing$ is initial), so the pushout property gives a unique $t : X +_\varnothing Y \to T$ with $\iota_X \mathbin{;} t = x$, $\iota_Y \mathbin{;} t = y$. This is exactly the universal property of the coproduct.

> Sources: 7 Sketches, Exercise 6.28 and Solution A.6.

## Solution 6.35

#proof — [[7S Chapter 6 Exercises#Exercise 6.35|Exercise 6.35]]

Suppose $T$ is a [[Cocone]] on the original diagram: maps from $X, Y, Z$ to $T$ making the two squares with $A$ and $B$ commute. Since $Q$ is the pushout of $X \leftarrow A \to Y$ there is a unique $Q \to T$ compatible with $X, Y$; since $R$ is the pushout of $Y \leftarrow B \to Z$ there is a unique $R \to T$ compatible with $Y, Z$. Both agree with the given map on $Y$, so $(Q, R, T)$ forms a cocone on $Q \leftarrow Y \to R$, and the pushout $S$ gives a unique $S \to T$ making everything commute. Hence $S$ is the colimit. This is the mechanism behind [[Finite Colimits in Set]]: an initial object and pushouts give all finite colimits.

````tabs
tab: Lean
```lean
import Mathlib
open CategoryTheory Limits
-- finite colimits from an initial object and pushouts
#check @CategoryTheory.Limits.hasFiniteColimits_of_hasInitial_and_pushouts
```
````

> Sources: 7 Sketches, Exercise 6.35 and Solution A.6.

## Solution 6.41

#proof — [[7S Chapter 6 Exercises#Exercise 6.41|Exercise 6.41]]

Theorem 6.37 says the colimit is the quotient of $X \sqcup N \sqcup Y$ by the equivalence relation generated by $x \sim n$ if $x = f(n)$ and $y \sim n$ if $y = g(n)$. Every $n \in N$ is related to $f(n) \in X$, so each class contains an element of $X \sqcup Y$; thus the quotient equals the quotient of $X \sqcup Y$ by the relation generated by $f(n) \sim g(n)$, which is exactly Example 6.25.

> Sources: 7 Sketches, Exercise 6.41 and Solution A.6.

## Solution 6.48

#example — [[7S Chapter 6 Exercises#Exercise 6.48|Exercise 6.48]]

The monoidal product of cospans $A \to N \leftarrow B$ and $B \to P \leftarrow C$ is the cospan $A + B \to N + P \leftarrow B + C$: one simply stacks the two wiring pictures vertically (disjoint union of apices and of feet). See [[Hypergraph Category]] for the general structure.

````tabs
tab: Julia
```julia
using Catlab
c1 = Cospan(FinFunction([1, 1], 2), FinFunction([2], 2))       # A=2 → N=2 ← B=1
c2 = Cospan(FinFunction([1], 3), FinFunction([1, 2, 3], 3))   # B=1 → P=3 ← C=3
c12 = Cospan(oplus(left(c1), left(c2)), oplus(right(c1), right(c2)))
apex(c12)                                                     # FinSet(5) = N + P
```
````

> Sources: 7 Sketches, Exercise 6.48 and Solution A.6.

## Solution 6.49

#annotation — [[7S Chapter 6 Exercises#Exercise 6.49|Exercise 6.49]]

Concatenate the two wire diagrams along the shared foot $B$. (i) The apex of the composite has one element for each connected component of the concatenated picture (this is the [[Pushout]] as a set of connected components, cf. [[Colimits and Connection]]). (ii) Each element of the outer feet $A$, $C$ is wired to the element representing the component it belongs to.

> Sources: 7 Sketches, Exercise 6.49 and Solution A.6.

## Solution 6.57

#example — [[7S Chapter 6 Exercises#Exercise 6.57|Exercise 6.57]]

By Theorem 6.55 (the spider theorem: a connected Frobenius diagram is determined by its number of inputs and outputs), two connected diagrams with the same inputs and outputs are equal, and a disconnected diagram is determined by its partition of the ports. Morphisms 1, 4 and 6 are equal (a single connected spider $2 \to 3$); morphisms 3 and 5 are equal (the same disconnected pattern); morphism 2 is not equal to any other.

> Sources: 7 Sketches, Exercise 6.57 and Solution A.6.

## Solution 6.59

#example — [[7S Chapter 6 Exercises#Exercise 6.59|Exercise 6.59]]

1. $B$: the wire into $h$ comes from a spider whose other legs are labelled $B$, and spiders connect only wires of the same type.
2. $D$, since $h : B \to D \otimes D$ and $g$'s output shares a spider with $h$'s outputs.
3. $D$ as well, for the same reason.

> Sources: 7 Sketches, Exercise 6.59 and Solution A.6.

## Solution 6.62

#example — [[7S Chapter 6 Exercises#Exercise 6.62|Exercise 6.62]]

| | cospan | wiring |
|---|---|---|
| multiplication $\mu$ | $\underline{2} \xrightarrow{[\mathrm{id},\mathrm{id}]} \underline{1} \xleftarrow{\mathrm{id}} \underline{1}$ | two wires merging into one |
| unit $\eta$ | $\underline{0} \xrightarrow{!} \underline{1} \xleftarrow{\mathrm{id}} \underline{1}$ | a wire starting from nothing |
| comultiplication $\delta$ | $\underline{1} \xrightarrow{\mathrm{id}} \underline{1} \xleftarrow{[\mathrm{id},\mathrm{id}]} \underline{2}$ | one wire splitting into two |
| counit $\epsilon$ | $\underline{1} \xrightarrow{\mathrm{id}} \underline{1} \xleftarrow{!} \underline{0}$ | a wire ending in nothing |

The empty set is depicted as blank space.

````tabs
tab: Julia
```julia
using Catlab
μ = Cospan(FinFunction([1, 1], 1), FinFunction([1], 1))
η = Cospan(FinFunction(Int[], 1), FinFunction([1], 1))
δ = Cospan(FinFunction([1], 1), FinFunction([1, 1], 1))
ε = Cospan(FinFunction([1], 1), FinFunction(Int[], 1))
```
````

> Sources: 7 Sketches, Exercise 6.62 and Solution A.6.

## Solution 6.63

#proof — [[7S Chapter 6 Exercises#Exercise 6.63|Exercise 6.63]]

The composite $\delta \mathbin{;} \mu$ is the cospan $X \xrightarrow{\mathrm{id}} X \xleftarrow{[\mathrm{id},\mathrm{id}]} X + X \xrightarrow{[\mathrm{id},\mathrm{id}]} X \xleftarrow{\mathrm{id}} X$, so it suffices to show that the square with $X + X$ at the top left, two copies of $[\mathrm{id}, \mathrm{id}] : X + X \to X$, and two identities $X \to X$ is a [[Pushout]]. It commutes trivially. Given $f, g : X \to T$ with $[\mathrm{id},\mathrm{id}] \mathbin{;} f = [\mathrm{id},\mathrm{id}] \mathbin{;} g$, precomposing with $\iota_1 : X \to X + X$ and using $\iota_1 \mathbin{;} [\mathrm{id},\mathrm{id}] = \mathrm{id}$ ([[7S Chapter 6 Exercises#Exercise 6.17|7S Exercise 6.17]]) gives $f = g$. Then $f$ is the unique map from the apex $X$ making the cocone commute. Hence the pushout apex is $X$ with identity legs, i.e. $\delta \mathbin{;} \mu = \mathrm{id}_X$.

````tabs
tab: Julia
```julia
using Catlab
X = FinSet(2)
δ = Cospan(id(X), FinFunction([1, 2, 1, 2], 2))
μ = Cospan(FinFunction([1, 2, 1, 2], 2), id(X))
P = pushout(right(δ), left(μ))
ob(P)                                # FinSet(2): the special law holds
```
````

> Sources: 7 Sketches, Exercise 6.63 and Solution A.6.

## Solution 6.67

#proof — [[7S Chapter 6 Exercises#Exercise 6.67|Exercise 6.67]]

The cup is $\eta \mathbin{;} \delta : I \to X \otimes X$ and the cap is $\mu \mathbin{;} \epsilon : X \otimes X \to I$. The snake equation $(\eta \mathbin{;} \delta) \otimes \mathrm{id} \mathbin{;} \mathrm{id} \otimes (\mu \mathbin{;} \epsilon) = \mathrm{id}_X$ is proved by the chain: apply the Frobenius law (6.53) to move the $\delta$ past the $\mu$, obtaining $\mathrm{id} \otimes \eta \mathbin{;} \mu \mathbin{;} \delta \mathbin{;} \epsilon \otimes \mathrm{id}$; the missing diagram is this middle step, in which the unit and counit are attached to the multiplication and comultiplication respectively. Then the unit law $(\mathrm{id} \otimes \eta) \mathbin{;} \mu = \mathrm{id}$ and its opposite $\delta \mathbin{;} (\epsilon \otimes \mathrm{id}) = \mathrm{id}$ reduce it to $\mathrm{id}_X$.

> Sources: 7 Sketches, Exercise 6.67 and Solution A.6.

## Solution 6.70

#proof — [[7S Chapter 6 Exercises#Exercise 6.70|Exercise 6.70]]

Let $A \subseteq S$, $B \subseteq T$. Then

$$
\varphi_{S',T'}(\mathrm{im}_f(A), \mathrm{im}_g(B)) = \{f(a) \mid a \in A\} \times \{g(b) \mid b \in B\} = \{(f(a), g(b)) \mid a \in A, b \in B\} = \mathrm{im}_{f \times g}(A \times B) = \mathrm{im}_{f \times g}(\varphi_{S,T}(A, B)),
$$

so the square commutes.

> Sources: 7 Sketches, Exercise 6.70 and Solution A.6.

## Solution 6.78

#annotation — [[7S Chapter 6 Exercises#Exercise 6.78|Exercise 6.78]]

Take $F : \mathcal{C} \to \mathbf{Set}$ constant at the singleton $\{*\}$; it is lax symmetric monoidal (all structure maps are the unique map into $\{*\}$). A morphism in $\mathbf{Cospan}_F$ is a cospan $X \to N \leftarrow Y$ together with an element of $F(N) = \{*\}$, i.e. no extra choice. Composition is the usual pushout composition since the decoration is forced. Hence $\mathbf{Cospan}_F \cong \mathbf{Cospan}_{\mathcal{C}}$ via the identity-on-objects functor that decorates each cospan with $*$; category theorists happily call these "equal".

> Sources: 7 Sketches, Exercise 6.78 and Solution A.6.

## Solution 6.79

#example #program — [[7S Chapter 6 Exercises#Exercise 6.79|Exercise 6.79]]

$V = \{ul, ur, dl, dr\}$, $A = \{r_1, r_2, r_3, c_1, i_1\}$, with

| | $r_1$ | $r_2$ | $r_3$ | $c_1$ | $i_1$ |
|---|---|---|---|---|---|
| $s$ | dl | ul | ur | ul | dl |
| $t$ | ul | ur | dr | ur | dr |
| $\ell$ | $1\Omega$ | $2\Omega$ | $1\Omega$ | $3F$ | $1H$ |

A $\mathcal{C}$-circuit is precisely an edge-labelled [[Graph]] ([[C-Set]] on the schema of graphs with a label attribute).

````tabs
tab: Julia
```julia
using Catlab
@present SchCircuit <: SchGraph begin
  Label::AttrType
  label::Attr(E, Label)
end
@acset_type Circuit(SchCircuit, index=[:src, :tgt])
# vertices 1=ul 2=ur 3=dl 4=dr
c = @acset Circuit{String} begin
  V = 4; E = 5
  src = [3, 1, 2, 1, 3]; tgt = [1, 2, 4, 2, 4]
  label = ["1Ω", "2Ω", "1Ω", "3F", "1H"]
end
```
tab: Haskell
```haskell
data Circuit v l = Circuit { arrows :: [(v, v, l)] }   -- (s a, t a, ℓ a)
c :: Circuit String String
c = Circuit [("dl","ul","1Ω"),("ul","ur","2Ω"),("ur","dr","1Ω"),("ul","ur","3F"),("dl","dr","1H")]
```
````

> Sources: 7 Sketches, Exercise 6.79 and Solution A.6.

## Solution 6.80

#example — [[7S Chapter 6 Exercises#Exercise 6.80|Exercise 6.80]]

The decoration functor $\mathrm{Circ} : \mathbf{FinSet} \to \mathbf{Set}$ acts on a function $f$ by relabelling vertices: $\mathrm{Circ}(f)(V, A, s, t, \ell) = (V', A, f \mathbin{;} s, f \mathbin{;} t, \ell)$. So $\mathrm{Circ}(f)(c)$ has vertices $\{1, 2 \sim 3, 4\}$ and the $3\Omega$ resistor now runs from the merged vertex $2 \sim 3$ to $4$; the wire from $1$ ends at $2 \sim 3$.

````tabs
tab: Julia
```julia
using Catlab
# Circ(f) is the pushforward of the vertex set: Σ-migration along f on V
@present SchCircuit <: SchGraph begin Label::AttrType; label::Attr(E, Label) end
@acset_type Circuit(SchCircuit, index=[:src, :tgt])
c = @acset Circuit{String} begin V = 4; E = 1; src = [3]; tgt = [4]; label = ["3Ω"] end
f = [1, 2, 2, 3]                                    # 4 → 3, identifies 2 and 3
c′ = @acset Circuit{String} begin V = 3; E = 1; src = f[c[:src]]; tgt = f[c[:tgt]]; label = c[:label] end
```
````

> Sources: 7 Sketches, Exercise 6.80 and Solution A.6.

## Solution 6.82

#example — [[7S Chapter 6 Exercises#Exercise 6.82|Exercise 6.82]]

$\psi_{V,V'}$ takes the disjoint union of labelled graphs: $\psi_{2,2}(b, s)$ is the 4-vertex circuit consisting of the battery between vertices $1, 2$ and the switch between vertices $3, 4$, with no connection between them. This laxator is what makes $\mathrm{Circ}$ a lax [[Monoidal Functor]], and thus $\mathbf{Cospan}_{\mathrm{Circ}}$ a [[Hypergraph Category]] ([[Decorated Cospan]]).

````tabs
tab: Julia
```julia
using Catlab
@present SchCircuit <: SchGraph begin Label::AttrType; label::Attr(E, Label) end
@acset_type Circuit(SchCircuit, index=[:src, :tgt])
b = @acset Circuit{Symbol} begin V = 2; E = 1; src = [1]; tgt = [2]; label = [:battery] end
s = @acset Circuit{Symbol} begin V = 2; E = 1; src = [1]; tgt = [2]; label = [:switch] end
ψ = ob(coproduct(b, s))          # ψ₂,₂(b, s): 4 vertices, 2 edges
```
````

> Sources: 7 Sketches, Exercise 6.82 and Solution A.6.

## Solution 6.84

#example — [[7S Chapter 6 Exercises#Exercise 6.84|Exercise 6.84]]

The cospan is $\underline{1} \xrightarrow{f} \underline{2} \xleftarrow{g} \underline{1}$ with $f(1) = 1$, $g(1) = 2$. The decoration is the $\mathcal{C}$-circuit $(\underline{2}, \{a\}, s, t, \ell)$ with $s(a) = 1$, $t(a) = 2$, $\ell(a) = \mathrm{battery}$: an open battery whose two terminals are exposed on the left and right. See the Julia snippet in [[Decorated Cospan]].

> Sources: 7 Sketches, Exercise 6.84 and Solution A.6.

## Solution 6.86

#example #program — [[7S Chapter 6 Exercises#Exercise 6.86|Exercise 6.86]]

The first is the cospan $\underline{1} \xrightarrow{f} V \xleftarrow{g} \underline{2}$, $f(1) = ul$, $g(1) = g(2) = ur$, decorated by the circuit $C$ of [[7S Chapter 6 Exercises#Exercise 6.79|7S Exercise 6.79]]. The second is $\underline{2} \xrightarrow{f'} V' \xleftarrow{g'} \underline{2}$ with $V' = \{l, r, d\}$, $f'(1) = l$, $f'(2) = d$, $g'(1) = g'(2) = r$, decorated by $C' = (V', \{r_1', r_2'\}, s', t', \ell')$ with $r_1' : l \to r$ ($5\Omega$), $r_2' : r \to d$ ($8\Omega$).

Composing: the [[Pushout]] of $V \xleftarrow{g} \underline{2} \xrightarrow{f'} V'$ identifies $ur \sim l \sim d$ into one vertex $m$, giving $V'' = \{ul, dl, dr, m, r\}$ (five vertices), and the composite cospan $\underline{1} \to V'' \leftarrow \underline{2}$ has $1 \mapsto ul$ and $(1, 2) \mapsto (r, m)$. The decoration is $\mathrm{Circ}$ of the pushout applied to $\psi(C, C')$: arrows $r_1 : dl \to ul$, $r_2 : ul \to m$, $r_3 : m \to dr$, $c_1 : ul \to m$, $i_1 : dl \to dr$, $r_1' : m \to r$, $r_2' : r \to m$. This matches Eq. (6.74).

````tabs
tab: Julia
```julia
using Catlab
@present SchCircuit <: SchGraph begin Label::AttrType; label::Attr(E, Label) end
@acset_type Circuit(SchCircuit, index=[:src, :tgt])
const OpenCircuitOb, OpenCircuit = OpenACSetTypes(Circuit, :V)
# vertices 1=ul 2=ur 3=dl 4=dr
C = @acset Circuit{String} begin V = 4; E = 5
  src = [3, 1, 2, 1, 3]; tgt = [1, 2, 4, 2, 4]; label = ["1Ω", "2Ω", "1Ω", "3F", "1H"] end
C′ = @acset Circuit{String} begin V = 3; E = 2       # 1=l 2=r 3=d
  src = [1, 2]; tgt = [2, 3]; label = ["5Ω", "8Ω"] end
x = OpenCircuit{String}(C, FinFunction([1], 4), FinFunction([2, 2], 4))
y = OpenCircuit{String}(C′, FinFunction([1, 3], 3), FinFunction([2, 2], 3))
xy = compose(x, y)
nparts(apex(xy), :V), nparts(apex(xy), :E)          # (5, 7)
```
````

> Sources: 7 Sketches, Exercise 6.86 and Solution A.6.

## Solution 6.88

#example — [[7S Chapter 6 Exercises#Exercise 6.88|Exercise 6.88]]

$\eta \mathbin{;} x$ identifies the two left terminals of $x$ into one vertex (a wire closing the left side); $\eta \mathbin{;} x \mathbin{;} \epsilon$ then also identifies the two right terminals. The result is a cospan $\underline{0} \to N \leftarrow \underline{0}$ decorated with the circuit of $x$ in which the left pair and the right pair of terminals are each merged: a closed loop with no exposed terminals. Such closed circuits are the scalars $I \to I$ of the [[Hypergraph Category]].

````tabs
tab: Julia
```julia
using Catlab
@present SchCircuit <: SchGraph begin Label::AttrType; label::Attr(E, Label) end
@acset_type Circuit(SchCircuit, index=[:src, :tgt])
const OpenCircuitOb, OpenCircuit = OpenACSetTypes(Circuit, :V)
x = @acset Circuit{Symbol} begin V = 4; E = 2; src = [1, 3]; tgt = [2, 4]; label = [:battery, :resistor] end
ox = OpenCircuit{Symbol}(x, FinFunction([1, 3], 4), FinFunction([2, 4], 4))
empty1 = @acset Circuit{Symbol} begin V = 1 end
η = OpenCircuit{Symbol}(empty1, FinFunction(Int[], 1), FinFunction([1, 1], 1))
ε = OpenCircuit{Symbol}(empty1, FinFunction([1, 1], 1), FinFunction(Int[], 1))
closed = compose(η, ox, ε)
nparts(apex(closed), :V), nparts(apex(closed), :E)   # (2, 2): a closed loop
```
````

> Sources: 7 Sketches, Exercise 6.88 and Solution A.6.

## Solution 6.96

#example #program — [[7S Chapter 6 Exercises#Exercise 6.96|Exercise 6.96]]

1. Two inner circles with two ports each, an outer circle with two ports, and three links: one port of the first circle is wired to a port of the second; the remaining port of each inner circle is wired to an outer port.
2. Three inner circles with two ports each, no outer ports; the links connect the circles in a chain.
3. $g \circ_1 f$ substitutes $f$ into the first circle of $g$; the operad composition is a pushout of the apices along the shared foot $\underline{2}$. Its arity is $(2, 2, 2, 2; 0)$: four inner circles and no outer ports.
4. The drawing shows $f$'s two circles sitting where the first circle of $g$ used to be — literally substitution of one wiring diagram into a circle of another.

````tabs
tab: Julia
```julia
using Catlab
f = UndirectedWiringDiagram(2)        # outer circle with 2 ports
add_box!(f, 2); add_box!(f, 2)        # two inner circles, 2 ports each
add_junctions!(f, 3)                  # apex 3
set_junction!(f, [1, 2, 2, 3])        # box ports → junctions
set_junction!(f, [1, 3], outer=true)  # outer ports → junctions
g = UndirectedWiringDiagram(0)
add_box!(g, 2); add_box!(g, 2); add_box!(g, 2)
add_junctions!(g, 3)
set_junction!(g, [1, 2, 2, 3, 3, 1])
h = ocompose(g, 1, f)                 # substitute f into box 1 of g
nboxes(h), length(ports(h, outer=true)), njunctions(h)   # (4, 0, 4)
```
````

> Sources: 7 Sketches, Exercise 6.96 and Solution A.6.
