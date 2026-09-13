#solution

Solutions to the exercises of DaoFP, Chapter 5: [[DaoFP Chapter 5 Exercises]]. Index: [[Map of Content]].

## Solution 5.1.1

#proof — [[DaoFP Chapter 5 Exercises#Exercise 5.1.1|Exercise 5.1.1]]

The bijection $\mathcal{C}(x, 1 \times a) \cong \mathcal{C}(x, a)$ sends $h = \langle !, f \rangle$ to $f = \mathsf{snd} \circ h$ (the component into $1$ is forced). Changing focus along $g : a \to b$: post-composing $h$ with $\mathrm{id}_1 \times g$ gives $\langle !, g \circ f \rangle$, whose image is $g \circ f$; applying the bijection first and then $g \circ -$ gives $g \circ f$ too. Naturality in $x$ (pre-composition with $k : x' \to x$) is equally immediate. Hence $\lambda = \mathsf{snd}$ is a natural isomorphism.

> Sources: DaoFP Exercise 5.1.1.

## Solution 5.1.2

#program — [[DaoFP Chapter 5 Exercises#Exercise 5.1.2|Exercise 5.1.2]]

A map into a product is a pair; each component maps out of a sum, so is itself a pair:

$$
h = \langle [\mathsf{Left} \circ !,\ \mathsf{Right} \circ \mathsf{fst}],\ [\mathrm{id}_b,\ \mathsf{snd}] \rangle .
$$

It is *not* unique: e.g. the first component could use $\mathsf{Left} \circ !$ on both summands (forgetting the $a$), or the second component could be any other arrow $a \times b \to b$. The type does not pin down the function.

```haskell
h :: Either b (a, b) -> (Either () a, b)
h (Left b)       = (Left (), b)
h (Right (a, b)) = (Right a, b)
```

> Sources: DaoFP Exercise 5.1.2.

## Solution 5.1.3

#program — [[DaoFP Chapter 5 Exercises#Exercise 5.1.3|Exercise 5.1.3]]

A map out of $b + a \times b$ is a pair $[h_1, h_2]$ with $h_1 : b \to (1 + a) \times b$ and $h_2 : a \times b \to (1 + a) \times b$, each a map into a product: $h_1 = \langle \mathsf{Left} \circ !, \mathrm{id} \rangle$, $h_2 = \langle \mathsf{Right} \circ \mathsf{fst}, \mathsf{snd} \rangle$. So $h = [\langle \mathsf{Left} \circ !, \mathrm{id} \rangle, \langle \mathsf{Right} \circ \mathsf{fst}, \mathsf{snd} \rangle]$ — the same function as before, reached by decomposing in the other order (`either (\b -> (Left (), b)) (\(a, b) -> (Right a, b))`).

> Sources: DaoFP Exercise 5.1.3.

## Solution 5.1.4

#program — [[DaoFP Chapter 5 Exercises#Exercise 5.1.4|Exercise 5.1.4]]

```haskell
maybeAB :: Either b (a, b) -> (Maybe a, b)
maybeAB (Left b)       = (Nothing, b)
maybeAB (Right (a, b)) = (Just a, b)
```

Not unique: `maybeAB (Right (a, b)) = (Nothing, b)` also type-checks, as would returning `Nothing` everywhere. Parametricity forces the `b` component (the only `b` available) but leaves the `Maybe a` component free.

> Sources: DaoFP Exercise 5.1.4.
