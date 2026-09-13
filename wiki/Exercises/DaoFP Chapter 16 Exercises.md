#exercise

Exercises from DaoFP, Chapter 16. Solutions: [[DaoFP Chapter 16 Solutions]]. Index: [[Map of Content]].

## Exercise 16.0.1

Show that composition of co-Kleisli arrows `composeWithEnv g f = \(a, e) -> g (f (a, e), e)` is associative ([[Comonad]]).

> Sources: DaoFP Exercise 16.0.1.

**Solution:** [[DaoFP Chapter 16 Solutions#Solution 16.0.1|Solution 16.0.1]]

## Exercise 16.1.1

Implement `duplicate` in terms of `extend` and vice versa ([[Comonad]]).

> Sources: DaoFP Exercise 16.1.1.

**Solution:** [[DaoFP Chapter 16 Solutions#Solution 16.1.1|Solution 16.1.1]]

## Exercise 16.1.2

Implement the [[Comonad]] instance for the bidirectional stream `data BiStream a = BStr [a] [a]` (past in reverse order; head of the second list = present; its tail = future), both lists infinite.

> Sources: DaoFP Exercise 16.1.2.

**Solution:** [[DaoFP Chapter 16 Solutions#Solution 16.1.2|Solution 16.1.2]]

## Exercise 16.1.3

Implement a low-pass filter for `BiStream` averaging the current value with its immediate past and future; also a Gaussian filter ([[Comonad]]).

> Sources: DaoFP Exercise 16.1.3.

**Solution:** [[DaoFP Chapter 16 Solutions#Solution 16.1.3|Solution 16.1.3]]

## Exercise 16.3.1

Run a few generations of the rule-110 cellular automaton given as a co-Kleisli arrow of the [[Store Comonad]].

> Sources: DaoFP Exercise 16.3.1.

**Solution:** [[DaoFP Chapter 16 Solutions#Solution 16.3.1|Solution 16.3.1]]
