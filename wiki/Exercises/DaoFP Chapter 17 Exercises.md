#exercise

Exercises from DaoFP, Chapter 17. Solutions: [[DaoFP Chapter 17 Solutions]]. Index: [[Map of Content]].

## Exercise 17.1.1

Show that there is a functor from a [[Collage]] of two categories to the [[Walking Arrow]] category $0 \to 1$.

> Sources: DaoFP Exercise 17.1.1.

**Solution:** [[DaoFP Chapter 17 Solutions#Solution 17.1.1|Solution 17.1.1]]

## Exercise 17.1.2

Show that if there is a functor $F : \mathcal{E} \to \mathbf{2}$ (the [[Walking Arrow]]) then $\mathcal{E}$ splits into a [[Collage]] of two categories.

> Sources: DaoFP Exercise 17.1.2.

**Solution:** [[DaoFP Chapter 17 Solutions#Solution 17.1.2|Solution 17.1.2]]

## Exercise 17.2.1

Verify that for an extranatural transformation $P \to \Delta_d$ the first extranaturality diamond is the cowedge condition and the second is trivial ([[Coend]]).

> Sources: DaoFP Exercise 17.2.1.

**Solution:** [[DaoFP Chapter 17 Solutions#Solution 17.2.1|Solution 17.2.1]]

## Exercise 17.2.2

Define a `Profunctor` instance for `newtype ProPair q p a b x y = ProPair (q a y, p x b)` keeping `a b` fixed ([[Coend]]).

> Sources: DaoFP Exercise 17.2.2.

**Solution:** [[DaoFP Chapter 17 Solutions#Solution 17.2.2|Solution 17.2.2]]

## Exercise 17.2.3

Define a `Profunctor` instance for `newtype CoEndCompose p q a b = CoEndCompose (Coend (ProPair q p a b))` ([[Coend]], profunctor composition).

> Sources: DaoFP Exercise 17.2.3.

**Solution:** [[DaoFP Chapter 17 Solutions#Solution 17.2.3|Solution 17.2.3]]

## Exercise 17.3.1

Show that a [[Product]] (a limit over the two-object discrete category $\mathbf{2}$) can be defined as an [[End]].

> Sources: DaoFP Exercise 17.3.1.

**Solution:** [[DaoFP Chapter 17 Solutions#Solution 17.3.1|Solution 17.3.1]]

## Exercise 17.6.1

Prove the contravariant co-Yoneda lemma $\int^{x} \mathcal{C}(a, x) \times G x \cong G a$ for a presheaf $G$ ([[Ninja Yoneda Lemma]]).

> Sources: DaoFP Exercise 17.6.1.

**Solution:** [[DaoFP Chapter 17 Solutions#Solution 17.6.1|Solution 17.6.1]]

## Exercise 17.7.1

Define the `Functor` instance for [[Day Convolution|`Day`]].

> Sources: DaoFP Exercise 17.7.1.

**Solution:** [[DaoFP Chapter 17 Solutions#Solution 17.7.1|Solution 17.7.1]]

## Exercise 17.7.2

Implement the associator `assoc :: Day f (Day g h) x -> Day (Day f g) h x` ([[Day Convolution]]).

> Sources: DaoFP Exercise 17.7.2.

**Solution:** [[DaoFP Chapter 17 Solutions#Solution 17.7.2|Solution 17.7.2]]

## Exercise 17.7.3

Define the `Functor` instance for the free applicative `FreeA` ([[Day Convolution]]).

> Sources: DaoFP Exercise 17.7.3.

**Solution:** [[DaoFP Chapter 17 Solutions#Solution 17.7.3|Solution 17.7.3]]

## Exercise 17.9.1

Show that $\mathcal{C}(s, y \times a) \times \mathcal{C}(x \times b, t)$ is a [[Profunctor]] in $\langle x, y\rangle$ ([[Existential Lens]]).

> Sources: DaoFP Exercise 17.9.1.

**Solution:** [[DaoFP Chapter 17 Solutions#Solution 17.9.1|Solution 17.9.1]]
