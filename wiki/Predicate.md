#definition #example

In a [[Topos]] $\mathcal{E}$ a **predicate** on an object $S$ is a morphism $p : S \to \Omega$ into the [[Subobject Classifier]]. Equivalently (by the classifier property) it is a [[Subobject]] $\{S \mid p\} \rightarrowtail S$, "the elements of $S$ for which $p$ holds". A predicate on the [[Terminal Object]] $1$ is a **proposition**; the set of propositions is written $|\Omega| = \mathcal{E}(1, \Omega)$.

> Sources: 7 Sketches §7.2.2 ("A predicate on $Y$ is a morphism $Y \to \Omega$"), §7.4.3 ("Predicates", "The poset of subobjects"), Eq. (7.63), Exercises 7.62, 7.64.

- **In $\mathbf{Set}$**: $p : S \to \mathbb{B}$ is a Boolean-valued function, e.g. `likes_cats : People → 𝔹`, and $\{S \mid p\}$ is the subset of people who like cats. Logical operations on predicates are set operations on subsets: AND is intersection.
- **In $\mathbf{Shv}(X)$**: a predicate $p : S \to \Omega$ gives for each open $U$ a function $p(U) : S(U) \to \Omega(U)$; applied to a section $s \in S(U)$ it returns an *open subset* $p(s) \subseteq U$ — the region where $p$ holds of $s$. If $S$ is the sheaf of people over time and $p$ = "likes the weather", $p(\mathrm{Bob})$ is the set of times at which Bob likes the weather: "in summers yes, in April 2018 yes, otherwise no". The subsheaf $\{S \mid p\}$ has as sections over $U$ the people alive throughout $U$ who like the weather throughout $U$ ([[7S Chapter 7 Exercises#Exercise 7.62|7S Exercise 7.62]]).
- **The poset of predicates** $(|\Omega^S|, \leq^S)$, Eq. (7.63): $p \leq q$ iff $p(s) \subseteq q(s)$ for every $U$ and $s \in S(U)$ — "$p$ implies $q$", written $p(s) \vdash_{s : S} q(s)$. It is a [[Partial Order]] (since $U \subseteq V \subseteq U$ forces $U = V$) and in fact a [[Heyting Algebra]]: $\wedge, \vee, \Rightarrow, \neg, \mathsf{true}, \mathsf{false}$ are computed pointwise ([[Internal Logic of a Topos]]). [[Quantification]] turns a predicate on $S \times T$ into one on $S$.

````tabs
tab: Julia
```julia
using Catlab
# predicates on a finite set as Bool vectors; the poset of predicates is Sub(S)
S = FinSet(6)
p = Subobject(S, [2, 4, 6])          # "even"
q = Subobject(S, [2, 3, 5])          # "prime"
collect(hom(p ∧ q)), collect(hom(p ∨ q))    # [2], [2, 3, 4, 5, 6]
# the same predicates as Bool vectors S → 𝔹, ordered pointwise
pv = [iseven(n) for n in 1:6]; qv = [n in (2, 3, 5) for n in 1:6]
all(pv .<= qv)                       # false: "even" does not entail "prime"
```
tab: Lean
```lean
import Mathlib
-- in Type, predicates S → Prop are the same as Set S, ordered by implication
example (S : Type) (p q : S → Prop) : (∀ s, p s → q s) ↔ ({s | p s} ⊆ {s | q s}) := Iff.rfl
#check @CategoryTheory.Subobject.instPartialOrder
```
tab: Haskell
```haskell
type Pred s = s -> Bool
implies :: Pred s -> Pred s -> Pred s          -- pointwise Heyting implication on Bool
implies p q s = not (p s) || q s
entails :: [s] -> Pred s -> Pred s -> Bool     -- p ⊢ q on a finite carrier
entails univ p q = all (\s -> not (p s) || q s) univ
```
````
