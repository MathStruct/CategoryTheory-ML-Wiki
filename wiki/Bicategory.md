#definition #example

A **bicategory** $\mathcal B$ has objects $A, B, \dots$, and for each pair of objects a *hom-category* $\mathcal B(A, B)$ whose objects are **1-cells** $f : A \to B$ and whose morphisms are **2-cells** $\alpha : f \Rightarrow g$. There are composition functors $\circ : \mathcal B(B, C) \times \mathcal B(A, B) \to \mathcal B(A, C)$ and identity 1-cells $1_A$, but composition of 1-cells is associative and unital only **up to specified invertible 2-cells**

$$
a_{h,g,f} : (h \circ g) \circ f \xRightarrow{\ \cong\ } h \circ (g \circ f),
\qquad
\lambda_f : 1_B \circ f \xRightarrow{\ \cong\ } f,
\qquad
\rho_f : f \circ 1_A \xRightarrow{\ \cong\ } f,
$$

natural in $f, g, h$ and satisfying the pentagon and triangle identities — exactly the coherence data of a [[Monoidal Category]], one dimension up. A bicategory with one object *is* a monoidal category (1-cells are objects, horizontal composition is $\otimes$), and a bicategory whose associators and unitors are identities is a strict [[2-Category]].

> Sources: Bénabou (1967), *Introduction to bicategories*; Johnson & Yau, *2-Dimensional Categories* (Oxford 2021); Cruttwell et al. [arXiv:2103.01931](https://arxiv.org/abs/2103.01931) ([[Categorical Foundations of Gradient-Based Learning|notes]]) Remark 2.1 (the 2-categorical perspective on $\mathbf{Para}$); Capucci et al. [arXiv:2105.06332](https://arxiv.org/abs/2105.06332) ([[Towards Foundations of Categorical Cybernetics|notes]]) Definition 2; St Clere Smithe & Perin, *AutoBayes* [arXiv:2503.18608](https://arxiv.org/abs/2503.18608) ([[AutoBayes - A Compositional Framework for Generalized Variational Inference|notes]]) Remarks 5, 7, 24, 30.

## Why bicategories show up in categorical machine learning

Whenever a morphism carries *extra data that gets multiplied under composition*, composition is associative only up to reassociating that data — and a bicategory appears.

- **[[Para Construction|$\mathbf{Para}(\mathcal C)$]]** (Capucci et al., Definition 2). A 1-cell $A \to B$ is a pair $(P, f : P \otimes A \to B)$; composing $(P, f)$ and $(Q, g)$ has parameter $Q \otimes P$. Associativity holds only up to the associator $(R \otimes Q) \otimes P \cong R \otimes (Q \otimes P)$ of $\mathcal C$, and the 2-cells are the **reparametrisations** $r : P' \to P$. Cruttwell et al. (Remark 2.1) quotient this away to get a category; optimisers, weight tying and LoRA *are* 2-cells, so the quotient loses information.
- **[[Open Model|Open models]]** (AutoBayes, Remark 5). A 1-cell $X \to Y$ is a kernel $X \rightsquigarrow [\![p]\!] \times Y$ with a *latent* space; composites accumulate latent spaces $[\![p]\!] \times Y \times [\![q]\!]$, again associative up to isomorphism. The operation `reveal`, which moves a latent factor into the observed codomain, is a 2-cell.
- **[[Statistical Game|Statistical games]]** and **[[Bayesian Lens|Bayesian lenses]]** (AutoBayes, Remark 24) inherit the bicategory structure of open models, and *parameterized* statistical games form a **monoidal bicategory** (Remark 30).
- **Spans and cospans.** [[Span|Spans]] in a category with pullbacks form the bicategory $\mathbf{Span}(\mathcal C)$ (composition by pullback, defined only up to isomorphism); dually [[Cospan|cospans]] compose by pushout. This is the original example of Bénabou.
- **[[Bicategory of Profunctors|Profunctors]]** compose by a [[Coend|coend]], which is again only associative up to isomorphism.

## Examples

- $\mathbf{Cat}$ is a (strict) 2-category: 1-cells are functors, 2-cells natural transformations.
- $\mathbf{Rel}$ as a *locally posetal* bicategory: hom-categories are the posets $(\mathcal P(A \times B), \subseteq)$, a 2-cell $R \Rightarrow S$ exists iff $R \subseteq S$.
- A monoidal category $(\mathcal M, \otimes, I)$ is the one-object bicategory $\mathbf B\mathcal M$ ("delooping").
- $\mathbf{Para}(\mathbf{Smooth})$: a 1-cell $\mathbb R^n \to \mathbb R^m$ is a neural network layer with its weight space; a 2-cell is a map of weight spaces compatible with the layers.

## Coherence and strictification

Every bicategory is biequivalent to a 2-category (Mac Lane–Paré coherence), so for most purposes one can compute as if associativity held strictly — in practice, *this is what implementations do*: a parameter tree stored as a nested `NamedTuple` (Lux.jl, Lenticulum.jl) is a canonical representative of $(R \otimes Q) \otimes P$, and the associator is never materialised. Functors between bicategories come in strict, pseudo, lax and oplax flavours; see [[Lax Functor]].

## Lenticulum.jl

Lenticulum's factors are 1-cells in the bicategory of [[Statistical Game|parameterized statistical games]]; its parameter trees are the $\mathbf{Para}$ 2-cell data made concrete. See [Factors are Parameterized Statistical Games](https://mathstruct.org/Lenticulum.jl/dev/vault/Foundations/Factors-are-Parameterized-Statistical-Games).

````tabs
tab: Julia
**Docs:** [Theories (Catlab): ThBicategoryRelations](https://algebraicjulia.github.io/Catlab.jl/v0.16/apis/theories/)
```julia
# 1-cells of Para(Set): a parameter "space" (here: a type) and a map P × A → B.
struct Para1{P,F}; ptype::Type{P}; f::F; end
compose(g::Para1, f::Para1) = Para1(Tuple{g.ptype, f.ptype}, ((q, p), a) -> g.f(q, f.f(p, a)))
# the associator is a relabelling of nested parameter tuples, not an equality
assoc(((r, q), p)) = (r, (q, p))
f = Para1(Float64, (w, x) -> w * x)          # a scaling layer
g = Para1(Float64, (b, x) -> x + b)          # a bias layer
h = Para1(Float64, (s, x) -> tanh(s * x))
left  = compose(compose(h, g), f)            # parameter shape ((s, b), w)
right = compose(h, compose(g, f))            # parameter shape (s, (b, w))
x, s, b, w = 0.3, 2.0, 0.1, 1.5
left.f(((s, b), w), x) ≈ right.f(assoc(((s, b), w)), x)   # equal up to the associator: true
# a 2-cell (reparametrisation) r : Q → P, here weight tying P = (Float64, Float64) ← Q = Float64
tie(θ) = (θ, θ)
reparam(φ::Para1, r, Q) = Para1(Q, (q, a) -> φ.f(r(q), a))
tied = reparam(compose(f, f), tie, Float64)
tied.f(3.0, 1.0) == 9.0                       # both layers share one weight
```
tab: Lean
```lean
import Mathlib
open CategoryTheory
#check Bicategory                       -- hom-categories, associator, unitors, pentagon, triangle
#check @Bicategory.associator
#check @Bicategory.leftUnitor
#check MonoidalSingleObj                -- a monoidal category as a one-object bicategory
```
tab: Haskell
```haskell
{-# LANGUAGE GADTs #-}
-- Para over (Hask, (,)): a 1-cell a -> b with parameter p
newtype Para p a b = Para { runPara :: (p, a) -> b }

compose :: Para q b c -> Para p a b -> Para (q, p) a c
compose (Para g) (Para f) = Para (\((q, p), a) -> g (q, f (p, a)))

-- the associator: reassociate the parameter, a 2-cell rather than an equality
assoc :: Para ((r, q), p) a d -> Para (r, (q, p)) a d
assoc (Para k) = Para (\((r, (q, p)), a) -> k (((r, q), p), a))

-- a 2-cell / reparametrisation along r :: p' -> p
reparam :: (p' -> p) -> Para p a b -> Para p' a b
reparam r (Para f) = Para (\(p', a) -> f (r p', a))
```
````
