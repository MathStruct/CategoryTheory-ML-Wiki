#definition #theorem #example

In a [[Markov Category]], a morphism $f : A \to X \otimes W \otimes Y$ **displays the conditional independence** $X \perp Y \mid W \,\|\, A$ if it factors as

$$
f \;=\; A \xrightarrow{\ \mathrm{copy}\ } A \otimes A \xrightarrow{\ g \otimes 1\ } W \otimes A \xrightarrow{\ \text{copy } W \text{ twice}\ } \cdots \xrightarrow{\ h \otimes 1_W \otimes k\ } X \otimes W \otimes Y
$$

for some $g : A \to W$, $h : A \otimes W \to X$ and $k : W \otimes A \to Y$ (Fritz, Definition 12.16): given $W$ (and the input $A$), $X$ and $Y$ are produced by **separate** kernels with no shared randomness. The unconditional case $X \perp Y \,\|\, A$ (Definition 12.12) says $f = (f_X \otimes f_Y) \circ \mathrm{copy}_A$ — the joint is the product of its marginals.

> Sources: Fritz [arXiv:1908.07021](https://arxiv.org/abs/1908.07021) ([[A Synthetic Approach to Markov Kernels, Conditional Independence and Theorems on Sufficient Statistics|notes]]) §12 (Definitions 12.1, 12.12, 12.16, 12.19, Propositions 12.17–12.20, Lemmas 12.11, 12.13); Cho & Jacobs [arXiv:1709.00322](https://arxiv.org/abs/1709.00322) ([[Disintegration and Bayesian Inversion via String Diagrams|notes]]) Definition 6.6, Propositions 6.9–6.10; Fritz & Klingler, *The d-separation criterion in Categorical Probability* [arXiv:2207.05740](https://arxiv.org/abs/2207.05740) ([[The d-separation Criterion in Categorical Probability|notes]]); Dawid (1979), *Conditional independence in statistical theory*.

## The semigraphoid laws become theorems

Symmetry, decomposition, weak union and contraction — the axioms Dawid and Pearl used to reason about independence — are *provable* from the string-diagram definition (Fritz Propositions 12.17–12.20; Cho & Jacobs Proposition 6.10). Each is a rewrite of a diagram; for instance **symmetry** is sliding the two output wires past each other, and **decomposition** ($X \perp YZ \mid W \Rightarrow X \perp Y \mid W$) is deleting the $Z$ output.

## Why it matters for machine learning

- **Graphical models are diagrams.** A Bayesian network is a string diagram in a Markov category; the independences it asserts are read off by **d-separation**, which Fritz & Klingler prove is sound and complete for Markov categories.
- **Mean-field is an independence assumption.** A variational posterior that factorises over blocks asserts $X \perp X' \mid \text{observations}$. Where the true posterior violates it, the error is the conditional mutual information $I(X; X' \mid \cdot)$ — the laxness measured by AutoBayes Remark 26 ([[Lax Functor]], [[Statistical Game]]).
- **Algebraic statistics.** For discrete variables, $X \perp Y \mid W$ holds iff certain $2 \times 2$ minors of the joint table vanish: conditional independence models are *determinantal varieties*.

````tabs
tab: Julia
**Docs:** [Theories (Catlab): copy/delete — ThMonoidalCategoryWithDiagonals](https://algebraicjulia.github.io/Catlab.jl/v0.16/apis/theories/)
```julia
# X ⊥ Y | W for a finite joint p(x, w, y): p(x,w,y) p(w) = p(x,w) p(w,y).
function ci(p)                        # p :: Array{Float64,3} indexed [x, w, y]
    pw  = vec(sum(p, dims = (1, 3)))
    pxw = dropdims(sum(p, dims = 3), dims = 3)
    pwy = dropdims(sum(p, dims = 1), dims = 1)
    all(isapprox(p[x, w, y] * pw[w], pxw[x, w] * pwy[w, y]; atol = 1e-12)
        for x in axes(p, 1), w in axes(p, 2), y in axes(p, 3))
end
# build p = p(w) h(x|w) k(y|w): conditionally independent by construction
pw = [0.4, 0.6]; h = [0.9 0.1; 0.3 0.7]; k = [0.2 0.8; 0.5 0.5]   # h[w, x], k[w, y]
p = [pw[w] * h[w, x] * k[w, y] for x in 1:2, w in 1:2, y in 1:2]
ci(p)                                                             # true
q = copy(p); q[1, 1, 1] += 0.05; q[2, 1, 1] -= 0.05               # couple x and y given w = 1
ci(q)                                                             # false
```
tab: Lean
```lean
import Mathlib
open MeasureTheory ProbabilityTheory
-- Mathlib's conditional independence of σ-algebras / random variables given a σ-algebra
#check @CondIndep
#check @CondIndepFun
#check @iCondIndepFun
```
tab: Haskell
```haskell
-- Check X ⊥ Y | W on a finite joint given as a function p x w y
ci :: [a] -> [b] -> [c] -> (a -> b -> c -> Double) -> Bool
ci xs ws ys p = and [ abs (p x w y * pw w - pxw x w * pwy w y) < 1e-12 | x <- xs, w <- ws, y <- ys ]
  where pw w    = sum [ p x w y | x <- xs, y <- ys ]
        pxw x w = sum [ p x w y | y <- ys ]
        pwy w y = sum [ p x w y | x <- xs ]
```
````
