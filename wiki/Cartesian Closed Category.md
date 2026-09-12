#definition #example #theorem

A **cartesian closed category** (CCC) is a [[Category]] with all finite [[Product|products]] (including a [[Terminal Object]]) in which every pair of objects has an [[Exponential Object]] $b^a$: the [[Currying|currying]] adjunction $\mathcal{C}(e \times a, b) \cong \mathcal{C}(e, [a, b])$ holds. If it also has finite [[Coproduct|coproducts]] it is **bicartesian closed** ([[Bicartesian Closed Category]]), and products then distribute over sums.

> Sources: DaoFP §6.3 ("Bicartesian Closed Categories", "Distributivity"), §10.1 ("A category in which this adjunction holds is called cartesian closed; CCCs form the basis of all models of programming"), §10.2, §20.1 ("Self-enrichment"); 7 Sketches §7.2.1 ("Set-like properties enjoyed by any topos"), Exercise 7.11, Remark 2.81; [Bro61].

- **Examples**: $\mathbf{Set}$ (functions sets $C^B$), $\mathbf{FinSet}$, $\mathbf{Cat}$ ([[Functor Category|functor categories]]), any [[Topos]] (7 Sketches §7.2.1: "a topos is cartesian closed"), presheaf categories $[\mathcal{C}^{\mathrm{op}}, \mathbf{Set}]$ and [[C-Set|C-sets]], the category of types of a typed lambda calculus ($\mathbf{Hask}$, approximately), Heyting algebras (thin CCCs: $\wedge$ with implication $\Rightarrow$, cf. [[Bool (Monoidal Preorder)|$\mathbf{Bool}$]] as a [[Monoidal Closed Preorder]]).
- **Logic and programming** (Curry–Howard–Lambek): objects are propositions/types, products are conjunctions/pairs, exponentials are implications/function types, the terminal object is $\top$/`()`, the [[Initial Object]] (strict in a CCC) is $\bot$/`Void`. The simply typed lambda calculus is the internal language of CCCs (7 Sketches §7.4.6 "Type theories and semantics"); dependent types need [[Locally Cartesian Closed Category|locally cartesian closed categories]] (DaoFP Ch. 11).
- In a CCC, $(- \times a)$ is a left adjoint so it preserves colimits: $(b + c) \times a \cong b \times a + c \times a$ and $0 \times a \cong 0$ ([[Right Adjoints Preserve Limits]]). Every CCC is self-[[Enriched Category|enriched]] via internal homs, and every endofunctor of a CCC that is enriched is [[Functorial Strength|strong]].
- A CCC is a special [[Monoidal Closed Category]] (with $\otimes = \times$, $I = 1$); [[Compact Closed Category|compact closed categories]] are a different specialization, appropriate for linear/quantum resources rather than copyable data ([[Discard and Copy Axioms]]).

````tabs
tab: Julia
```julia
# Julia's types with tuples and functions form (approximately) a CCC: Tuple{A,B}, functions, Nothing/Union{}
# Catlab: the GAT of cartesian closed categories
using Catlab
@present CCC(FreeCartesianClosedCategory) begin
  (A, B)::Ob
  f::Hom(A ⊗ B, A)
end
curry(CCC[:A], CCC[:B], CCC[:f])     # a morphism A → hom(B, A) in the free CCC
```
tab: Lean
```lean
#check CategoryTheory.CartesianClosed     -- class: HasFiniteProducts + Exponentiable for every object
example : CategoryTheory.CartesianClosed (Type u) := inferInstance
#check CategoryTheory.ChosenFiniteProducts
#check CategoryTheory.Closed              -- monoidal closed structure on an object
```
tab: Haskell
```haskell
-- Hask is (approximately) bicartesian closed: (,) / () for products, Either / Void for sums, (->) for exponentials
-- distributivity witnessed by an isomorphism:
distribute :: (Either b c, a) -> Either (b, a) (c, a)
distribute (Left b, a)  = Left (b, a)
distribute (Right c, a) = Right (c, a)
```
````
