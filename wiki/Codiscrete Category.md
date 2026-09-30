#definition

The **codiscrete** (indiscrete, chaotic) category on a set $X$ has objects $X$ and exactly one morphism $x \to y$ for every pair $(x, y)$. It is a [[Groupoid]] and the [[Codiscrete Preorder]] viewed as a category. The construction $\mathbf{Set} \to \mathbf{Cat}$ is right adjoint to the objects functor $\mathrm{Ob} : \mathbf{Cat} \to \mathbf{Set}$ (7 Sketches Example 3.74: "codiscrete things are right adjoints"). Contrast: [[Discrete Category]].

**Indiscrete categories are equivalent to a point** (CTfS Example 4.3.4.3). For a nonempty set $S$, the indiscrete category $K_S$ is isomorphic to the terminal category $\mathbf 1$ only if $|S| = 1$, but it is always *[[Equivalence of Categories|equivalent]]* to $\mathbf 1$: pick any $s_0 \in S$; the unique isomorphisms $s \to s_0$ assemble into a natural isomorphism $\mathrm{id}_{K_S} \cong (\text{constant at } s_0)$. Every object of $K_S$ is both initial and terminal ([[CTfS Chapter 4 Exercises#Exercise 4.5.3.13|CTfS Exercise 4.5.3.13]]). The functor $\mathrm{Ind} : \mathbf{Set} \to \mathbf{Cat}$ also exhibits a [[Subcategory|full subcategory]] $\mathcal{C}_{\mathrm{Ob} = X}$ as the fiber product $\mathcal{C} \times_{\mathrm{Ind}(\mathrm{Ob}\,\mathcal{C})} \mathrm{Ind}(X)$ (CTfS Example 4.6.3.4).

> Sources: 7 Sketches Example 3.74; CTfS Examples 4.3.4.3, 4.6.3.4, 5.1.1.7.
