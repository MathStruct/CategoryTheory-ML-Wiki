#theorem #proof #example

**Proposition (CTfS 2.7.3.1).** Writing $A + B$ for the [[Coproduct]] (disjoint union), $A \times B$ for the [[Product]], $B^A$ for the set of functions $A \to B$ ([[Exponential Object]]), and $0 = \varnothing$, $1 = \{\star\}$, there are isomorphisms, for all sets $A, B, C$:

| addition | multiplication | exponentiation |
|---|---|---|
| $A + 0 \cong A$ | $A \times 0 \cong 0$, $A \times 1 \cong A$ | $A^0 \cong 1$, $A^1 \cong A$ |
| $A + B \cong B + A$ | $A \times B \cong B \times A$ | $0^A \cong 0$ (for $A \neq 0$), $1^A \cong 1$ |
| $(A + B) + C \cong A + (B + C)$ | $(A \times B) \times C \cong A \times (B \times C)$ | $A^{B + C} \cong A^B \times A^C$ |
| | $A \times (B + C) \cong (A \times B) + (A \times C)$ | $(A^B)^C \cong A^{B \times C}$ |

"One can think of the natural numbers as literally being the isomorphism classes of finite sets — that's what they are used for in counting": to count cows in a field is to build a bijection between the herd and $\underline{n}$ ([[Cardinality]]). So the laws of arithmetic of $\mathbb N$ are shadows of isomorphisms of sets; that multiplication distributes over addition "is a fact about grids of dots".

> Sources: CTfS §2.7.3 (Proposition 2.7.3.1, Exercises 2.7.3.2–2.7.3.3), §2.7.2 (Exercises 2.7.2.2, 2.7.2.6), Proposition 4.6.5.1 (the same laws for categories); DaoFP Chapters 4–6 ("tuple arithmetic", "function types", distributivity), §10.7; 7 Sketches §3.5; related: [[Categorification]], [[Bicartesian Closed Category]].

## Why these hold — universal properties, not elements

Each isomorphism can be proved by exhibiting maps both ways, but the categorical proof compares *maps out of* (or *into*) both sides, which is why the same laws hold in every [[Bicartesian Closed Category]]:

- $A^{B + C} \cong A^B \times A^C$: a map out of a sum is a pair of maps ([[Coproduct]], "if we know how economy seats and first-class seats are priced, we know how all seats are priced").
- $(A^B)^C \cong A^{B \times C}$: [[Currying]].
- $A \times (B + C) \cong A \times B + A \times C$: $(A \times -)$ is a left adjoint, so it preserves coproducts ([[Right Adjoints Preserve Limits]]; CTfS Exercise 5.1.3.4).
- $A^0 \cong 1$ (the empty function), $0^A \cong 0$ for $A \neq 0$ (no functions into $\varnothing$), $A \times 0 \cong 0$.

## The one exception: $0^0$

The table claims both $A^0 \cong 1$ for every $A$ and $0^A \cong 0$ — which conflict at $A = 0$. Going back to the definitions settles it: $\mathbf{Set}(\varnothing, \varnothing) = \{\mathrm{id}_\varnothing\}$ has exactly one element, so $0^0 = 1$ ([[CTfS Chapter 2 Exercises#Exercise 2.7.3.2|CTfS Exercise 2.7.3.2]]). This is why the hypothesis $A \neq 0$ appears in $0^A \cong 0$.

## Other consequences

- $|B^A| = |B|^{|A|}$ for finite sets, including the empty cases ([[CTfS Chapter 2 Exercises#Exercise 2.7.2.2|CTfS Exercise 2.7.2.2]]); $|\mathcal P(B)| = 2^{|B|}$ because $\mathcal P(B) \cong \{\mathit{True}, \mathit{False}\}^B$ ([[Power Set]], [[Subobject Classifier]]).
- $\mathbb R^2$ means both $\mathbb R \times \mathbb R$ and $\mathbb R^{\underline 2}$; they agree because $\underline 2 = 1 + 1$ and $\mathbb R^{1 + 1} \cong \mathbb R^1 \times \mathbb R^1 \cong \mathbb R \times \mathbb R$ ([[CTfS Chapter 2 Exercises#Exercise 2.7.2.6|CTfS Exercise 2.7.2.6]]).
- **Zero divisors**: as for numbers, $A \times B \cong \varnothing$ forces $A \cong \varnothing$ or $B \cong \varnothing$ — a pair needs both components ([[CTfS Chapter 2 Exercises#Exercise 2.7.3.3|CTfS Exercise 2.7.3.3]]).
- **Categories** (CTfS Proposition 4.6.5.1): with coproduct $+$, product $\times$ and functor categories $\mathcal B^{\mathcal A} = \mathrm{Fun}(\mathcal A, \mathcal B)$, *exactly the same* laws hold in $\mathbf{Cat}$ — "astoundingly" — e.g. $\mathrm{Fun}(\mathbf 0, \mathcal C) \cong \mathbf 1$, $\mathrm{Fun}(\mathbf 1, \mathcal C) \cong \mathcal C$, $\mathrm{Fun}(\mathcal C, \mathbf 1) \cong \mathbf 1$ ([[Functor Category]]). The same holds in any [[Topos]], e.g. for [[C-Set|database instances]].
- **Types**: in Haskell `Either`, `(,)`, `->`, `Void`, `()` satisfy the table up to isomorphism — "algebraic data types" (DaoFP).

````tabs
tab: Julia
**Docs:** [FinSets](https://algebraicjulia.github.io/Catlab.jl/v0.16/apis/categorical_algebra/#Catlab.CategoricalAlgebra.FinSets) · [Limits & colimits](https://algebraicjulia.github.io/Catlab.jl/v0.16/apis/categorical_algebra/#Catlab.CategoricalAlgebra.Limits)
```julia
using Catlab
A, B, C = FinSet(2), FinSet(3), FinSet(4)
# A × (B + C) ≅ A×B + A×C, checked on cardinalities of the (co)limits Catlab computes
length(apex(product(A, apex(coproduct(B, C))))) ==
  length(apex(coproduct(apex(product(A, B)), apex(product(A, C)))))       # 14 == 14
# |B^A| = |B|^|A|: enumerate all functions A → B as FinFunctions
functions(A, B) = [FinFunction(collect(v), B) for v in Iterators.product(fill(1:length(B), length(A))...)]
length(functions(A, B)) == length(B)^length(A)                           # 9
length(functions(FinSet(0), FinSet(0)))                                  # 1  (0⁰ = 1)
```
tab: Lean
```lean
import Mathlib
-- the laws as explicit equivalences of types
#check @Equiv.sumComm                  -- α ⊕ β ≃ β ⊕ α
#check @Equiv.prodSumDistrib           -- α × (β ⊕ γ) ≃ α × β ⊕ α × γ
#check @Equiv.sumArrowEquivProdArrow   -- (α ⊕ β → γ) ≃ (α → γ) × (β → γ)
#check @Equiv.curry                    -- (α × β → γ) ≃ (α → β → γ)
#check @Equiv.emptyArrowEquivPUnit     -- (Empty → α) ≃ PUnit      (A⁰ ≅ 1)
#check @Fintype.card_fun               -- card (α → β) = card β ^ card α
example : (0 : ℕ) ^ 0 = 1 := rfl
```
tab: Haskell
```haskell
import Data.Void (Void, absurd)

distrib :: (a, Either b c) -> Either (a, b) (a, c)     -- A × (B + C) → A×B + A×C
distrib (a, Left b)  = Left (a, b)
distrib (a, Right c) = Right (a, c)

expSum :: (Either b c -> a) -> (b -> a, c -> a)        -- A^(B+C) → A^B × A^C
expSum h = (h . Left, h . Right)

expZero :: () -> (Void -> a)                            -- 1 → A^0: the empty function
expZero () = absurd
```
````
