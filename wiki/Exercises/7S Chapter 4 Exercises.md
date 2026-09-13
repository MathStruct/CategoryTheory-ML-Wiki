#exercise

Exercises from 7 Sketches, Chapter 4. Solutions: [[7S Chapter 4 Solutions]]. Index: [[Map of Content]].

## Exercise 4.4

Let $X = \{\mathsf{monoid}, \mathsf{preorder} \leq \mathsf{category}\}$ and $Y = \{\mathsf{nothing} \leq \mathsf{this\ book}\}$. 1. Draw the Hasse diagram of $X^{\mathrm{op}} \times Y$. 2. Give a profunctor $\Lambda : X \nrightarrow Y$, reading $\Lambda(x, y) = \mathsf{true}$ as "my aunt can explain an $x$ given $y$", and interpret the fact that $\Lambda^{-1}(\mathsf{true})$ is an [[Upper Set]].

> Sources: 7 Sketches, Exercise 4.4 and Solution A.4.

**Solution:** [[7S Chapter 4 Solutions#Solution 4.4|Solution 4.4]]

## Exercise 4.7

Show that $\Rightarrow$ (with $\mathsf{true} \Rightarrow \mathsf{false} = \mathsf{false}$ and $\mathsf{true}$ otherwise) satisfies $b \wedge c \leq d$ iff $b \leq (c \Rightarrow d)$.

> Sources: 7 Sketches, Exercise 4.7 and Solution A.4.

**Solution:** [[7S Chapter 4 Solutions#Solution 4.7|Solution 4.7]]

## Exercise 4.9

Show that a $\mathcal{V}$-[[Profunctor]] $\Phi : \mathcal{X}^{\mathrm{op}} \times \mathcal{Y} \to \mathcal{V}$ is the same as a function $\Phi : \mathrm{Ob}(\mathcal{X}) \times \mathrm{Ob}(\mathcal{Y}) \to V$ with $\mathcal{X}(x', x) \otimes \Phi(x, y) \otimes \mathcal{Y}(y, y') \leq \Phi(x', y')$.

> Sources: 7 Sketches, Exercise 4.9 and Solution A.4.

**Solution:** [[7S Chapter 4 Solutions#Solution 4.9|Solution 4.9]]

## Exercise 4.10

Is a $\mathbf{Bool}$-profunctor exactly a [[Feasibility Relation]]?

> Sources: 7 Sketches, Exercise 4.10 and Solution A.4.

**Solution:** [[7S Chapter 4 Solutions#Solution 4.10|Solution 4.10]]

## Exercise 4.12

Fill in the feasibility matrix of the bridge profunctor $\Phi : X \nrightarrow Y$ of Example 4.11.

> Sources: 7 Sketches, Exercise 4.12 and Solution A.4.

**Solution:** [[7S Chapter 4 Solutions#Solution 4.12|Solution 4.12]]

## Exercise 4.15

Fill in the [[Cost]]-matrix of the bridge profunctor of Example 4.13.

> Sources: 7 Sketches, Exercise 4.15 and Solution A.4.

**Solution:** [[7S Chapter 4 Solutions#Solution 4.15|Solution 4.15]]

## Exercise 4.17

Compute $M_X^3 \ast M_\Phi \ast M_Y^2$ with min-plus multiplication and compare with [[7S Chapter 4 Exercises#Exercise 4.15|7S Exercise 4.15]].

> Sources: 7 Sketches, Exercise 4.17 and Solution A.4.

**Solution:** [[7S Chapter 4 Solutions#Solution 4.17|Solution 4.17]]

## Exercise 4.18

The node $(\mathsf{g/n}, \mathsf{funny})$ has no bridge out of it. Valid? Meaning?

> Sources: 7 Sketches, Exercise 4.18 and Solution A.4.

**Solution:** [[7S Chapter 4 Solutions#Solution 4.18|Solution 4.18]]

## Exercise 4.22

Fill in the composite $\Phi \mathbin{;} \Psi : X \nrightarrow Z$ of the two Cost-profunctors shown.

> Sources: 7 Sketches, Exercise 4.22 and Solution A.4.

**Solution:** [[7S Chapter 4 Solutions#Solution 4.22|Solution 4.22]]

## Exercise 4.26

Draw a bridge diagram for the unit profunctor $U_X$ of a Cost-category $X$.

> Sources: 7 Sketches, Exercise 4.26 and Solution A.4.

**Solution:** [[7S Chapter 4 Solutions#Solution 4.26|Solution 4.26]]

## Exercise 4.30

Justify the steps in Eqs. (4.28)–(4.29) of Lemma 4.27, and show they are equalities when $\mathcal{V} = \mathbf{Bool}$.

> Sources: 7 Sketches, Exercise 4.30 and Solution A.4.

**Solution:** [[7S Chapter 4 Solutions#Solution 4.30|Solution 4.30]]

## Exercise 4.32

Prove associativity of profunctor composition (Lemma 4.31).

> Sources: 7 Sketches, Exercise 4.32 and Solution A.4.

**Solution:** [[7S Chapter 4 Solutions#Solution 4.32|Solution 4.32]]

## Exercise 4.36

Check that the [[Companion and Conjoint|companion]] of $\mathrm{id} : P \to P$ is the unit profunctor.

> Sources: 7 Sketches, Exercise 4.36 and Solution A.4.

**Solution:** [[7S Chapter 4 Solutions#Solution 4.36|Solution 4.36]]

## Exercise 4.38

What is the conjoint $\check{+}$ of $+ : \mathbb{R}^3 \to \mathbb{R}$?

> Sources: 7 Sketches, Exercise 4.38 and Solution A.4.

**Solution:** [[7S Chapter 4 Solutions#Solution 4.38|Solution 4.38]]

## Exercise 4.41

For a skeletal quantale $\mathcal{V}$: 1. show $\mathcal{V}$-functors $F, G$ are $\mathcal{V}$-adjoint iff $\hat F = \check G$; 2. deduce $\widehat{\mathrm{id}} = \check{\mathrm{id}}$.

> Sources: 7 Sketches, Exercise 4.41 and Solution A.4.

**Solution:** [[7S Chapter 4 Solutions#Solution 4.41|Solution 4.41]]

## Exercise 4.44

Draw the Hasse diagram of the [[Collage]] of the Cost-profunctor of Example 4.13.

> Sources: 7 Sketches, Exercise 4.44 and Solution A.4.

**Solution:** [[7S Chapter 4 Solutions#Solution 4.44|Solution 4.44]]

## Exercise 4.48

Check that a [[Symmetric Monoidal Preorder]] is a [[Monoidal Category]] with at most one morphism between any two objects.

> Sources: 7 Sketches, Exercise 4.48 and Solution A.4.

**Solution:** [[7S Chapter 4 Solutions#Solution 4.48|Solution 4.48]]

## Exercise 4.50

In $(\mathbf{Set}, 1, \times)$ with $A = B = C = D = F = G = \mathbb{Z}$, $E = \mathbb{B}$, $f_C(a) = |a|$, $f_D(a) = 5a$, $g_E(d, b) = [d \leq b]$, $g_F(d, b) = d - b$, $h(c, e) = $ if $e$ then $c$ else $1 - c$: compute the listed values and the composite $q : A \times B \to G \times F$.

> Sources: 7 Sketches, Exercise 4.50 and Solution A.4.

**Solution:** [[7S Chapter 4 Solutions#Solution 4.50|Solution 4.50]]

## Exercise 4.52

Does Rough Definition 4.51 with $\mathcal{V} = (\mathbf{Set}, 1, \times)$ agree with the definition of [[Category]]?

> Sources: 7 Sketches, Exercise 4.52 and Solution A.4.

**Solution:** [[7S Chapter 4 Solutions#Solution 4.52|Solution 4.52]]

## Exercise 4.54

What are identity elements in [[Lawvere Metric Space|Cost-categories]]?

> Sources: 7 Sketches, Exercise 4.54 and Solution A.4.

**Solution:** [[7S Chapter 4 Solutions#Solution 4.54|Solution 4.54]]

## Exercise 4.62

For $\underline{3}$ in [[Corelation|$\mathbf{Corel}$]]: draw the unit $\varnothing \to \underline{3} \sqcup \underline{3}$, the counit $\underline{3} \sqcup \underline{3} \to \varnothing$, and check a snake equation.

> Sources: 7 Sketches, Exercise 4.62 and Solution A.4.

**Solution:** [[7S Chapter 4 Solutions#Solution 4.62|Solution 4.62]]

## Exercise 4.64

Interpret monoidal products in $\mathbf{Feas}$ in terms of feasibility.

> Sources: 7 Sketches, Exercise 4.64 and Solution A.4.

**Solution:** [[7S Chapter 4 Solutions#Solution 4.64|Solution 4.64]]

## Exercise 4.65

What are the isomorphisms $X \times \mathbf{1} \cong X$ and $\mathbf{1} \times X \cong X$ in $\mathbf{Prof}_{\mathcal{V}}$?

> Sources: 7 Sketches, Exercise 4.65 and Solution A.4.

**Solution:** [[7S Chapter 4 Solutions#Solution 4.65|Solution 4.65]]

## Exercise 4.66

Check the snake equations for $\eta_X(1, x, x') = X(x,x')$ and $\varepsilon_X(x, x', 1) = X(x, x')$ in $\mathbf{Prof}_{\mathcal{V}}$.

> Sources: 7 Sketches, Exercise 4.66 and Solution A.4.

**Solution:** [[7S Chapter 4 Solutions#Solution 4.66|Solution 4.66]]
