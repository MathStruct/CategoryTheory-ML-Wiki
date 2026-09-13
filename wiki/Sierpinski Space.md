#example #exercise

The **Sierpiński space** is the [[Topological Space]] $(\{1, 2\}, \mathrm{Op}_1)$ with $\mathrm{Op}_1 = \{\varnothing, \{1\}, \{1, 2\}\}$ (Example 7.30). Its poset of opens is the chain $\varnothing \to \{1\} \to \{1, 2\}$, and the only non-trivial cover is the empty cover of $\varnothing$ ([[7S Chapter 7 Exercises#Exercise 7.31|7S Exercise 7.31]]).

> Sources: 7 Sketches Example 7.30, Exercises 7.31, 7.49.

- A [[Presheaf]] on $\mathrm{Op}_1$ is three sets and two functions $F(\{1,2\}) \to F(\{1\}) \to F(\varnothing)$; it is a [[Sheaf]] iff $F(\varnothing) = \{()\}$, so $\mathbf{Shv}(\text{Sierpiński})$ is equivalent to the category of *functions* $F(\{1,2\}) \to F(\{1\})$, i.e. the [[Functor Category|arrow category]] of $\mathbf{Set}$ ([[7S Chapter 7 Exercises#Exercise 7.49|7S Exercise 7.49]]).
- The Sierpiński space is the "open-set classifier" in $\mathbf{Top}$: continuous maps $X \to \{1, 2\}$ correspond to open subsets $f^{-1}(1) \subseteq X$, just as maps to [[Booleans|$\mathbb{B}$]] classify subsets ([[Subobject Classifier]]).

````tabs
tab: Lean
```lean
import Mathlib
#check @sierpinskiSpace                  -- the topology on Prop with {True} open
#check @isOpen_singleton_true
#check @continuous_Prop                  -- continuous f ↔ IsOpen {x | f x}
```
tab: Haskell
```haskell
-- sheaves on Sierpiński space = functions; a "section" over {1,2} restricts to one over {1}
data SierpSheaf a b = SierpSheaf { global :: [a], local1 :: [b], restrict :: a -> b }
```
````
