#definition #example

The **equalizer** of a parallel pair $f, g : a \rightrightarrows b$ is the [[Limit]] of the diagram of shape $\bullet \rightrightarrows \bullet$: an object $e$ with $p : e \to a$ such that $f \circ p = g \circ p$, universal: any $h : x \to a$ with $f \circ h = g \circ h$ factors uniquely through $p$. (The second leg $p' = f \circ p = g \circ p$ is determined.)

> Sources: DaoFP §9.5 ("Equalizers"); 7 Sketches §7.2 (equalizers in a topos); Kittenlab Lecture 13–14 (subsets as constraints; "limits allow you to filter"); CTfS Definition 2.5.3.1, Example 2.5.3.2, Exercises 2.5.3.3–2.5.3.4, 3.3.1.10

- **In $\mathbf{Set}$**: $E = \{x \in A \mid f(x) = g(x)\}$ — "an equation equates the outcomes of two ways of producing something; in geometry, the intersection of two geometric objects; in category theory all these patterns are embodied in the equalizer". Elements of $E$ (arrows $1 \to E$) are the solutions of the system of equations.
- **Theory meets experiment** (CTfS Example 2.5.3.2): with two functions "an input *should, according to theory, yield* an output" and "an input *according to experiment yields* an output", their equalizer is the set of inputs on which theory and experiment agree. Similarly, "an author who has published exactly one paper" is the equalizer of "has as first paper" and "has as most recent paper" ([[CTfS Chapter 2 Exercises#Exercise 2.5.3.3|CTfS Exercise 2.5.3.3]]); the equalizer of $\mathrm{src}, \mathrm{tgt}$ of a [[Graph]] is its set of loops (CTfS Exercise 3.3.1.10).
- Equalizers are [[Monomorphism|monos]]; a category with all products and all equalizers has all [[Limit|limits]] (DaoFP: "in a complete category you can equalize an arbitrary set of arrows"). Kernels in algebra are equalizers with the zero map. A subset given by a constraint $[\phi(x) = \psi(x)]$ (Kittenlab's "subsets as constraints", the filled parabola $y \geq x^2$) is an equalizer-like [[Subobject]].
- Dual: [[Coequalizer]] ("bucketizing").

````tabs
tab: Julia
**Docs:** [FinSets](https://algebraicjulia.github.io/Catlab.jl/v0.16/apis/categorical_algebra/#Catlab.CategoricalAlgebra.FinSets) · [Limits & colimits](https://algebraicjulia.github.io/Catlab.jl/v0.16/apis/categorical_algebra/#Catlab.CategoricalAlgebra.Limits) — Kittenlab [Lecture 13](https://algebraicjulia.github.io/Kittenlab.jl/lecture13.html)
```julia
using Catlab
f = FinFunction([1, 2, 2, 3], 3); g = FinFunction([1, 1, 2, 2], 3)
E = equalizer(f, g)
apex(E), collect(incl(E))        # FinSet(2), [1, 3]: the elements where f and g agree
```
tab: Lean
```lean
#check CategoryTheory.Limits.equalizer        -- equalizer f g with equalizer.ι and equalizer.lift
#check CategoryTheory.Limits.equalizer.condition
#check CategoryTheory.Limits.Types.equalizerIso   -- { x // f x = g x }
```
tab: Haskell
```haskell
equalizerSet :: Eq b => [a] -> (a -> b) -> (a -> b) -> [a]
equalizerSet as f g = [ a | a <- as, f a == g a ]     -- solutions of f a = g a
```
````
