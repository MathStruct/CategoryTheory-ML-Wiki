#solution #proof

**Solution to [[7S Exercise 2.94|Exercise 2.94]].**

Yes. Joins are unions. The hom-element is $B \multimap C = \overline{B} \cup C$: if $A \cap B \subseteq C$ then $A = (A \cap B) \cup (A \cap \overline{B}) \subseteq \overline{B} \cup C$; conversely if $A \subseteq \overline{B} \cup C$ then $A \cap B \subseteq (\overline{B} \cup C) \cap B = C \cap B \subseteq C$. (This is the [[Topos|Boolean/Heyting algebra]] structure of the power set.)

> Sources: 7 Sketches, Exercise 2.94 and Solution A.2.
