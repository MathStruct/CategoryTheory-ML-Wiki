#exercise

Exercises from Category Theory for Scientists (CTfS), Chapter 5 ("Categories at work"). CTfS gives no solutions; the solutions here are the wiki's own. This is a selection: the exercises referenced from concept notes. Solutions: [[CTfS Chapter 5 Solutions]]. Index: [[Map of Content]].

## Exercise 5.1.1.3

Let $X = \{a, b, c\}$, $M = (\mathbb N, 1, \ast)$ the multiplicative monoid of natural numbers, and $f : X \to \mathbb N$ with $f(a) = 7$, $f(b) = 2$, $f(c) = 2$. Let $\beta_{X,M} : \mathrm{Hom}_{\mathbf{Set}}(X, U(M)) \to \mathrm{Hom}_{\mathbf{Mon}}(\mathrm{List}(X), M)$ be the adjunction bijection. What is $\beta_{X,M}(f)([b, b, a, c])$?

> CTfS §5.1.1; context: [[Free-Forgetful Adjunction]], [[Map of Content]].

> Sources: CTfS, Exercise 5.1.1.3.

**Solution:** [[CTfS Chapter 5 Solutions#Solution 5.1.1.3|Solution 5.1.1.3]]

## Exercise 5.1.1.6

Let $U : \mathbf{Grph} \to \mathbf{Set}$ send a graph to its set of vertices. This functor has both a left and a right adjoint. What are they?

> CTfS §5.1.1; context: [[Adjunction]], [[Map of Content]].

> Sources: CTfS, Exercise 5.1.1.6.

**Solution:** [[CTfS Chapter 5 Solutions#Solution 5.1.1.6|Solution 5.1.1.6]]

## Exercise 5.1.1.9

The discrete category functor $\mathrm{Disc} : \mathbf{Set} \to \mathbf{Cat}$ has a left adjoint $p : \mathbf{Cat} \to \mathbf{Set}$. Describe it. (Hint: look at the mate isomorphism at $[1] \in \mathbf{Cat}$ and $\underline 2 \in \mathbf{Set}$.)

> CTfS §5.1.1; context: [[Adjunction]], [[Map of Content]].

> Sources: CTfS, Exercise 5.1.1.9.

**Solution:** [[CTfS Chapter 5 Solutions#Solution 5.1.1.9|Solution 5.1.1.9]]

## Exercise 5.1.4.5

Let $[1] = (0 \xrightarrow{f} 1)$, $[2] = (0 \xrightarrow{g} 1 \xrightarrow{h} 2)$ and $F : [1] \to [2]$ with $0 \mapsto 0$, $1 \mapsto 2$.

- **a.** How many possibilities are there for $F(f)$?
- **b.** Let $I : [2] \to \mathbf{Set}$ be the instance with table 0 (word ↦ $g$): Am ↦ To be verb, Baltimore ↦ Place, Carla ↦ Person, Develop ↦ Action verb, Edward ↦ Person, Foolish ↦ Adjective, Green ↦ Adjective; table 1 ($h$): Action verb ↦ Verb, Adjective ↦ Adjective, Place ↦ Noun, Person ↦ Noun, To be verb ↦ Verb; table 2: Adjective, Noun, Verb. Write out the two tables of $\Delta_F(I)$.

> CTfS §5.1.4; context: [[Data Migration Functor]], [[Map of Content]].

> Sources: CTfS, Exercise 5.1.4.5.

**Solution:** [[CTfS Chapter 5 Solutions#Solution 5.1.4.5|Solution 5.1.4.5]]

## Exercise 5.1.4.8

Let $F : \underline 3 \to \underline 2$ (discrete categories) send $1 \mapsto 1$, $2 \mapsto 2$, $3 \mapsto 2$.

- **a.** Write down an instance $I : \underline 3 \to \mathbf{Set}$.
- **b.** Given that "$\Sigma_F$ performs a parameterized colimit", guess $\Sigma_F(I)$ as two sets made from the three sets you wrote down.

> CTfS §5.1.4; context: [[Data Migration Functor]], [[Map of Content]].

> Sources: CTfS, Exercise 5.1.4.8.

**Solution:** [[CTfS Chapter 5 Solutions#Solution 5.1.4.8|Solution 5.1.4.8]]

## Exercise 5.1.4.11

With $F : \underline 3 \to \underline 2$ as in Exercise 5.1.4.8:

- **a.** Write down an instance $I : \underline 3 \to \mathbf{Set}$.
- **b.** Given that "$\Pi_F$ performs a parameterized limit", guess $\Pi_F(I)$ as two sets made from the three sets.

> CTfS §5.1.4; context: [[Data Migration Functor]], [[Map of Content]].

> Sources: CTfS, Exercise 5.1.4.11.

**Solution:** [[CTfS Chapter 5 Solutions#Solution 5.1.4.11|Solution 5.1.4.11]]

## Exercise 5.2.3.3

- **a.** Come up with 4 overlapping open subsets covering the square $X = [0, 3] \times [0, 3]$; label each open set and each overlap, and draw the preorder $\mathrm{Op}(X)$ of these regions under inclusion. Make up formulas $R_1, R_2 : X \to \mathbb R$ with $R_1 \leq R_2$, a range of temperatures that can exist at each point.
- **b.** Define a presheaf $\mathcal O$ by $\mathcal O(A) = \{f : A \to \mathbb R \mid R_1(a) \leq f(a) \leq R_2(a)\}$. What are the restriction maps? Do you like the name "value-assignment throughout $A$"?
- **c.** Define $\mathcal O'(A) = \{f \in \mathcal O(A) \mid f \text{ continuous}\}$. Is there a morphism of presheaves $\mathcal O' \to \mathcal O$?

> CTfS §5.2.3; context: [[Map of Content]], [[Sheaf]].

> Sources: CTfS, Exercise 5.2.3.3.

**Solution:** [[CTfS Chapter 5 Solutions#Solution 5.2.3.3|Solution 5.2.3.3]]

## Exercise 5.3.2.5

Let $E$ be a set of "exceptions" (like "overflow!", "division by zero!"). Let $T : \mathbf{Set} \to \mathbf{Set}$ be $X \mapsto X \sqcup E$. Following Example 5.3.2.4 (the Maybe monad), find a unit $\eta$ and multiplication $\mu$ making $(T, \eta, \mu)$ a monad.

> CTfS §5.3.2; context: [[Map of Content]], [[Maybe Monad]].

> Sources: CTfS, Exercise 5.3.2.5.

**Solution:** [[CTfS Chapter 5 Solutions#Solution 5.3.2.5|Solution 5.3.2.5]]

## Exercise 5.3.3.5

Let $J = (\mathcal P, \eta, \mu)$ be the power set monad.

- **a.** Given a morphism $f : A \to B$ in $\mathrm{Kls}(J)$ (a function $A \to \mathcal P(B)$), is there a natural way to associate a relation $R \subseteq A \times B$?
- **b.** How does composition in $\mathrm{Kls}(J)$ relate to composition of relations?

> CTfS §5.3.3; context: [[Map of Content]], [[Power Set Monad]].

> Sources: CTfS, Exercise 5.3.3.5.

**Solution:** [[CTfS Chapter 5 Solutions#Solution 5.3.3.5|Solution 5.3.3.5]]

## Exercise 5.3.3.6

Let $J$ be the power set monad. $\mathrm{Kls}(J)$ has binary products. What is the product of $A = \{1, 2, 3\}$ and $B = \{a, b\}$?

> CTfS §5.3.3; context: [[Map of Content]], [[Power Set Monad]].

> Sources: CTfS, Exercise 5.3.3.6.

**Solution:** [[CTfS Chapter 5 Solutions#Solution 5.3.3.6|Solution 5.3.3.6]]

## Exercise 5.3.3.7

Let $J$ be the power set monad. $\mathrm{Kls}(J)$ has binary coproducts. What is the coproduct of $A = \{1, 2, 3\}$ and $B = \{a, b\}$?

> CTfS §5.3.3; context: [[Map of Content]], [[Power Set Monad]].

> Sources: CTfS, Exercise 5.3.3.7.

**Solution:** [[CTfS Chapter 5 Solutions#Solution 5.3.3.7|Solution 5.3.3.7]]

## Exercise 5.4.1.4

Consider an operad $\mathcal O$ like the little squares operad but with three objects: square, circle, equilateral triangle. A morphism is a positioning of non-overlapping shapes inside a shape.

- **a.** Draw an example of a morphism $f$ from two circles and a square to a triangle.
- **b.** Find three other morphisms that compose into $f$, and draw the composite.

> CTfS §5.4.1; context: [[Map of Content]], [[Operad]].

> Sources: CTfS, Exercise 5.4.1.4.

**Solution:** [[CTfS Chapter 5 Solutions#Solution 5.4.1.4|Solution 5.4.1.4]]
