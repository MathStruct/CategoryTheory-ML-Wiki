#exercise

Exercises from DaoFP, Chapter 14. Solutions: [[DaoFP Chapter 14 Solutions]]. Index: [[Map of Content]].

## Exercise 14.4.1

Define `Functor` and `Monad` instances for `newtype E e a = E (e -> Maybe a)` ([[Reader Monad]] combined with [[Maybe Monad]]).

> Sources: DaoFP Exercise 14.4.1.

**Solution:** [[DaoFP Chapter 14 Solutions#Solution 14.4.1|Solution 14.4.1]]

## Exercise 14.5.1

Implement `ap :: Monad m => m (a -> b) -> m a -> m b` using [[Do Notation]].

> Sources: DaoFP Exercise 14.5.1.

**Solution:** [[DaoFP Chapter 14 Solutions#Solution 14.5.1|Solution 14.5.1]]

## Exercise 14.5.2

Rewrite `pairs` ([[List Monad]]) using bind operators and lambdas.

> Sources: DaoFP Exercise 14.5.2.

**Solution:** [[DaoFP Chapter 14 Solutions#Solution 14.5.2|Solution 14.5.2]]

## Exercise 14.8.1

Implement conversions between the rose tree `data Rose a = Leaf a | Rose [Rose a]` and `FreeMonad [] a` ([[Free Monad]]).

> Sources: DaoFP Exercise 14.8.1.

**Solution:** [[DaoFP Chapter 14 Solutions#Solution 14.8.1|Solution 14.8.1]]

## Exercise 14.8.2

Implement conversions between a non-empty binary tree and `FreeMonad Bin a` with `data Bin a = Bin a a` ([[Free Monad]]).

> Sources: DaoFP Exercise 14.8.2.

**Solution:** [[DaoFP Chapter 14 Solutions#Solution 14.8.2|Solution 14.8.2]]

## Exercise 14.8.3

Implement a pretty printer for programs in the stack-calculator [[Free Monad]], using an algebra with carrier `Const String`.

> Sources: DaoFP Exercise 14.8.3.

**Solution:** [[DaoFP Chapter 14 Solutions#Solution 14.8.3|Solution 14.8.3]]

## Exercise 14.9.1

Implement the `Monoidal` instance for the list functor ([[Monoidal Functor]], [[Applicative Functor]]).

> Sources: DaoFP Exercise 14.9.1.

**Solution:** [[DaoFP Chapter 14 Solutions#Solution 14.9.1|Solution 14.9.1]]

## Exercise 14.9.2

Implement `liftA3` for an [[Applicative Functor]].

> Sources: DaoFP Exercise 14.9.2.

**Solution:** [[DaoFP Chapter 14 Solutions#Solution 14.9.2|Solution 14.9.2]]

## Exercise 14.9.3

Verify the [[Applicative Functor|applicative]] laws for the zip instance of lists (`pure = repeat`, `fs <*> as = zipWith ($) fs as`).

> Sources: DaoFP Exercise 14.9.3.

**Solution:** [[DaoFP Chapter 14 Solutions#Solution 14.9.3|Solution 14.9.3]]
