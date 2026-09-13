#exercise

Exercises from DaoFP, Chapter 9. Solutions: [[DaoFP Chapter 9 Solutions]]. Index: [[Map of Content]].

## Exercise 9.3.1

Prove naturality of the vertical composite $\gamma = \beta \cdot \alpha$: $\gamma_y \circ Ff = Hf \circ \gamma_x$.

> Sources: DaoFP Exercise 9.3.1.

**Solution:** [[DaoFP Chapter 9 Solutions#Solution 9.3.1|Solution 9.3.1]]

## Exercise 9.3.2

Implement two versions of the horizontal composition of `safeHead` after `reverse` and compare.

> Sources: DaoFP Exercise 9.3.2.

**Solution:** [[DaoFP Chapter 9 Solutions#Solution 9.3.2|Solution 9.3.2]]

## Exercise 9.3.3

Same for `reverse` after `safeHead`: `reverse . fmap safeHead` vs `fmap safeHead . reverse` of type `[[a]] -> [Maybe a]`.

> Sources: DaoFP Exercise 9.3.3.

**Solution:** [[DaoFP Chapter 9 Solutions#Solution 9.3.3|Solution 9.3.3]]

## Exercise 9.5.1

Show the limit of a diagram of shape the [[Walking Arrow]] ($D_1 \xrightarrow{D f} D_2$) has the same elements as $D_1$.

> Sources: DaoFP Exercise 9.5.1.

**Solution:** [[DaoFP Chapter 9 Solutions#Solution 9.5.1|Solution 9.5.1]]

## Exercise 9.5.2

Check that `(h q') . q == q'` for `q' x = testBit x 0` (with `q n = even n`, `h q' True = q' 0`, `h q' False = q' 1`).

> Sources: DaoFP Exercise 9.5.2.

**Solution:** [[DaoFP Chapter 9 Solutions#Solution 9.5.2|Solution 9.5.2]]

## Exercise 9.6.1

Fill the gap in the Yoneda proof when $F a = \varnothing$.

> Sources: DaoFP Exercise 9.6.1.

**Solution:** [[DaoFP Chapter 9 Solutions#Solution 9.6.1|Solution 9.6.1]]

## Exercise 9.6.2

Show that $\alpha_x(h) := (F h)(p)$ is natural in $x$.

> Sources: DaoFP Exercise 9.6.2.

**Solution:** [[DaoFP Chapter 9 Solutions#Solution 9.6.2|Solution 9.6.2]]

## Exercise 9.6.3

Derive $\alpha_x(h) = (Fh)(p)$ from $\alpha_a(\mathrm{id}_a) = p$ and naturality.

> Sources: DaoFP Exercise 9.6.3.

**Solution:** [[DaoFP Chapter 9 Solutions#Solution 9.6.3|Solution 9.6.3]]

## Exercise 9.8.1

Describe [[Limit|limits]] and [[Colimit|colimits]] as representing objects.

> Sources: DaoFP Exercise 9.8.1.

**Solution:** [[DaoFP Chapter 9 Solutions#Solution 9.8.1|Solution 9.8.1]]

## Exercise 9.8.2

The singleton functor $F(c) = \{c\}$ (on arrows, the unique map between singletons): show $F$ representable iff $\mathcal{C}$ has an [[Initial Object]].

> Sources: DaoFP Exercise 9.8.2.

**Solution:** [[DaoFP Chapter 9 Solutions#Solution 9.8.2|Solution 9.8.2]]

## Exercise 9.8.3

Implement `Representable` for `data Pair x = Pair x x`.

> Sources: DaoFP Exercise 9.8.3.

**Solution:** [[DaoFP Chapter 9 Solutions#Solution 9.8.3|Solution 9.8.3]]

## Exercise 9.8.4

Is the constant functor to the terminal object representable? Implement `Representable` for `data Unit a = U`.

> Sources: DaoFP Exercise 9.8.4.

**Solution:** [[DaoFP Chapter 9 Solutions#Solution 9.8.4|Solution 9.8.4]]

## Exercise 9.8.5

The list functor is not representable; can it be considered a sum of representables?

> Sources: DaoFP Exercise 9.8.5.

**Solution:** [[DaoFP Chapter 9 Solutions#Solution 9.8.5|Solution 9.8.5]]
