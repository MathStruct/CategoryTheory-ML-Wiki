#definition #example

A **group** is a [[Monoid]] in which every element has an inverse — equivalently, a one-object [[Category]] in which every morphism is an [[Isomorphism]] (a one-object [[Groupoid]]).

> Sources: 7 Sketches Exercise 3.32, Example 3.18 ($\mathbb{Z}/2\mathbb{Z}$), §3.2.4 ($\mathbf{Grp}$: "reversible action, symmetry"), Example 3.74 (abelianization is a left adjoint); Kittenlab Lecture 7 (natural transformations between group homomorphisms are conjugations); CTfS §3.2 (Definition 3.2.1.1, Proposition 3.2.1.2, Examples 3.2.1.3–3.2.1.10, Application 3.2.1.6, Exercises 3.2.1.7–3.2.1.14), Slogan 4.2.1.5, Application 4.1.2.4

- **Examples** (CTfS §3.2): $(\mathbb Z, 0, +)$; the clock $\mathbb Z/12$ (the inverse of $Q^5$ is $Q^7$); the eight symmetries $\{\mathrm{id}, \rho, \rho^2, \rho^3, \varphi, \varphi\rho, \varphi\rho^2, \varphi\rho^3\}$ of a square with $\rho^4 = \varphi^2 = \mathrm{id}$, $\rho^3\varphi = \varphi\rho$; invertible matrices $GL_3$, the orthogonal group $O_3$ (symmetries of the sphere) and the Euclidean group $E(3)$ of isometries of $\mathbb R^3$; the permutations $\Sigma_X$ of a set; the circle group $U(1)$ of angles. In crystallography the *space group* of an arrangement of atoms $A \subseteq \mathbb R^3$ consists of the isometries $f$ of $\mathbb R^3$ that map $A$ into $A$ (CTfS Application 3.2.1.6). Inverses are unique: $m' = m'(m m'') = (m' m) m'' = m''$ (CTfS Proposition 3.2.1.2). The cyclic groups are $\mathbb Z$ and $\mathbb Z/n$ — exactly the cyclic monoids "without a tail" ([[Presentation of a Monoid]]).
- **Time-reversibility** (CTfS §3.2): "monoids are likely useful in thinking about diffusion, in which time plays a role and things cannot be undone; groups are more likely useful in thinking about mechanics, where actions are time-reversible". When a symmetry breaks, pass along the forgetful functor $\mathbf{Grp} \to \mathbf{Mon}$ and keep working with the monoid (CTfS Application 4.1.2.4). How groups act on sets — orbits, latitudes on the earth — is in [[Group Action]].
- The monoid $\mathbb{N}$ of Example 3.13 is not a group ($s^n \mathbin{;} s = s^{n+1} \neq s^0$); the category presented by one loop $s$ with $s \mathbin{;} s = \mathrm{id}$ is the group $\mathbb{Z}/2\mathbb{Z}$ ([[7S Chapter 3 Exercises#Exercise 3.32|7S Exercise 3.32]]).
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
