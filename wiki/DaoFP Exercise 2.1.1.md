#exercise #solution #proof

**Exercise 2.1.1.** For $f : a \to b$, $g : b \to c$, show $((g \circ f) \circ -) = (g \circ -) \circ (f \circ -)$ as maps on arrows $h : x \to a$.

## Solution

Both sides send $h$ to $g \circ (f \circ h) = (g \circ f) \circ h$ by associativity. Post-composition is thus a [[Functor]]: the covariant [[Hom Functor]] $\mathcal{C}(x, -)$.
