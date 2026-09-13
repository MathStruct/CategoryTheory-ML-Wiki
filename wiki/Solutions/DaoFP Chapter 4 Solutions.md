#solution

Solutions to the exercises of DaoFP, Chapter 4: [[DaoFP Chapter 4 Exercises]]. Index: [[Map of Content]].

## Solution 4.1.1

#program — [[DaoFP Chapter 4 Exercises#Exercise 4.1.1|Exercise 4.1.1]]

A function out of `Bool` is a pair of elements of the target; the four pairs of booleans give four functions:

```haskell
idB, constTrue, constFalse, notB :: Bool -> Bool
idB b        = if b then True  else False    -- (True, False)
constTrue b  = if b then True  else True     -- (True, True)
constFalse b = if b then False else False    -- (False, False)
notB b       = if b then False else True     -- (False, True)
```

> Sources: DaoFP Exercise 4.1.1.

## Solution 4.4.1

#program — [[DaoFP Chapter 4 Exercises#Exercise 4.4.1|Exercise 4.4.1]]

```haskell
import Data.Void (Void, absurd)
f :: Either a Void -> a
f (Left a)  = a
f (Right v) = absurd v          -- unreachable: Void has no terms

f_1 :: a -> Either a Void
f_1 = Left
```

`f . f_1 = id` by the computation rule; `f_1 . f = id` because the `Right` case never occurs. Categorically: arrows out of $a + 0$ are pairs $(x, \text{¡})$ with $\text{¡}$ unique, hence in natural bijection with arrows out of $a$.

> Sources: DaoFP Exercise 4.4.1.

## Solution 4.4.2

#proof — [[DaoFP Chapter 4 Exercises#Exercise 4.4.2|Exercise 4.4.2]]

Change focus along $k : x \to y$. Post-composing $h = [f, g]$ with $k$ gives $k \circ h = [k \circ f, k \circ g]$ (by uniqueness of copairing); applying $\beta_y$ yields $[k \circ g, k \circ f]$. Alternatively apply $\beta_x$ first, getting $[g, f]$, then post-compose: $k \circ [g, f] = [k \circ g, k \circ f]$. Both routes agree, so $\beta$ is natural; by the [[Yoneda Lemma]] $a + b \cong b + a$.

> Sources: DaoFP Exercise 4.4.2.

## Solution 4.4.3

#program — [[DaoFP Chapter 4 Exercises#Exercise 4.4.3|Exercise 4.4.3]]

```haskell
swapE :: Either a b -> Either b a
swapE (Left a)  = Right a
swapE (Right b) = Left b
-- swapE . swapE = id, so swapE is an involution (point-free: swapE = either Right Left)
```

> Sources: DaoFP Exercise 4.4.3.

## Solution 4.4.4

#proof — [[DaoFP Chapter 4 Exercises#Exercise 4.4.4|Exercise 4.4.4]]

The lifted arrows are $\langle \mathrm{id}, g \rangle = [\mathsf{Left}, \mathsf{Right} \circ g]$ and $\langle \mathrm{id}, g' \rangle = [\mathsf{Left}, \mathsf{Right} \circ g']$. Their composite, precomposed with the injections, gives $\mathsf{Left} \mapsto \mathsf{Left}$ and $\mathsf{Right} \mapsto \mathsf{Right} \circ g' \circ g$; so does $\langle \mathrm{id}, g' \circ g \rangle$. By uniqueness of the copairing they are equal. In Haskell: `bimap id g' . bimap id g = bimap id (g' . g)`, checked on both constructors.

> Sources: DaoFP Exercise 4.4.4.

## Solution 4.4.5

#proof — [[DaoFP Chapter 4 Exercises#Exercise 4.4.5|Exercise 4.4.5]]

$\langle \mathrm{id}, \mathrm{id} \rangle = [\mathsf{Left} \circ \mathrm{id}, \mathsf{Right} \circ \mathrm{id}] = [\mathsf{Left}, \mathsf{Right}]$, and $\mathrm{id}_{a+b}$ also satisfies $\mathrm{id} \circ \mathsf{Left} = \mathsf{Left}$, $\mathrm{id} \circ \mathsf{Right} = \mathsf{Right}$; uniqueness of copairing gives equality. (Same as [[7S Chapter 6 Exercises#Exercise 6.17|7S Exercise 6.17]] (4).)

> Sources: DaoFP Exercise 4.4.5.
