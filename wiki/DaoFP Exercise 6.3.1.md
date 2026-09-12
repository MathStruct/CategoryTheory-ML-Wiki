#exercise #solution #proof #program

**Exercise 6.3.1.** Show that $2 \times a \cong a + a$, where $2$ is `Bool`. Do the proof diagrammatically, then implement both directions in Haskell ([[Bicartesian Closed Category]], distributivity).

## Solution

By distributivity, $2 \times a = (1 + 1) \times a \cong 1 \times a + 1 \times a \cong a + a$. Directly: a map out of $a + a$ into $2 \times a$ is a pair $\langle \mathsf{True} \circ !, \mathrm{id} \rangle$, $\langle \mathsf{False} \circ !, \mathrm{id} \rangle$; the inverse $2 \times a \to a + a$ is `uncurry` of the map $2 \to (a + a)^a$ given by the pair of elements $\mathsf{curry}\, \mathsf{Left}$, $\mathsf{curry}\, \mathsf{Right}$ — this direction uses the [[Exponential Object]], as in `undist`.

```haskell
toSum :: (Bool, a) -> Either a a
toSum (True,  a) = Left a
toSum (False, a) = Right a

fromSum :: Either a a -> (Bool, a)
fromSum (Left a)  = (True, a)
fromSum (Right a) = (False, a)
-- point-free: toSum = uncurry (\b -> if b then Left else Right)
```

> Sources: DaoFP Exercise 6.3.1.
