#definition #example #theorem

Let $(X, \mathrm{Op})$ be a [[Topological Space]] and $P : \mathrm{Op}^{\mathrm{op}} \to \mathbf{Set}$ a [[Presheaf]] on its poset of opens: $P(U)$ is the set of **sections** over $U$ and for $V \subseteq U$ the **restriction** $P(U) \to P(V)$ is written $s \mapsto s|_V$.

- Let $(U_i)_{i \in I}$ cover $U$. A **matching family** is a choice of $s_i \in P(U_i)$ for each $i$ with $s_i|_{U_i \cap U_j} = s_j|_{U_i \cap U_j}$ for all $i, j$.
- A **gluing** of $(s_i)$ is an $s \in P(U)$ with $s|_{U_i} = s_i$ for all $i$.
- $P$ satisfies the **sheaf condition** for the cover if every matching family has a *unique* gluing; $P$ is a **sheaf** if it satisfies the sheaf condition for every cover.

Morphisms of sheaves are [[Natural Transformation|natural transformations]] of the underlying presheaves; the category $\mathbf{Shv}(X, \mathrm{Op})$ of sheaves on $X$ is a [[Topos]].

> Sources: 7 Sketches §7.3 ("a sheaf on a space is roughly 'a sort of thing that can happen on the space'"), §7.3.3, Definition 7.35, Examples 7.36, 7.45, 7.46, 7.48, Exercises 7.42–7.44, 7.47, 7.49, 7.80; §7.4.1; §7.5.2.

## Remarks and examples

- **Empty cover** (Example 7.36): the empty family covers $\varnothing$, and the empty tuple is its only matching family; so a sheaf must have $P(\varnothing) = \{()\}$ — a necessary but rarely sufficient condition.
- **Sections of a function/bundle** (Example 7.45): for continuous $f : X \to Y$, $\mathrm{Sec}_f(U) = \{g : U \to X \text{ continuous} \mid g \mathbin{;} f = \mathrm{id}_U\}$ is a sheaf on $Y$; see [[Sheaf of Sections]]. Vector fields on a manifold are the sections of the tangent bundle (Example 7.46); this is *one* sheaf among a proper class of sheaves on $M$ ([[7S Exercise 7.47]]).
- **Constant sheaf** (Example 7.78): $\underline{A}(U) := A$ for a set $A$ — behaviours that never change. (Strictly, this is a sheaf on spaces whose opens are connected, like basic opens of $\mathbb{I}\mathbb{R}$.)
- **Local functions** (Example 7.79, [[7S Exercise 7.80]]): $F_X(U) = \{f : U \to X \text{ continuous}\}$ and, for a subspace $R \subseteq X$, $H_X(U) = \{f : U \cap R \to X \text{ continuous}\}$ are sheaves: continuous functions agreeing on overlaps glue.
- **Trivial covers**: if every object covers only itself, sheaves = presheaves; so presheaf categories ($\mathcal{C}$-$\mathbf{Inst}$, [[C-Set|C-sets]], [[Category of Graphs|graphs]]) count as sheaf toposes (footnote 10). $\mathbf{Set} = \mathbf{Shv}(\{*\})$ (Example 7.48, [[7S Exercise 7.52]]).
- The [[Subobject Classifier]] of $\mathbf{Shv}(X)$ is the sheaf $\Omega(U) = \{U' \in \mathrm{Op} \mid U' \subseteq U\}$; sheaves on the [[Interval Domain]] are [[Topos of Behavior Types|behavior types]].
- On the [[Sierpinski Space]], a sheaf is just a function ([[7S Exercise 7.49]]).

````tabs
tab: Julia
```julia
# the sheaf of sections of a finite function f : X → Y on the discrete space Y (Example 7.45)
X = ["a1","a2","b1","b2","b3","c1","e1","e2"]
f = Dict("a1"=>"a","a2"=>"a","b1"=>"b","b2"=>"b","b3"=>"b","c1"=>"c","e1"=>"e","e2"=>"e")
fiber(y) = [x for x in X if f[x] == y]
sections(U) = [Dict(zip(U, c)) for c in Iterators.product((fiber(y) for y in U)...)]   # Sec_f(U)
restrict(s, V) = Dict(v => s[v] for v in V)
length(sections(["a","b"]))          # 6 sections, Eq. (7.39)
length(sections(["a","b","c","d"]))  # 0: the fiber over d is empty
# gluing: two sections over U1, U2 agreeing on the overlap glue uniquely over U1 ∪ U2
s1 = Dict("a"=>"a1","b"=>"b2"); s2 = Dict("b"=>"b2","e"=>"e1")
restrict(s1, ["b"]) == restrict(s2, ["b"])   # matching family
merge(s1, s2)                                # the glued section
```
tab: Lean
```lean
import Mathlib
#check @TopCat.Presheaf                        -- (Opens X)ᵒᵖ ⥤ C
#check @TopCat.Presheaf.IsSheaf
#check @TopCat.Sheaf                           -- the category Shv(X)
#check @TopCat.Presheaf.isSheaf_iff_isSheafUniqueGluing   -- matching families glue uniquely
#check @TopCat.Presheaf.IsCompatible          -- matching family
#check @TopCat.Presheaf.IsGluing
#check @CategoryTheory.Sheaf                   -- sheaves for a Grothendieck topology J on a site
```
tab: Haskell
```haskell
-- a presheaf on a finite poset of opens, with a brute-force sheaf check on one cover
import qualified Data.Map as M
data Presheaf u s = Presheaf { sections :: u -> [s], restrict :: u -> u -> s -> s }
-- unique gluing of a matching family (s_i) over a cover (u_i) of u
sheafCond :: (Eq s) => Presheaf u s -> u -> [u] -> [s] -> Bool
sheafCond p u cover fam = length [ s | s <- sections p u, and (zipWith (\ui si -> restrict p u ui s == si) cover fam) ] == 1
```
````
