#exercise #solution #proof

**Exercise 6.70.** Check that the maps $\varphi_{S,T} : \mathcal{P}(S) \times \mathcal{P}(T) \to \mathcal{P}(S \times T)$, $(A, B) \mapsto A \times B$, of Example 6.69 are natural in $S$ and $T$: for $f : S \to S'$, $g : T \to T'$ the square with $\mathrm{im}_f \times \mathrm{im}_g$ and $\mathrm{im}_{f \times g}$ commutes. (This makes the [[Power Set]] functor a lax [[Monoidal Functor]].)

## Solution

Let $A \subseteq S$, $B \subseteq T$. Then
$$\varphi_{S',T'}(\mathrm{im}_f(A), \mathrm{im}_g(B)) = \{f(a) \mid a \in A\} \times \{g(b) \mid b \in B\} = \{(f(a), g(b)) \mid a \in A, b \in B\} = \mathrm{im}_{f \times g}(A \times B) = \mathrm{im}_{f \times g}(\varphi_{S,T}(A, B)),$$
so the square commutes.

> Sources: 7 Sketches, Exercise 6.70 and Solution A.6.
