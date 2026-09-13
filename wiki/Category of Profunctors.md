#definition #theorem #proof

**Theorem 4.23.** For any skeletal [[Quantale]] $\mathcal{V}$ there is a category $\mathbf{Prof}_{\mathcal{V}}$ whose objects are $\mathcal{V}$-[[Enriched Category|categories]], whose morphisms $\mathcal{X} \nrightarrow \mathcal{Y}$ are $\mathcal{V}$-[[Profunctor|profunctors]], with composition

$$
(\Phi \mathbin{;} \Psi)(p, r) := \bigvee_{q \in \mathcal{Q}} \Phi(p, q) \otimes \Psi(q, r)
$$

(Definition 4.21 — [[Matrix Multiplication in a Quantale]]) and identities the **unit profunctors** $U_{\mathcal{X}}(x, y) := \mathcal{X}(x, y)$ (Eq. 4.25). **$\mathbf{Feas} := \mathbf{Prof}_{\mathbf{Bool}}$** is the category of preorders and [[Feasibility Relation|feasibility relations]] (Definition 4.24).

> Sources: 7 Sketches §4.3 (Definitions 4.21, 4.24, Theorem 4.23, Eq. 4.25, Lemmas 4.27, 4.31, Remark 4.33, Exercises 4.22, 4.26, 4.30, 4.32), §4.5.2 (Theorem 4.63); DaoFP §17.2, §17.8 ("The Bicategory of Profunctors").

## Composition as navigation (Eq. 4.19–4.20)

Given bridges $\Phi : P \nrightarrow Q$ and $\Psi : Q \nrightarrow R$ between three cities, the composite feasibility matrix is found by a *navigator*: for each $p, r$, search $Q$ for a way-point $q$ reachable from $p$ AND from which $r$ is reachable — "a big OR over all possible $q$": $(\Phi \mathbin{;} \Psi)(p, r) = \bigvee_q \Phi(p, q) \wedge \Psi(q, r)$. In the example, you cannot get from $N$ to $x$ but you can from $N$ to $y$. For [[Cost]]: min-plus products of distance matrices ([[7S Chapter 4 Exercises#Exercise 4.22|7S Exercise 4.22]]).

## Unitality (Lemma 4.27)

$U_P \mathbin{;} \Phi = \Phi = \Phi \mathbin{;} U_Q$. *Proof.* Skeletality lets us prove equality by two inequalities. (i) $\Phi(p,q) = I \otimes \Phi(p, q) \leq P(p, p) \otimes \Phi(p, q) \leq \bigvee_{p_1} P(p, p_1) \otimes \Phi(p_1, q) = (U_P \mathbin{;} \Phi)(p, q)$ — unit law, $I \leq P(p,p)$ with monotonicity of $\otimes$, the join bound, and the definition (Eq. 4.28). (ii) For each $p_1$, $P(p, p_1) \otimes \Phi(p_1, q) = P(p, p_1) \otimes \Phi(p_1, q) \otimes I \leq P(p, p_1) \otimes \Phi(p_1, q) \otimes Q(q, q) \leq \Phi(p, q)$ by the profunctor inequality of [[7S Chapter 4 Exercises#Exercise 4.9|7S Exercise 4.9]] (Eq. 4.29); hence the join is $\leq \Phi(p, q)$. $\blacksquare$ ([[7S Chapter 4 Exercises#Exercise 4.30|7S Exercise 4.30]]; in $\mathbf{Bool}$ all four steps are equalities.) Associativity (Lemma 4.31, [[7S Chapter 4 Exercises#Exercise 4.32|7S Exercise 4.32]]) uses distributivity of $\otimes$ over joins and skeletality.

**Why skeletal?** Without skeletality the unit laws hold only up to $\cong$; one can either take isomorphism classes of profunctors (as for [[Cospan|cospans]]) or accept composition "up to isomorphism" — a [[2-Category|bicategory]] (Remark 4.33; DaoFP §17.8: monads in $\mathbf{Prof}$ are [[Prearrow|prearrows]]).

## Structure

- Every [[Enriched Functor|$\mathcal{V}$-functor]] embeds via its [[Companion and Conjoint|companion or conjoint]]; the identity functor's companion is $U_{\mathcal{X}}$ (Example 4.35, [[7S Chapter 4 Exercises#Exercise 4.36|7S Exercise 4.36]]).
- **Compact closed** (Theorem 4.63): monoidal product is the [[Product of Enriched Categories|product of $\mathcal{V}$-categories]] with $(\Phi \times \Psi)((x_1, y_1), (x_2, y_2)) = \Phi(x_1, x_2) \otimes \Psi(y_1, y_2)$ ("stacking wires with no new interaction"; for feasibility: provide both given both, [[7S Chapter 4 Exercises#Exercise 4.64|7S Exercise 4.64]]); the unit is the one-object $\mathcal{V}$-category $\mathbf{1}$ with $\mathbf{1}(1,1) = I$ ([[7S Chapter 4 Exercises#Exercise 4.65|7S Exercise 4.65]]: the unitors are the profunctors $\mathcal{X} \times \mathbf{1} \nrightarrow \mathcal{X}$ given by $\mathcal{X}(x, x')$); the dual of $\mathcal{X}$ is $\mathcal{X}^{\mathrm{op}}$ with unit $\eta_{\mathcal{X}} : \mathbf{1} \nrightarrow \mathcal{X}^{\mathrm{op}} \times \mathcal{X}$, $\eta(1, x, x') = \mathcal{X}(x, x')$, and counit $\varepsilon_{\mathcal{X}} : \mathcal{X} \times \mathcal{X}^{\mathrm{op}} \nrightarrow \mathbf{1}$, $\varepsilon(x, x', 1) = \mathcal{X}(x, x')$ — "the unit and counit look like identities" ([[7S Chapter 4 Exercises#Exercise 4.66|7S Exercise 4.66]] checks the snake equations). See [[Compact Closed Category]].
- In wiring-diagram terms, boxes are feasibility relations with one input and one output wire (a plain category) — but $\mathbf{Feas}$'s monoidal and compact structure allows the rich [[Co-design]] diagrams with many ports and feedback.

````tabs
tab: Julia
```julia
# Feas: compose Bool-profunctors (feasibility matrices) by Bool matrix multiplication (Eq. 4.20)
Φ = Bool[1 0 1 0 0; 1 0 1 0 1; 1 1 1 1 0; 1 1 1 1 1]      # N, E, W, S × a..e
Ψ = Bool[0 1; 1 1; 0 1; 1 1; 0 0]                        # a..e × x, y
qmul(BoolPre(), Φ, Ψ)      # [0 1; 0 1; 1 1; 1 1]: you can't get from N to x but you can to y
unit_profunctor(X::VCategory) = VProfunctor(X, X, X.hom)   # U_X(x, y) = X(x, y)
```
tab: Lean
```lean
-- Mathlib has no bundled Prof_V; for V = Bool on preorders one can model composition as
-- relational composition of monotone relations (see `Rel.comp`)
#check @Rel.comp
```
tab: Haskell
```haskell
-- Feas: objects are preorders, morphisms feasibility relations, composition by "search for a way-point"
composeFeas :: [q] -> Feas p q -> Feas q r -> Feas p r
composeFeas qs (Feas phi) (Feas psi) = Feas (\p r -> or [ phi p q && psi q r | q <- qs ])

unitFeas :: Preorder x => Feas x x
unitFeas = Feas leq                      -- U_X(x, y) = X(x, y)
```
````
