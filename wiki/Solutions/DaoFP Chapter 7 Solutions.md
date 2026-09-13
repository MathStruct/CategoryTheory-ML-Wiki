#solution

Solutions to the exercises of DaoFP, Chapter 7: [[DaoFP Chapter 7 Exercises]]. Index: [[Map of Content]].

## Solution 7.1.1

#program — [[DaoFP Chapter 7 Exercises#Exercise 7.1.1|Exercise 7.1.1]]

```haskell
natToInt :: Nat -> Int
natToInt = rec 0 (+ 1)         -- init = 0, step = successor on Int
```

> Sources: DaoFP Exercise 7.1.1.

## Solution 7.1.2

#program — [[DaoFP Chapter 7 Exercises#Exercise 7.1.2|Exercise 7.1.2]]

```haskell
plusC :: Nat -> (Nat -> Nat)
plusC = rec init step
  where
    init = id                  -- 0 + m = m
    step f = S . f             -- (n+1) + m = S (n + m)
```

Here the target of the recursor is the exponential $N^N$; `init` is the element $\mathrm{id} \in N^N$ and `step` post-composes with $S$.

> Sources: DaoFP Exercise 7.1.2.

## Solution 7.2.1

#annotation — [[DaoFP Chapter 7 Exercises#Exercise 7.2.1|Exercise 7.2.1]]

$L_1$ has constructors $\mathsf{Nil} : 1 \to L_1$ and $\mathsf{Cons} : 1 \times L_1 \cong L_1 \to L_1$ — exactly $Z$ and $S$ of the [[Natural Numbers Object]]. A list of units is a natural number in base-one (unary) encoding: its length.

> Sources: DaoFP Exercise 7.2.1.

## Solution 7.2.2

#annotation — [[DaoFP Chapter 7 Exercises#Exercise 7.2.2|Exercise 7.2.2]]

In $\mathbf{Set}$ there are uncountably many functions $L_a \to 1 + a$ (any assignment of `Nothing` or an element to each list), and only countably many are folds built from finitely describable `init` and `step`, so the recursor does not reach them all — an arbitrary mapping out of a recursive type contains infinite information. Haskell functions `[a] -> Maybe a` that are *parametric* in `a` are far fewer: they can only return `Nothing` or one of the list's elements chosen by position, and each such function (e.g. `safeHead`, `last`, the third element) is a fold.

> Sources: DaoFP Exercise 7.2.2.

## Solution 7.2.3

#program — [[DaoFP Chapter 7 Exercises#Exercise 7.2.3|Exercise 7.2.3]]

```haskell
third :: [a] -> Maybe a
third (_ : _ : x : _) = Just x
third _               = Nothing
-- pattern matching is the idiomatic elimination rule; as a fold one would thread a counter
-- through the accumulator, which is far less readable.
```

> Sources: DaoFP Exercise 7.2.3.
