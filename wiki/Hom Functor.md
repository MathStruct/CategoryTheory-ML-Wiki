#definition #example #theorem

In a (locally small) [[Category]] $\mathcal{C}$ the assignment $(a, b) \mapsto \mathcal{C}(a, b)$ is a [[Functor]]

$$
\mathcal{C}(-, -) : \mathcal{C}^{\mathrm{op}} \times \mathcal{C} \to \mathbf{Set},
$$

the **hom-functor** — a [[Profunctor]]. On arrows $g' : a' \to a$ and $g : b \to b'$ it sends $h : a \to b$ to $g \circ h \circ g' : a' \to b'$ (pre-compose with $g'$, post-compose with $g$; Haskell `dimap f g h = g . h . f`). Fixing one variable:

- **covariant** $\mathcal{C}(a, -) : \mathcal{C} \to \mathbf{Set}$, with $\mathcal{C}(a, g) = (g \circ -)$ (post-composition): "the world according to $a$" — all arrows out of $a$ organized coherently; an *oracle* answering "is $a$ connected to me?";
- **contravariant** $\mathcal{C}(-, b) : \mathcal{C}^{\mathrm{op}} \to \mathbf{Set}$, with $\mathcal{C}(g', b) = (- \circ g')$ (pre-composition): "the picture of $b$ as seen by the world"; "am I connected to $b$?".

> Sources: DaoFP §8.4 ("The Hom-Functor"), §9.1, §9.7, §17.4 ("Continuity of the Hom-Functor"), §20.2 (enriched hom-functor); Kittenlab Lecture 8, 10 (the covariant [[Representable Functor|representable]] $y_{\mathcal{C}}(x) = \mathrm{Hom}(x, -)$, "the shape of the information about all morphisms out of an object is a $\mathcal{C}$-set"); 7 Sketches §3.4.2 (naturality of adjunctions is naturality of maps between hom-functors $\mathcal{C}^{\mathrm{op}} \times \mathcal{D} \to \mathbf{Set}$), Exercise 1.66.

**Properties.**
- The profunctor $\mathcal{C}(x, y)$ is a *proof-relevant relation*: each element is a proof that $x$ is connected to $y$; empty means unrelated (DaoFP). For [[Preorder|preorders]], $\mathcal{C}(x, y)$ is the truth value of $x \leq y$ ([[Upper Set|upper sets]] $\uparrow x$, [[Yoneda Lemma for Preorders]]).
- **Isomorphisms.** If $a \cong b$ then $\mathcal{C}(a, x) \cong \mathcal{C}(b, x)$ and $\mathcal{C}(x, a) \cong \mathcal{C}(x, b)$; conversely a *natural* family of such isomorphisms gives $a \cong b$ ([[Isomorphism]], [[Yoneda Lemma]]).
- **Continuity** (DaoFP §10.7, §17.4): $\mathcal{C}(x, -)$ preserves [[Limit|limits]], $\mathcal{C}(x, \mathrm{Lim}\,D) \cong \mathrm{Lim}\,\mathcal{C}(x, D-)$, and $\mathcal{C}(-, x)$ turns [[Colimit|colimits]] into limits, $\mathcal{C}(\mathrm{Colim}\,D, x) \cong \mathrm{Lim}\,\mathcal{C}(D-, x)$ ("the hom-functor preserves colimits") — because a cone in $\mathbf{Set}$ with apex $1$ over $\mathcal{C}(x, D-)$ is exactly a cone over $D$ with apex $x$. This is the engine behind [[Right Adjoints Preserve Limits]].
- Currying the hom-functor gives the [[Yoneda Embedding]] $\mathcal{C} \to [\mathcal{C}^{\mathrm{op}}, \mathbf{Set}]$ and the co-Yoneda $\mathcal{C}^{\mathrm{op}} \to [\mathcal{C}, \mathbf{Set}]$. In a [[Monoidal Closed Category]] the hom-functor is [[Enriched Functor|enriched]], $\mathrm{Hom} : \mathcal{C}^{\mathrm{op}} \otimes \mathcal{C} \to \mathcal{V}$ (DaoFP §20.2), and [[Exponential Object|internal homs]] $[a, b]$ represent it: $\mathcal{C}(1, b^a) \cong \mathcal{C}(a, b)$.

````tabs
tab: Julia
```julia
# Kittenlab Lecture 8/10: the representable Hom(x, -) on a finitely presented category as a C-set;
# for graphs, Hom(V,-) is the one-vertex graph and Hom(E,-) the one-edge graph.
using Catlab
yV = @acset Graph begin V = 1 end
yE = @acset Graph begin V = 2; E = 1; src = [1]; tgt = [2] end
G = path_graph(Graph, 3)
length(homomorphisms(yV, G)), length(homomorphisms(yE, G))   # (3, 2) = (#vertices, #edges)
```
tab: Lean
```lean
#check CategoryTheory.Functor.hom     -- Cᵒᵖ × C ⥤ Type v, the hom-functor as a profunctor
#check CategoryTheory.coyoneda        -- Cᵒᵖ ⥤ (C ⥤ Type): a ↦ Hom(a, -)
#check CategoryTheory.yoneda          -- C ⥤ (Cᵒᵖ ⥤ Type): b ↦ Hom(-, b)
```
tab: Haskell
```haskell
-- the hom-functor of Hask is the function type, a profunctor
class Profunctor p where
  dimap :: (a' -> a) -> (b -> b') -> (p a b -> p a' b')

instance Profunctor (->) where
  dimap f g h = g . h . f       -- pre-compose with f, post-compose with g

-- fixing the source: the covariant hom-functor (a -> -), i.e. the Reader functor
newtype Reader a x = Reader (a -> x)
instance Functor (Reader a) where fmap g (Reader h) = Reader (g . h)   -- post-composition
```
````
