#exercise #solution #proof

**Exercise 1.25.** Suppose $f : A \to \varnothing$ is a function to the empty set. Show that $A$ is empty.

> 7 Sketches §1.2.1; see [[Function]], [[Initial Object]].

## Solution

By Definition 1.22, $f$ is a subset $F \subseteq A \times \varnothing$ such that for every $a \in A$ there is a unique $b \in \varnothing$ with $(a, b) \in F$. There are no $b \in \varnothing$, so there can be no $a \in A$: $A = \varnothing$. (Categorically: $\varnothing$ is [[Initial Object|initial]] and *strict* in $\mathbf{Set}$ — any map into it is an isomorphism.)
