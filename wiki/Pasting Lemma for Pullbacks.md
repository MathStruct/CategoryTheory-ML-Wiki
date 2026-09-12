#theorem #proof

**Proposition 7.3 (pasting lemma).** In the commutative diagram below suppose the right square $(B, C, B', C')$ is a [[Pullback]]. Then the left square $(A, B, A', B')$ is a pullback if and only if the outer rectangle $(A, C, A', C')$ is a pullback.

```tikz
\usepackage{tikz-cd}
\begin{document}
\begin{tikzcd}
A \arrow[r, "f"] \arrow[d, "h_1"'] & B \arrow[r, "g"] \arrow[d, "h_2"] \arrow[dr, phantom, "\lrcorner", very near start] & C \arrow[d, "h_3"] \\
A' \arrow[r, "f'"'] & B' \arrow[r, "g'"'] & C'
\end{tikzcd}
\end{document}
```

This removes the ambiguity of the corner symbol $\lrcorner$ in a rectangle made of two squares: when the right square is a pullback, "the left square is a pullback" and "the whole rectangle is a pullback" mean the same thing.

> Sources: 7 Sketches §7.2.1, Proposition 7.3, Exercise 7.4 (proof), Exercises 7.7–7.8 (applications: pullback of an iso is an iso, monos are pullback-stable).

## Proof ([[7S Exercise 7.4]])

($\Rightarrow$) Suppose the left square is a pullback and let $(X, p : X \to C, q : X \to A')$ satisfy $q \mathbin{;} f' \mathbin{;} g' = p \mathbin{;} h_3$. The right pullback gives a unique $r : X \to B$ with $r \mathbin{;} h_2 = q \mathbin{;} f'$ and $r \mathbin{;} g = p$; then the left pullback gives a unique $r' : X \to A$ with $r' \mathbin{;} f = r$, $r' \mathbin{;} h_1 = q$. So $r'$ mediates for the rectangle, and any other mediator $r_0$ must have $r_0 \mathbin{;} f = r$ (uniqueness for the right square) and then $r_0 = r'$ (uniqueness for the left square).

($\Leftarrow$) Suppose the rectangle is a pullback and $(X, r : X \to B, q : X \to A')$ satisfies $r \mathbin{;} h_2 = q \mathbin{;} f'$. Put $p := r \mathbin{;} g$; then $p \mathbin{;} h_3 = q \mathbin{;} f' \mathbin{;} g'$, so the rectangle gives a unique $r' : X \to A$ with $r' \mathbin{;} f \mathbin{;} g = p$ and $r' \mathbin{;} h_1 = q$. Both $r' \mathbin{;} f$ and $r$ satisfy the two equations characterizing the mediator into the right pullback, so $r' \mathbin{;} f = r$. Uniqueness of $r'$ follows from uniqueness for the rectangle. $\blacksquare$

## Consequences

- The pullback of an [[Isomorphism]] is an isomorphism, and $(A, B, A, B)$ with identities and $f$ twice is a pullback ([[7S Exercise 7.7]]).
- [[Monomorphism|Monos]] are stable under pullback ([[7S Exercise 7.8]]): two applications of the lemma to a cube.

````tabs
tab: Julia
```julia
using Catlab
# pasting in FinSet: pulling back in two steps equals pulling back along the composite
g′ = FinFunction([1, 2, 2], 3)          # B' → C'
h3 = FinFunction([1, 3, 3, 2], 3)       # C  → C'
f′ = FinFunction([1, 1, 2], 3)          # A' → B'
right = pullback(g′, h3)                # B := B' ×_{C'} C, with h2 : B → B'
h2 = legs(right)[1]
left = pullback(f′, h2)                 # A := A' ×_{B'} B
outer = pullback(f′ ⋅ g′, h3)           # A' ×_{C'} C
ob(left) == ob(outer)                   # true: FinSet(3) both ways
```
tab: Lean
```lean
import Mathlib
open CategoryTheory Limits
#check @CategoryTheory.Limits.bigSquareIsPullback   -- left ∧ right pullbacks ⇒ rectangle
#check @CategoryTheory.Limits.leftSquareIsPullback  -- rectangle ∧ right ⇒ left
#check @CategoryTheory.IsPullback.paste_horiz
#check @CategoryTheory.IsPullback.of_right          -- cancellation form
```
````
