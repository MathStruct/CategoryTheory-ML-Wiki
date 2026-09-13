#solution #program

**Solution to [[DaoFP Exercise 5.1.2|Exercise 5.1.2]].**

A map into a product is a pair; each component maps out of a sum, so is itself a pair:
$$h = \langle [\mathsf{Left} \circ !,\ \mathsf{Right} \circ \mathsf{fst}],\ [\mathrm{id}_b,\ \mathsf{snd}] \rangle .$$
It is *not* unique: e.g. the first component could use $\mathsf{Left} \circ !$ on both summands (forgetting the $a$), or the second component could be any other arrow $a \times b \to b$. The type does not pin down the function.

```haskell
h :: Either b (a, b) -> (Either () a, b)
h (Left b)       = (Left (), b)
h (Right (a, b)) = (Right a, b)
```

> Sources: DaoFP Exercise 5.1.2.
