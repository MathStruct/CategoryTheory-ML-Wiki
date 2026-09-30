#exercise

Exercises from Category Theory for Scientists (CTfS), Chapter 2 ("The category of sets"). CTfS gives no solutions; the solutions here are the wiki's own. This is a selection: the exercises referenced from concept notes. Solutions: [[CTfS Chapter 2 Solutions]]. Index: [[Map of Content]].

## Exercise 2.1.2.2

A simplified account of how the brain receives light: the eye contains about 100 million photoreceptor (PR) cells, each connected to a retinal ganglion (RG) cell. No PR cell connects to two different RG cells, but usually many PR cells attach to a single RG cell.

- **a.** Does the connection pattern constitute a function $RG \to PR$, a function $PR \to RG$, or neither?
- **b.** Would you guess that the connection patterns between other areas of the brain are "function-like"?

> CTfS §2.1.2; context: [[Function]], [[Map of Content]].

> Sources: CTfS, Exercise 2.1.2.2.

**Solution:** [[CTfS Chapter 2 Solutions#Solution 2.1.2.2|Solution 2.1.2.2]]

## Exercise 2.1.2.5

Let $A = \{1, 2, 3, 4, 5\}$ and $B = \{x, y\}$.

- **a.** How many elements does $\mathrm{Hom}_{\mathbf{Set}}(A, B)$ have?
- **b.** How many elements does $\mathrm{Hom}_{\mathbf{Set}}(B, A)$ have?

> CTfS §2.1.2; context: [[Function]], [[Map of Content]].

> Sources: CTfS, Exercise 2.1.2.5.

**Solution:** [[CTfS Chapter 2 Solutions#Solution 2.1.2.5|Solution 2.1.2.5]]

## Exercise 2.1.2.10

Let $n \in \mathbb N$ and let $X$ be a set with exactly $n$ elements.

- **a.** How many isomorphisms are there from $X$ to itself?
- **b.** Does your formula from part a hold when $n = 0$?

> CTfS §2.1.2; context: [[Cardinality]], [[Map of Content]].

> Sources: CTfS, Exercise 2.1.2.10.

**Solution:** [[CTfS Chapter 2 Solutions#Solution 2.1.2.10|Solution 2.1.2.10]]

## Exercise 2.1.2.13

Find a set $A$ such that for any set $X$ there is an isomorphism of sets $X \cong \mathrm{Hom}_{\mathbf{Set}}(A, X)$. (Hint: draw a picture of proposed $A$'s and $X$'s.)

> CTfS §2.1.2; context: [[Global Element]], [[Map of Content]].

> Sources: CTfS, Exercise 2.1.2.13.

**Solution:** [[CTfS Chapter 2 Solutions#Solution 2.1.2.13|Solution 2.1.2.13]]

## Exercise 2.4.1.15

- **a.** Let $X, Y$ be sets. Construct the "swap map" $s : X \times Y \to Y \times X$ using only the universal property for products. Write $s$ in terms of the projections $\pi_1, \pi_2$ and the symbols $\times$, $\circ$ (or $\langle -, - \rangle$).
- **b.** Can you prove that $s$ is an isomorphism using only the universal property for product?

> CTfS §2.4.1; context: [[Map of Content]], [[Product]].

> Sources: CTfS, Exercise 2.4.1.15.

**Solution:** [[CTfS Chapter 2 Solutions#Solution 2.4.1.15|Solution 2.4.1.15]]

## Exercise 2.4.2.13

Understand Example 2.4.2.12 (the coproduct of "an animal that can fly" and "an animal that can swim" counts ducks twice) and see if a similar idea makes sense for particles and waves. Make an [[Olog]], choosing your wording according to the olog rules. How do photons, which exhibit properties of both waves and particles, fit into the coproduct in your olog?

> CTfS §2.4.2; context: [[Coproduct]], [[Map of Content]].

> Sources: CTfS, Exercise 2.4.2.13.

**Solution:** [[CTfS Chapter 2 Solutions#Solution 2.4.2.13|Solution 2.4.2.13]]

## Exercise 2.5.1.3

- **a.** Draw a set $X$ with five elements and a set $Y$ with three elements. Colour each element of $X$ and of $Y$ red, blue or yellow in a "random-looking" way. Regarding the colourings as functions $X \to C$ and $Y \to C$ with $C = \{\text{red}, \text{blue}, \text{yellow}\}$, draw the fiber product $X \times_C Y$, coloured appropriately.
- **b.** The universal property for products gives a function $X \times_C Y \to X \times Y$, which is an injection. Draw the $5 \times 3$ grid and indicate this subset.

> CTfS §2.5.1; context: [[Map of Content]], [[Pullback]].

> Sources: CTfS, Exercise 2.5.1.3.

**Solution:** [[CTfS Chapter 2 Solutions#Solution 2.5.1.3|Solution 2.5.1.3]]

## Exercise 2.5.1.5

Given $f : X \to Z$ and $g : Y \to Z$:

- **a.** Suppose $Y = \varnothing$; what can you say about $X \times_Z Y$?
- **b.** Suppose instead $Y$ is any set but $Z$ has exactly one element; what can you say about $X \times_Z Y$?

> CTfS §2.5.1; context: [[Finite Limits in Set]], [[Map of Content]].

> Sources: CTfS, Exercise 2.5.1.5.

**Solution:** [[CTfS Chapter 2 Solutions#Solution 2.5.1.5|Solution 2.5.1.5]]

## Exercise 2.5.1.6

Let $S = \mathbb R^3$ (space), $T = \mathbb R$ (time), with the origin of $S \times T$ the centre of mass of MIT at its founding. Let $Y = S \times T$ with projections $g_1 : Y \to S$, $g_2 : Y \to T$, and let $X = \{\ast\}$ with $f_1 : X \to S$, $f_2 : X \to T$ both picking the origin.

- **a.** What are the fiber products $W_1 = X \times_S Y$ (along $f_1, g_1$) and $W_2 = X \times_T Y$ (along $f_2, g_2$)?
- **b.** Interpret these sets in terms of the centre of mass of MIT at its founding.

> CTfS §2.5.1; context: [[Finite Limits in Set]], [[Map of Content]].

> Sources: CTfS, Exercise 2.5.1.6.

**Solution:** [[CTfS Chapter 2 Solutions#Solution 2.5.1.6|Solution 2.5.1.6]]

## Exercise 2.5.1.10

For each of the following, an author proposes that the square is a pullback. Is the label on the upper-left box reasonable given the rest of the olog, or suspect?

- **a.** "a person" $\xrightarrow{\text{has as favourite colour}}$ "a colour" $\xleftarrow{\text{is}}$ "blue"; proposed pullback: "a person whose favourite colour is blue".
- **b.** "a dog" $\xrightarrow{\text{has as owner}}$ "a person" $\xleftarrow{\text{is}}$ "a woman"; proposed pullback: "a dog whose owner is a woman".
- **c.** "a space in our house" $\xrightarrow{\text{has}}$ "a width" $\xleftarrow{\text{has}}$ "a piece of furniture"; proposed pullback: "a good fit".

> CTfS §2.5.1; context: [[Map of Content]], [[Pullback]].

> Sources: CTfS, Exercise 2.5.1.10.

**Solution:** [[CTfS Chapter 2 Solutions#Solution 2.5.1.10|Solution 2.5.1.10]]

## Exercise 2.5.3.3

Come up with an olog that uses equalizers in a reasonably interesting way. Alternatively, use an equalizer to specify those published authors who have published exactly one paper. (Hint: find a function from authors to papers; then find another.)

> CTfS §2.5.3; context: [[Equalizer]], [[Map of Content]].

> Sources: CTfS, Exercise 2.5.3.3.

**Solution:** [[CTfS Chapter 2 Solutions#Solution 2.5.3.3|Solution 2.5.3.3]]

## Exercise 2.6.1.3

Let $X$ be the set of people, and let $(x, y) \in R$ if $x$ spends a lot of time thinking about $y$. Is $R$ (a) reflexive, (b) symmetric, (c) transitive?

> CTfS §2.6.1; context: [[Equivalence Relation]], [[Map of Content]].

> Sources: CTfS, Exercise 2.6.1.3.

**Solution:** [[CTfS Chapter 2 Solutions#Solution 2.6.1.3|Solution 2.6.1.3]]

## Exercise 2.6.1.5

Let $f : X \to B$ be a function and define $R = \{(x, y) \mid f(x) = f(y)\} \subseteq X \times X$. Is $R$ an equivalence relation?

- **a.** Are all equivalence relations on $X$ obtainable in this way (as the fibers of some function with domain $X$)?
- **b.** Does this viewpoint relate to that of Example 2.6.1.4 (partitions)?

> CTfS §2.6.1; context: [[Equivalence Relation]], [[Map of Content]].

> Sources: CTfS, Exercise 2.6.1.5.

**Solution:** [[CTfS Chapter 2 Solutions#Solution 2.6.1.5|Solution 2.6.1.5]]

## Exercise 2.6.1.10

Let $N$ be a network (graph) with node set $X$, and let $R \subseteq X \times X$ be the relation "there is an edge connecting $x$ and $y$".

- **a.** What is the equivalence relation $\sim$ generated by $R$?
- **b.** What is the quotient $X/\!\sim$?

> CTfS §2.6.1; context: [[Equivalence Relation]], [[Map of Content]].

> Sources: CTfS, Exercise 2.6.1.10.

**Solution:** [[CTfS Chapter 2 Solutions#Solution 2.6.1.10|Solution 2.6.1.10]]

## Exercise 2.6.2.6

Let $W = \mathbb N$, $X = \mathbb Z$, $Y = \{\ast\}$. Define $f : W \to X$ by $f(w) = -(w + 1)$ and let $g : W \to Y$ be the unique map. Describe the pushout $X \sqcup_W Y$.

> CTfS §2.6.2; context: [[Finite Colimits in Set]], [[Map of Content]].

> Sources: CTfS, Exercise 2.6.2.6.

**Solution:** [[CTfS Chapter 2 Solutions#Solution 2.6.2.6|Solution 2.6.2.6]]

## Exercise 2.6.2.7

Let $i : R \subseteq X \times X$ be an equivalence relation, with maps $\pi_1 \circ i, \pi_2 \circ i : R \to X$.

- **a.** What is the pushout of $X \xleftarrow{\pi_1 \circ i} R \xrightarrow{\pi_2 \circ i} X$?
- **b.** If $R$ is not assumed to be an equivalence relation, we can still form this pushout. How does it relate to the equivalence relation generated by $R$?

> CTfS §2.6.2; context: [[Equivalence Relation]], [[Map of Content]], [[Pushout]].

> Sources: CTfS, Exercise 2.6.2.7.

**Solution:** [[CTfS Chapter 2 Solutions#Solution 2.6.2.7|Solution 2.6.2.7]]

## Exercise 2.6.3.2

Let $X = \mathbb R$. What is the coequalizer of the two maps $X \to X$ given by $x \mapsto x$ and $x \mapsto x + 1$?

> CTfS §2.6.3; context: [[Finite Colimits in Set]], [[Map of Content]].

> Sources: CTfS, Exercise 2.6.3.2.

**Solution:** [[CTfS Chapter 2 Solutions#Solution 2.6.3.2|Solution 2.6.3.2]]

## Exercise 2.7.1.2

Create an olog that includes sets $X, Y$ and functions $f : X \to Y$ and $g : Y \to X$ such that $g \circ f = \mathrm{id}_X$ but $f \circ g \neq \mathrm{id}_Y$, i.e. $f$ is a retract section but not an isomorphism.

> CTfS §2.7.1; context: [[Map of Content]], [[Section and Retraction]].

> Sources: CTfS, Exercise 2.7.1.2.

**Solution:** [[CTfS Chapter 2 Solutions#Solution 2.7.1.2|Solution 2.7.1.2]]

## Exercise 2.7.2.2

For a finite set $A$ let $|A|$ be its cardinality. If $A, B$ are finite (possibly empty), is it always true that $|B^A| = |B|^{|A|}$?

> CTfS §2.7.2; context: [[Arithmetic of Sets]], [[Map of Content]].

> Sources: CTfS, Exercise 2.7.2.2.

**Solution:** [[CTfS Chapter 2 Solutions#Solution 2.7.2.2|Solution 2.7.2.2]]

## Exercise 2.7.2.5

We have $\mathrm{id}_{B^A} : \mathrm{Hom}(A, B) \to B^A$. Applying the inverse of the currying bijection $\varphi^{-1} : \mathrm{Hom}(\mathrm{Hom}(A, B), B^A) \to \mathrm{Hom}(\mathrm{Hom}(A, B) \times A, B)$ gives a function $\mathrm{ev} := \varphi^{-1}(\mathrm{id}_{B^A}) : \mathrm{Hom}(A, B) \times A \to B$.

- **a.** Describe $\mathrm{ev}$ on elements.
- **b.** Why might one be tempted to denote this function $\mathrm{ev}$?

> CTfS §2.7.2; context: [[Currying]], [[Map of Content]].

> Sources: CTfS, Exercise 2.7.2.5.

**Solution:** [[CTfS Chapter 2 Solutions#Solution 2.7.2.5|Solution 2.7.2.5]]

## Exercise 2.7.2.6

$\mathbb R^2$ was introduced as an abbreviation for $\mathbb R \times \mathbb R$, but also as $\mathbb R^{\underline 2}$ (functions $\underline 2 \to \mathbb R$). Use Exercise 2.1.2.13, Proposition 2.7.2.3 (currying), Exercise 2.4.2.10 (maps out of a coproduct) and $1 + 1 = 2$ to prove $\mathbb R^{\underline 2} \cong \mathbb R \times \mathbb R$.

> CTfS §2.7.2; context: [[Arithmetic of Sets]], [[Currying]], [[Map of Content]].

> Sources: CTfS, Exercise 2.7.2.6.

**Solution:** [[CTfS Chapter 2 Solutions#Solution 2.7.2.6|Solution 2.7.2.6]]

## Exercise 2.7.3.2

Proposition 2.7.3.1 claims both $A^0 \cong 1$ and $0^A \cong 0$ for every set $A$, which conflict for $A = 0$. What is the correct answer for $0^0$, based on the definitions of $0$, $1$ and $B^A$?

> CTfS §2.7.3; context: [[Arithmetic of Sets]], [[Cardinality]], [[Map of Content]].

> Sources: CTfS, Exercise 2.7.3.2.

**Solution:** [[CTfS Chapter 2 Solutions#Solution 2.7.3.2|Solution 2.7.3.2]]

## Exercise 2.7.3.3

For natural numbers, $ab = 0$ implies $a = 0$ or $b = 0$. Is the analogous statement true for sets?

> CTfS §2.7.3; context: [[Arithmetic of Sets]], [[Map of Content]].

> Sources: CTfS, Exercise 2.7.3.3.

**Solution:** [[CTfS Chapter 2 Solutions#Solution 2.7.3.3|Solution 2.7.3.3]]

## Exercise 2.7.4.7

The 2-simplex $\Delta^2$ is drawn as a filled-in triangle with vertices $V = \{1, 2, 3\}$. There is a simplicial complex $X = \partial\Delta^2$, drawn as an empty triangle with the same vertices. What is $X$, i.e. what are $X_0, X_1, X_2, X_3, \dots$?

> CTfS §2.7.4; context: [[Map of Content]], [[Simplicial Complex]].

> Sources: CTfS, Exercise 2.7.4.7.

**Solution:** [[CTfS Chapter 2 Solutions#Solution 2.7.4.7|Solution 2.7.4.7]]

## Exercise 2.7.4.12

Let $f : A \to \Omega$ be the characteristic function of $A_1 \subseteq A$, and let $A_2 = A - A_1$ be its complement.

- **a.** What is the characteristic function of $A_2$?
- **b.** Can you phrase it in terms of some function $\Omega \to \Omega$?

> CTfS §2.7.4; context: [[Booleans]], [[Map of Content]].

> Sources: CTfS, Exercise 2.7.4.12.

**Solution:** [[CTfS Chapter 2 Solutions#Solution 2.7.4.12|Solution 2.7.4.12]]

## Exercise 2.7.5.6

Show, in analogy to Proposition 2.7.5.5 (pullbacks preserve monomorphisms), that pushouts preserve epimorphisms.

> CTfS §2.7.5; context: [[Epimorphism]], [[Map of Content]].

> Sources: CTfS, Exercise 2.7.5.6.

**Solution:** [[CTfS Chapter 2 Solutions#Solution 2.7.5.6|Solution 2.7.5.6]]

## Exercise 2.7.6.4

A *pseudo-multiset* is like a multiset $(E, B, \pi)$ except that $\pi$ need not be surjective. Write down a pseudo-multiset that is not a multiset.

- **a.** Describe the difference between the two notions in terms of multiplicities.
- **b.** Which do you think is more useful: multisets or pseudo-multisets?

> CTfS §2.7.6; context: [[Map of Content]], [[Multiset]].

> Sources: CTfS, Exercise 2.7.6.4.

**Solution:** [[CTfS Chapter 2 Solutions#Solution 2.7.6.4|Solution 2.7.6.4]]

## Exercise 2.7.6.5

Consider the multisets $X = (1, 1, 2, 3)$ and $Y = (a, b, b, b)$ of Exercise 2.7.6.2.

- **a.** Write each of them in the form $(E, B, \pi)$.
- **b.** What are the mappings $X \to Y$ (pairs $(f_1, f_0)$ with $\pi' f_1 = f_0 \pi$)?
- **c.** If we remove the requirement that the square commutes, how many mappings $X \to Y$ are there?

> CTfS §2.7.6; context: [[Map of Content]], [[Multiset]].

> Sources: CTfS, Exercise 2.7.6.5.

**Solution:** [[CTfS Chapter 2 Solutions#Solution 2.7.6.5|Solution 2.7.6.5]]

## Exercise 2.7.6.8

Given relative sets $(X, p)$, $(Y, q)$, $(Z, r)$ over $B$ and mappings $f : (X, p) \to (Y, q)$, $g : (Y, q) \to (Z, r)$ (functions with $q f = p$, $r g = q$), is there a reasonable notion of composition giving a mapping $(X, p) \to (Z, r)$?

> CTfS §2.7.6; context: [[Map of Content]], [[Multiset]].

> Sources: CTfS, Exercise 2.7.6.8.

**Solution:** [[CTfS Chapter 2 Solutions#Solution 2.7.6.8|Solution 2.7.6.8]]

## Exercise 2.7.6.9

- **a.** Let $\{\ast\}$ be a one-element set. What is the difference between sets over $\{\ast\}$ and simply sets?
- **b.** Describe the sets relative to $\varnothing$. How many are there?

> CTfS §2.7.6; context: [[Map of Content]], [[Multiset]].

> Sources: CTfS, Exercise 2.7.6.9.

**Solution:** [[CTfS Chapter 2 Solutions#Solution 2.7.6.9|Solution 2.7.6.9]]

## Exercise 2.7.6.14

There is a strong relationship between $A$-indexed sets and relative sets over $A$. What is it?

> CTfS §2.7.6; context: [[Dependent Type]], [[Indexed Set]], [[Map of Content]].

> Sources: CTfS, Exercise 2.7.6.14.

**Solution:** [[CTfS Chapter 2 Solutions#Solution 2.7.6.14|Solution 2.7.6.14]]
