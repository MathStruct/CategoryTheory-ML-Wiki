#solution #proof

**Solution to [[DaoFP Exercise 16.0.1|Exercise 16.0.1]].**

For $f : (a, e) \to b$, $g : (b, e) \to c$, $h : (c, e) \to d$:
`(h ∘ (g ∘ f)) (a, e) = h ((g ∘ f)(a, e), e) = h (g (f (a, e), e), e)` and
`((h ∘ g) ∘ f) (a, e) = (h ∘ g) (f (a, e), e) = h (g (f (a, e), e), e)`. Both pass the same environment $e$ to every stage, so they agree. The identity `idWithEnv (a, e) = a` is a two-sided unit.

> Sources: DaoFP Exercise 16.0.1.
