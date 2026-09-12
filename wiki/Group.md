#definition #example

A **group** is a [[Monoid]] in which every element has an inverse — equivalently, a one-object [[Category]] in which every morphism is an [[Isomorphism]] (a one-object [[Groupoid]]).

> Sources: 7 Sketches Exercise 3.32, Example 3.18 ($\mathbb{Z}/2\mathbb{Z}$), §3.2.4 ($\mathbf{Grp}$: "reversible action, symmetry"), Example 3.74 (abelianization is a left adjoint); Kittenlab Lecture 7 (natural transformations between group homomorphisms are conjugations).

- The monoid $\mathbb{N}$ of Example 3.13 is not a group ($s^n \mathbin{;} s = s^{n+1} \neq s^0$); the category presented by one loop $s$ with $s \mathbin{;} s = \mathrm{id}$ is the group $\mathbb{Z}/2\mathbb{Z}$ ([[7S Exercise 3.32]]).
- Functors between groups-as-categories are group homomorphisms; a [[Natural Transformation]] $f \Rightarrow g$ between homomorphisms $G \to H$ is an $h \in H$ with $f(x) = h\,g(x)\,h^{-1}$ — for abelian $H$ only $f = g$, but e.g. rotation by $\theta$ and by $-\theta$ in $GL_2(\mathbb{R})$ are conjugate by a reflection (Kittenlab Lecture 7).
- $\mathbf{Grp} \to \mathbf{Set}$ (forgetful) has a left adjoint (free group); $\mathbf{Ab} \hookrightarrow \mathbf{Grp}$ has left adjoint the abelianization $G \mapsto G/[G,G]$ ([[Free-Forgetful Adjunction]]).
- A [[Dagger Category|dagger]] structure in which every morphism is unitary makes a category a groupoid; symmetric-monoidal groupoids underlie [[Compact Closed Category|compact closed]] structure in physics.

````tabs
tab: Lean
```lean
#check Group
#check CategoryTheory.SingleObj       -- a group/monoid as a one-object category
#check CategoryTheory.Groupoid        -- every morphism is an iso
#check CategoryTheory.GrpCat          -- the category of groups
```
tab: Haskell
```haskell
class Monoid g => Group g where
  inverse :: g -> g
  -- inverse x <> x = mempty = x <> inverse x
newtype Z2 = Z2 Bool deriving (Eq, Show)
instance Semigroup Z2 where Z2 a <> Z2 b = Z2 (a /= b)
instance Monoid Z2 where mempty = Z2 False
instance Group Z2 where inverse = id
```
````
