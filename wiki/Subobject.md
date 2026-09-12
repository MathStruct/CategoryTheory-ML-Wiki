#definition #example

A **subobject** of an object $Y$ in a [[Category]] is (an isomorphism class of) a [[Monomorphism]] $m : X \rightarrowtail Y$. Two monos $m : X \to Y$, $m' : X' \to Y$ define the same subobject when there is an [[Isomorphism]] $i : X \cong X'$ with $i \mathbin{;} m' = m$. Subobjects of $Y$ are preordered by factorization ($m \leq m'$ iff $m = k \mathbin{;} m'$ for some $k$), giving the poset $\mathrm{Sub}(Y)$.

> Sources: 7 Sketches §7.2.2 ("it will do no harm to think of monomorphisms into $Y$ as subobjects of $Y$"), Definition 7.12, §7.4.3 ("The poset of subobjects"); Kittenlab Lecture 14 (subsets as injections vs. as characteristic functions); DaoFP §2.4.

- In $\mathbf{Set}$, a mono $X \rightarrowtail Y$ is an [[Injection]], isomorphic to the inclusion of the [[Subset]] $\mathrm{im}(m) \subseteq Y$; so $\mathrm{Sub}(Y) \cong \mathcal{P}(Y)$, the [[Power Set]].
- In a [[C-Set|presheaf category]] a subobject of $Y$ is a sub-functor: a choice of subset $X(c) \subseteq Y(c)$ for each $c$, closed under the action of morphisms. For graphs: a subgraph.
- In a category with a [[Subobject Classifier]] $\Omega$, $\mathrm{Sub}(Y) \cong \mathcal{E}(Y, \Omega)$: subobjects are the same as [[Predicate|predicates]] on $Y$. In a [[Topos]], $\mathrm{Sub}(Y)$ is a [[Heyting Algebra]]; [[Meet|meets]] are [[Pullback|pullbacks]], [[Join|joins]] are images of [[Coproduct|coproducts]] ([[Epi-Mono Factorization]]).
- Pullback along $f : Y' \to Y$ gives a [[Monotone Map]] $f^* : \mathrm{Sub}(Y) \to \mathrm{Sub}(Y')$ (the preimage), with adjoints $\exists_f \dashv f^* \dashv \forall_f$ ([[Quantification]], [[Direct Image, Preimage, and Dual Image]]).
- Kittenlab (Lecture 14) contrasts two representations of a finite subset: as an injective `FinFunction` and as a Boolean vector $Y \to \mathbb{B}$ — the two sides of the subobject-classifier bijection.

````tabs
tab: Julia
```julia
using Catlab
Y = FinSet(5)
A = Subobject(Y, [1, 2, 4])            # a subobject of a finite set
hom(A)                                  # the mono FinFunction([1, 2, 4], 5)
B = Subobject(Y, [2, 4, 5])
A ∧ B, A ∨ B                            # Sub(Y) is a lattice (here: a Boolean algebra)
G = path_graph(Graph, 3)
H = Subobject(G, V=[1, 2], E=[1])       # a subgraph as a subobject
force(hom(H))                           # the monic ACSetTransformation H ↪ G
```
tab: Lean
```lean
import Mathlib
open CategoryTheory
#check @CategoryTheory.Subobject          -- Subobject X := quotient of MonoOver X by iso
#check @CategoryTheory.MonoOver
#check @CategoryTheory.Subobject.pullback -- f^* : Subobject Y ⥤ Subobject X
#check @CategoryTheory.Subobject.inf      -- meets via pullback (needs HasPullbacks)
```
tab: Haskell
```haskell
-- a subobject of a finite type, two ways (Kittenlab): as a list of elements or as a predicate
newtype Sub a = Sub [a]
toPred :: Eq a => Sub a -> (a -> Bool)
toPred (Sub xs) = (`elem` xs)
fromPred :: [a] -> (a -> Bool) -> Sub a
fromPred univ p = Sub (filter p univ)
```
````
