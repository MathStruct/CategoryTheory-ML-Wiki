#definition #example #theorem

A **symmetric monoidal structure** on a [[Category]] $\mathcal{C}$ consists of

(i) an object $I \in \mathrm{Ob}(\mathcal{C})$, the **monoidal unit**, and
(ii) a [[Functor]] $\otimes : \mathcal{C} \times \mathcal{C} \to \mathcal{C}$, the **monoidal product** (a [[Bifunctor]]: $f \otimes g$ for morphisms too),

together with well-behaved [[Natural Isomorphism|natural isomorphisms]]

(a) **left unitor** $\lambda_c : I \otimes c \cong c$,
(b) **right unitor** $\rho_c : c \otimes I \cong c$,
(c) **associator** $\alpha_{c,d,e} : (c \otimes d) \otimes e \cong c \otimes (d \otimes e)$,
(d) **swap** (braiding, symmetry) $\sigma_{c,d} : c \otimes d \cong d \otimes c$ with $\sigma \circ \sigma = \mathrm{id}$,

satisfying coherence laws (the pentagon and triangle equations, hidden under "well behaved" in 7 Sketches' *Rough Definition 4.45*). Without (d) one has a **monoidal category**; a **symmetric monoidal category** (SMC) has all four. If (a)–(c) are equalities the structure is **strict**; by **Mac Lane's coherence theorem** every monoidal category is equivalent to a strict one (Remark 4.46–4.47: "a symmetric monoidal category is a category equipped with an equivalence to a symmetric strict monoidal category"), so one may pretend strictness — as wiring diagrams implicitly do.

> Sources: 7 Sketches §4.4.3 (Definition 4.45, Remarks 4.46–4.47, Examples 4.49, Exercises 4.48, 4.50), §4.4.4, §5–6; DaoFP §4.4 ("Symmetric Monoidal Category" from sums), §5.3 ("Monoidal Category", "Monoids"), §14.9 (monoidal functors), §15.1 (string diagrams), §17.7 ([[Day Convolution]]), §19.1, §20.1 (enrichment); Kittenlab (implicitly: $\mathbf{FinSet}$ with $+$).

A [[Symmetric Monoidal Preorder]] is exactly a symmetric monoidal category with at most one morphism between any two objects ([[7S Chapter 4 Exercises#Exercise 4.48|7S Exercise 4.48]]) — monoidal categories are the [[Categorification|categorification]] of monoidal preorders: equations become isomorphisms ("bookkeeping") which in turn must satisfy new equations.

## Examples

| category | $\otimes$ | $I$ | notes |
|---|---|---|---|
| $\mathbf{Set}$ | $\times$ (cartesian product; $f \times g$ pointwise) | $\{1\}$ | Example 4.49; $\alpha : (s, (t, u)) \mapsto ((s, t), u)$; a [[Cartesian Category]] |
| $\mathbf{Set}$, $\mathbf{FinSet}$ | $\sqcup$ / $+$ | $\varnothing$ | cocartesian (DaoFP §4.4: $0 + a \cong a$, commutativity, associativity, functoriality); the [[Prop]] $\mathbf{FinSet}$ |
| any category with finite [[Product|products]] / [[Coproduct|coproducts]] | $\times$ / $+$ | $1$ / $0$ | DaoFP Chapters 4–5; "tuple arithmetic" |
| $\mathbf{Vect}_k$ | $\otimes_k$ | $k$ | not cartesian: no diagonal $V \to V \otimes V$ — no copying (quantum) |
| $[\mathcal{C}, \mathcal{C}]$ endofunctors | $\circ$ | $\mathrm{Id}$ | strict, *not* symmetric; its monoids are [[Monad|monads]] (DaoFP §14.7) |
| $\mathbf{Cat}$ | $\times$ | $\underline{\mathbf{1}}$ | cartesian closed (DaoFP §10.1) |
| [[Category of Profunctors|$\mathbf{Prof}_{\mathcal{V}}$]] / $\mathbf{Feas}$ | product of $\mathcal{V}$-categories | $\mathbf{1}$ | [[Compact Closed Category|compact closed]] (Theorem 4.63) |
| [[Corelation|$\mathbf{Corel}$]], [[Cospan|$\mathbf{Cosp}_{\mathcal{C}}$]] | $\sqcup$ | $\varnothing$ | compact closed / [[Hypergraph Category|hypergraph]] (Chapter 6) |
| [[Prop|Props]], $\mathbf{Mat}(R)$ | $+$ on $\mathbb{N}$ | $0$ | strict SMCs with $\mathrm{Ob} = \mathbb{N}$ (Chapter 5) |
| $[\mathcal{C}, \mathbf{Set}]$ | [[Day Convolution]] | $\mathcal{C}(I, -)$ | DaoFP §17.7 |

## Wiring diagrams and interpretation

An SMC is "an algebraic structure with labelled boxes having multiple typed inputs and outputs" (§4.4.2); series composition is $\mathbin{;}$, parallel composition is $\otimes$, crossing wires is $\sigma$, and coherence lets diagrams be read unambiguously ([[Wiring Diagram]], [[String Diagram]]). [[7S Chapter 4 Exercises#Exercise 4.50|7S Exercise 4.50]] evaluates a diagram of functions in $(\mathbf{Set}, 1, \times)$ as a single function $A \times B \to G \times F$. DaoFP: "if we think of morphisms as actions, their tensor product corresponds to performing two actions in parallel", and "a tensor product is the lowest common denominator of product and sum: it has an introduction rule requiring both objects but no elimination rule — once created it forgets how it was created; unlike a cartesian product it has no projections". [[Discard and Copy Axioms|Copying and discarding]] are extra structure ([[Cartesian Category|cartesian]] = every object a cocommutative comonoid).

## Structures on / in monoidal categories

- [[Monoid Object|Monoids]] in a monoidal category ($\mu : m \otimes m \to m$, $\eta : I \to m$, DaoFP §5.3, 7 Sketches §5.4.2), comonoids, [[Frobenius Monoid|Frobenius monoids]] (§6.3.1), [[Hopf Algebra|bialgebras]].
- [[Monoidal Functor|Monoidal functors]] (lax/strong/strict; Definition 6.68, DaoFP §14.9), [[Monoidal Natural Transformation|monoidal natural transformations]].
- [[Enriched Category|Enrichment]] in an SMC (Rough Definition 4.51): hom-objects $\mathcal{X}(x, y) \in \mathcal{V}$, identity $\mathrm{id}_x : I \to \mathcal{X}(x,x)$, composition $\mathcal{X}(x,y) \otimes \mathcal{X}(y,z) \to \mathcal{X}(x,z)$; $\mathbf{Set}$-categories are categories ([[7S Chapter 4 Exercises#Exercise 4.52|7S Exercise 4.52]]), $\mathbf{Cost}$-categories have identity elements $0 \geq d(x, x)$ ([[7S Chapter 4 Exercises#Exercise 4.54|7S Exercise 4.54]]).
- Closed structures: [[Monoidal Closed Category]] ($[c, d]$ with $\mathcal{C}(b \otimes c, d) \cong \mathcal{C}(b, [c, d])$), [[Compact Closed Category]] (duals), [[Cartesian Closed Category]]; [[Hypergraph Category]]; [[Traced Monoidal Category|traced]] categories.
- [[Operad|Operads]] arise from SMCs by taking multi-input morphisms (§6.5.2).

````tabs
tab: Julia
```julia
# Catlab: the GAT of symmetric monoidal categories and free SMC expressions
using Catlab
@present P(FreeSymmetricMonoidalCategory) begin
  (A, B, C)::Ob
  f::Hom(A, B); g::Hom(B ⊗ C, C)
end
f, g = P[:f], P[:g]
(f ⊗ id(P[:C])) ⋅ g                   # series and parallel composition: A ⊗ C → C
braid(P[:A], P[:B])                   # the swap σ_{A,B}
munit(FreeSymmetricMonoidalCategory.Ob)   # I

# FinSet with + is a symmetric monoidal category (a prop):
f = FinFunction([2, 1], 2); g = FinFunction([1, 1, 3], 3)
oplus(f, g)                           # f ⊕ g : 5 → 5
# and with ×:
otimes(f, g)                          # f × g : 6 → 6
```
tab: Lean
```lean
#check CategoryTheory.MonoidalCategory     -- class: tensorObj, whiskerLeft/Right, tensorUnit, associator, unitors, pentagon, triangle
#check CategoryTheory.SymmetricCategory    -- braiding with symmetry
#check CategoryTheory.BraidedCategory
example : CategoryTheory.MonoidalCategory (Type u) := inferInstance   -- (Type, ×, PUnit)
#check CategoryTheory.MonoidalCategory.associator
#check CategoryTheory.MonoidalCategory.pentagon
```
tab: Haskell
```haskell
-- DaoFP §5.3: Hask with (,) and () is symmetric monoidal (up to isomorphism)
assoc :: ((a, b), c) -> (a, (b, c))
assoc ((a, b), c) = (a, (b, c))
lunit :: ((), a) -> a
lunit ((), a) = a
runit :: (a, ()) -> a
runit (a, ()) = a
swap :: (a, b) -> (b, a)
swap (a, b) = (b, a)
-- functoriality of the tensor: f ⊗ g
tensor :: (a -> a') -> (b -> b') -> (a, b) -> (a', b')
tensor f g (a, b) = (f a, g b)
-- Either / Void give a second symmetric monoidal structure
```
````
