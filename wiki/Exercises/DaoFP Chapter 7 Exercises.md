#exercise

Exercises from DaoFP, Chapter 7. Solutions: [[DaoFP Chapter 7 Solutions]]. Index: [[Map of Content]].

## Exercise 7.1.1

Implement a function turning a `Nat` into an `Int` using the recursor ([[Natural Numbers Object]]).

> Sources: DaoFP Exercise 7.1.1.

**Solution:** [[DaoFP Chapter 7 Solutions#Solution 7.1.1|Solution 7.1.1]]

## Exercise 7.1.2

Implement curried addition as a mapping out of $N$ into the function object $N^N$, using `init :: Nat -> Nat` and `step :: (Nat -> Nat) -> (Nat -> Nat)` in the recursor ([[Natural Numbers Object]], [[Exponential Object]]).

> Sources: DaoFP Exercise 7.1.2.

**Solution:** [[DaoFP Chapter 7 Solutions#Solution 7.1.2|Solution 7.1.2]]

## Exercise 7.2.1

What happens when $a$ is replaced by the terminal object in the definition of a [[List]]?

> Sources: DaoFP Exercise 7.2.1.

**Solution:** [[DaoFP Chapter 7 Solutions#Solution 7.2.1|Solution 7.2.1]]

## Exercise 7.2.2

How many mappings $h : L_a \to 1 + a$ are there? Can we get all of them with the list recursor? What about Haskell functions `h :: [a] -> Maybe a`?

> Sources: DaoFP Exercise 7.2.2.

**Solution:** [[DaoFP Chapter 7 Solutions#Solution 7.2.2|Solution 7.2.2]]

## Exercise 7.2.3

Implement a function extracting the third element of a [[List]], if it is long enough, with result type `Maybe a`.

> Sources: DaoFP Exercise 7.2.3.

**Solution:** [[DaoFP Chapter 7 Solutions#Solution 7.2.3|Solution 7.2.3]]
