#definition #example

For a category $\mathcal I$, the **left cone** $\mathcal I^{\triangleleft}$ is obtained by adjoining one new object $-\infty$, the **cone point**, with exactly one morphism $-\infty \to i$ for every object $i$ of $\mathcal I$ (and $\mathrm{Hom}(-\infty, -\infty) = \{\mathrm{id}\}$, nothing into $-\infty$ from $\mathcal I$). Composites are forced: $-\infty \to i \xrightarrow{f} j$ is the unique arrow $-\infty \to j$. So $-\infty$ is an [[Initial Object]] of $\mathcal I^{\triangleleft}$. Dually the **right cone** $\mathcal I^{\triangleright}$ adjoins a terminal object $\infty$.

> Sources: CTfS §4.5.2 (Definition 4.5.2.6, Remark 4.5.2.7, Examples 4.5.2.8, 4.5.2.12, Exercises 4.5.2.9–4.5.2.10, Definition 4.5.2.11), §4.5.3 (Definition 4.5.3.18, Exercise 4.5.3.19, Examples 4.5.3.21, 4.5.3.28); 7 Sketches §3.5.2 (cones).

## Why: cones are functors out of cones

For a diagram $X : \mathcal I \to \mathcal C$, a [[Cone]] over $X$ is exactly a functor $\mathcal I^{\triangleleft} \to \mathcal C$ that restricts to $X$ on $\mathcal I$: the cone point goes to the apex, the arrows $-\infty \to i$ go to the legs, and functoriality is the commutativity of every triangle. Cones over $X$ form a category $\mathcal C_{/X}$ and the [[Limit]] of $X$ is its terminal object (CTfS Definition 4.5.3.18); cocones and [[Colimit|colimits]] use $\mathcal I^{\triangleright}$ and initial objects. The shape of a limit problem is thus $\mathcal I \hookrightarrow \mathcal I^{\triangleleft}$: "given the bottom, find the best top".

## Examples

- **Products** (CTfS Example 4.5.2.8): for the discrete category $\underline n$, the left cone is $\mathrm{Star}_n$ — a centre with $n$ spokes. Functors $\mathrm{Star}_n \to \mathcal C$ are $n$-legged spans, and the limit is the $n$-ary [[Product]] (CTfS Example 4.5.3.21). For $n = 0$ the cone is the one-object category and the limit is the [[Terminal Object]].
- **Pullbacks**: the cospan $\bullet \to \bullet \leftarrow \bullet$ plus a cone point is the commutative square — the shape of a [[Pullback]].
- **Iterated cones** ([[CTfS Chapter 4 Exercises#Exercise 4.5.2.9|CTfS Exercise 4.5.2.9]]): $\varnothing^{\triangleleft} = [0]$, $[0]^{\triangleleft} = [1]$ (the [[Walking Arrow]]), and in general $[n]^{\triangleleft} \cong [n+1]$; the $(n+1)$-fold cone on the empty category is the linear order $[n]$ ([[Simplex Category]]).
- **Squares** (CTfS Example 4.5.2.12): $(\underline 2^{\triangleleft})^{\triangleright}$ is the commutative square $[1] \times [1]$ — a span closed off by a cospan.
- **Graphs** ([[CTfS Chapter 4 Exercises#Exercise 4.5.2.10|CTfS Exercise 4.5.2.10]], [[CTfS Chapter 4 Exercises#Exercise 4.5.3.19|4.5.3.19]]): for the graph-indexing category $A \rightrightarrows V$, the left cone has arrows $a : -\infty \to A$ and $v : -\infty \to V$ with $a \,\mathbin{;}\, \mathrm{src} = v = a \,\mathbin{;}\, \mathrm{tgt}$. A cone over a graph in $\mathbf{Set}$ is a set $Z$ with $Z \to A$ landing in arrows whose source equals their target; the limit is the set of **loops** — the [[Equalizer]] of $\mathrm{src}, \mathrm{tgt}$ — and the colimit is the set of connected components.

## As a pushout

The cone is itself a colimit in $\mathbf{Cat}$ (CTfS Example 4.5.3.28):

$$
\mathcal I^{\triangleleft} \;\cong\; \mathbf 1 \sqcup_{\mathcal I} (\mathcal I \times [1]),
$$

the [[Pushout]] that takes the cylinder $\mathcal I \times [1]$ and collapses the end $\mathcal I \times \{0\}$ to a point — exactly the topologist's cone on a space (compare CTfS Exercise 4.5.3.29, $D^2 \sqcup_{S^1} D^2 \cong S^2$).

````tabs
tab: Julia
**Docs:** [FinSets](https://algebraicjulia.github.io/Catlab.jl/v0.16/apis/categorical_algebra/#Catlab.CategoricalAlgebra.FinSets) · [Limits & colimits](https://algebraicjulia.github.io/Catlab.jl/v0.16/apis/categorical_algebra/#Catlab.CategoricalAlgebra.Limits) · [ACSets API](https://algebraicjulia.github.io/ACSets.jl/stable/api/) · [Graphs](https://algebraicjulia.github.io/Catlab.jl/v0.16/apis/graphs/) · [ThCategory (GATlab)](https://algebraicjulia.github.io/GATlab.jl/stable/stdlib/#GATlab.Stdlib.StdTheories.ThCategory)
```julia
using Catlab
# The left cone on the graph-indexing category A ⇉ V (CTfS Exercise 4.5.2.10)
@present SchGraphCone(FreeCategory) begin
  (Apex, A, V)::Ob
  (src, tgt)::Hom(A, V)
  a::Hom(Apex, A); v::Hom(Apex, V)
  a ⋅ src == v; a ⋅ tgt == v            # every triangle commutes
end
# The limit of a graph (as a diagram A ⇉ V in FinSet) is its set of loops: an equalizer
G = @acset Graph begin V = 3; E = 4; src = [1, 1, 2, 3]; tgt = [2, 1, 3, 3] end
loops = equalizer(FinFunction(G[:src], nv(G)), FinFunction(G[:tgt], nv(G)))
collect(incl(loops))                     # [2, 4]: the loops at vertices 1 and 3
# the colimit is the set of connected components: a coequalizer
length(apex(coequalizer(FinFunction(G[:src], nv(G)), FinFunction(G[:tgt], nv(G)))))   # 1
```
tab: Lean
```lean
import Mathlib
open CategoryTheory Limits
-- Mathlib's cone point: `WithInitial J` adjoins an initial object, `WithTerminal J` a terminal one
#check @WithInitial               -- J◁
#check @WithTerminal              -- J▷
#check @Cone                      -- a cone over F : J ⥤ C, equivalently a functor out of WithInitial J
#check @IsLimit                   -- a terminal cone
```
tab: Haskell
```haskell
-- a finite category given by objects and hom-sets; the left cone adds a new initial object
data Obj o = Bottom | Old o deriving (Eq, Show)   -- Bottom plays −∞

-- hom-set sizes of the cone, given those of the original category
coneHom :: (o -> o -> Int) -> Obj o -> Obj o -> Int
coneHom _ Bottom Bottom   = 1          -- only the identity
coneHom _ Bottom (Old _)  = 1          -- exactly one leg to each object
coneHom _ (Old _) Bottom  = 0          -- nothing goes back
coneHom h (Old x) (Old y) = h x y

-- Star_2: the cone on the discrete category with two objects (the shape of a product)
star2 :: Obj Bool -> Obj Bool -> Int
star2 = coneHom (\x y -> if x == y then 1 else 0)
```
````
