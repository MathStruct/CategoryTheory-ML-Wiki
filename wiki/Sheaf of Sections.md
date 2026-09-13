#example #definition #program

Let $f : X \to Y$ be a function between finite sets (Eq. 7.37: $X = \{a_1, a_2, b_1, b_2, b_3, c_1, e_1, e_2\}$, $Y = \{a, b, c, d, e\}$, $f$ sends each element to the letter below it), regarded as a continuous map of discrete [[Topological Space|spaces]]. The presheaf $\mathrm{Sec}_f : \mathrm{Op}(Y)^{\mathrm{op}} \to \mathbf{Set}$,

$$
\mathrm{Sec}_f(U) := \{s : U \to X \mid (s \mathbin{;} f)(u) = u \text{ for all } u \in U\},
$$

assigns to $U \subseteq Y$ the set of *cross-sections* over $U$: one element of each [[Fiber]] over $U$. Restriction along $V \subseteq U$ restricts the function $s$ to $V$. It is a [[Sheaf]]: sections over $U_1$ and $U_2$ that agree on $U_1 \cap U_2$ glue uniquely to a section over $U_1 \cup U_2$.

> Sources: 7 Sketches §7.3.3 ("Extended example: sections of a function"), Eqs. (7.37)–(7.43), Exercises 7.38, 7.40, 7.42, 7.44; Example 7.45 (continuous case), Example 7.46 (vector fields), Example 7.61 (vector bundles).

- $|\mathrm{Sec}_f(\{a, b\})| = 2 \cdot 3 = 6$ (Eq. 7.39); $|\mathrm{Sec}_f(\{a,b,c\})| = 6$, $\mathrm{Sec}_f(\{a,b,c,d\}) = \varnothing$ because the fiber over $d$ is empty, $|\mathrm{Sec}_f(\{a,b,d,e\})| = 0$ likewise ([[7S Chapter 7 Exercises#Exercise 7.40|7S Exercise 7.40]]). In general $|\mathrm{Sec}_f(U)| = \prod_{u \in U} |f^{-1}(u)|$.
- Restriction $\mathrm{Sec}_f(\{a,b,c\}) \to \mathrm{Sec}_f(\{a,c\})$ forgets the $b$-component; it is $3$-to-$1$ ([[7S Chapter 7 Exercises#Exercise 7.42|7S Exercise 7.42]]).
- Non-matching pairs such as $(a_1, b_1)$ over $\{a,b\}$ and $(b_2, e_1)$ over $\{b, e\}$ have no gluing ([[7S Chapter 7 Exercises#Exercise 7.44|7S Exercise 7.44]]).
- **General case** (Example 7.45): for any continuous $f : X \to Y$, $\mathrm{Sec}_f(U) = \{g : U \to X \text{ continuous} \mid g \mathbin{;} f = \mathrm{id}_U\}$ is a sheaf on $Y$. For the tangent bundle $\pi : TM \to M$ of a manifold, $\mathrm{Sec}_\pi$ is the sheaf of **vector fields** (wind velocities on Earth: fields on Afghanistan and Pakistan agreeing at the border glue). The hairy ball theorem — every global vector field on the sphere vanishes somewhere, although local ones need not — is a [[Generative Effect]] between local and global sections, measured by cohomology.
- Sections of a [[Fiber|bundle]] are the semantic counterpart of a [[Dependent Product]] (DaoFP §11): $\mathrm{Sec}_f(Y) = \prod_{y \in Y} f^{-1}(y)$.

````tabs
tab: Julia
```julia
X = ["a1","a2","b1","b2","b3","c1","e1","e2"]
f = Dict(x => string(x[1]) for x in X)                  # a1 ↦ a, …
fiber(y) = [x for x in X if f[x] == y]
Sec(U) = [Dict(zip(U, c)) for c in Iterators.product((fiber(y) for y in U)...)] |> vec
length(Sec(["a","b","c"]))       # 6
length(Sec(["a","b","c","d"]))   # 0
restrict(s, V) = Dict(v => s[v] for v in V)
unique(restrict.(Sec(["a","b","c"]), Ref(["a","c"])))   # the 2 sections over {a,c}
```
tab: Lean
```lean
import Mathlib
-- sections of a bundle as dependent functions: Sec_f(U) ≃ Π u : U, fiber f u
example {X Y : Type} (f : X → Y) (U : Set Y) : Type :=
  { s : U → X // ∀ u, f (s u) = u }
#check @TopCat.Presheaf                        -- the general presheaf/sheaf machinery
```
tab: Haskell
```haskell
-- Sec_f(U) for a finite function given by its fibers
sections :: Eq y => [x] -> (x -> y) -> [y] -> [[(y, x)]]
sections xs f = mapM (\y -> [ (y, x) | x <- xs, f x == y ])
-- length (sections xs f ["a","b"]) == 6 for the example of Eq. (7.37)
```
````
