#definition #example #annotation

Every [[Topos]] $\mathcal{E}$ has an **internal language**: a formal higher-order (intuitionistic, typed) logic whose types are objects of $\mathcal{E}$, whose terms are morphisms, whose formulas are [[Predicate|predicates]] $S \to \Omega$, with connectives from the [[Internal Logic of a Topos|Heyting structure of $\Omega$]] and quantifiers from [[Quantification]]. Its **semantics** — a "logic-to-sheaf compiler" — is called *categorical* or *Kripke–Joyal semantics*: each formal statement is converted into a statement about particular sheaves. The semantics is *sound*: every formal proof becomes a true fact about the sheaves.

> Sources: 7 Sketches §7.4.6 ("Type theories and semantics"), Eq. (7.73), Example 7.74, §7.1, §7.6 ([MM92], [Jac99], [LS88]); DaoFP §11 (dependent types), §6.3 (lambda calculus in a CCC).

**Example 7.74.** For any $f : S \to T$ in a topos, $f$ is an [[Epimorphism]] iff the formula
$$\forall(t : T).\, \exists(s : S).\, f(s) = t \tag{7.73}$$
holds. In the topos of [[C-Set|database instances]] on a schema $\mathcal{C}$ this compiles to "for every table $c$ and every row $t \in T(c)$ there is a row $s \in S(c)$ with $f(s) = t$" — surjectivity table by table. In $\mathbf{Shv}(X)$ it compiles to "for every open $U$ and section $t \in T(U)$ there is an open cover $(U_i)$ of $U$ and sections $s_i \in S(U_i)$ with $f(s_i) = t|_{U_i}$" — local surjectivity, because $\exists$ takes covers into account.

- Logic (syntax: expressions and deductions by strict rules) and semantics (meaning in sheaves) are separate: "a computer can carry out logical deductions without knowing what any of them mean about sheaves".
- The simply typed lambda calculus is the internal language of a [[Cartesian Closed Category]]; dependent type theory that of a [[Locally Cartesian Closed Category]]; the internal language of a topos adds $\Omega$, i.e. power objects and a full higher-order logic.
- In the [[Topos of Behavior Types]] the internal language becomes a [[Temporal Logic]]: "what we call graphs will actually be graphs that change through time".

````tabs
tab: Julia
```julia
using Catlab
# Example 7.74 in the presheaf topos of graphs: f is epi iff surjective on every table (V and E)
G = path_graph(Graph, 2); H = cycle_graph(Graph, 1)
f = ACSetTransformation(G, H; V=[1, 1], E=[1])
is_epic(f)                                   # true: ∀t ∃s f(s) = t holds tablewise
```
tab: Lean
```lean
import Mathlib
open CategoryTheory
-- in Type the internal statement (7.73) is ordinary surjectivity, and epi ↔ surjective
#check @CategoryTheory.epi_iff_surjective
#check @CategoryTheory.mono_iff_injective
```
tab: Haskell
```haskell
-- the formula ∀t. ∃s. f s = t evaluated on finite types
isEpiFinite :: Eq t => [s] -> [t] -> (s -> t) -> Bool
isEpiFinite ss ts f = all (\t -> any (\s -> f s == t) ss) ts
```
````
