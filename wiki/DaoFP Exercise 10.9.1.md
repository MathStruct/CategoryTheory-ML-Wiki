#exercise #solution

**Exercise 10.9.1.** What are the unit and counit of the free-monoid adjunction $F \dashv U$?

## Solution

Unit $\eta_X : X \to U F X$, $x \mapsto [x]$ (singleton string). Counit $\varepsilon_m : F U m \to m$, evaluating a string of elements of $m$ by multiplying them (`foldr mappend mempty`, i.e. `mconcat`). See [[Free-Forgetful Adjunction]], [[List Monad]].
