#exercise #solution #proof

**Exercise 7.8.** Suppose $A' \to A$, $f' : A' \to B'$, $f : A \to B$, $h : B' \to B$ form a [[Pullback]] square. Use the [[Pasting Lemma for Pullbacks]] and [[7S Exercise 7.7]] to show that if $f$ is a [[Monomorphism]] then so is $f'$.

## Solution

Build a cube: the front and bottom faces are the given pullback, the right face is the square $(A, A, A, B)$ with identities and $f$ — a pullback because $f$ is mono (Definition 7.5) — and the back and top faces are the "identity" squares of [[7S Exercise 7.7]] (2), which are pullbacks. By the pasting lemma, right face + back face pullbacks make the diagonal rectangle a pullback; then front face pullback + rectangle pullback make the left face $(A', A', A', B')$ with identities and $f'$ a pullback, which says $f'$ is mono. Hence monomorphisms are stable under pullback.

```tabs
tab: Lean
```lean
import Mathlib
open CategoryTheory Limits
#check @CategoryTheory.Limits.pullback.fst_of_mono    -- Mono g → Mono (pullback.fst f g)
#check @CategoryTheory.Limits.pullback.snd_of_mono    -- Mono f → Mono (pullback.snd f g)
```
```

> Sources: 7 Sketches, Exercise 7.8 and Solution A.7.
