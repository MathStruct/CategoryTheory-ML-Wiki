#exercise #solution #proof

**Exercise 7.4.** Prove Proposition 7.3 (the [[Pasting Lemma for Pullbacks]]) using the definition of [[Limit]] from Section 3.4.2.

## Solution

Let the diagram be $A \xrightarrow{f} B \xrightarrow{g} C$ over $A' \xrightarrow{f'} B' \xrightarrow{g'} C'$ with verticals $h_1, h_2, h_3$, and suppose the right square is a pullback.

*Left square pullback $\Rightarrow$ rectangle pullback.* Given $p : X \to C$ and $q : X \to A'$ with $q \mathbin{;} f' \mathbin{;} g' = p \mathbin{;} h_3$, the right pullback yields a unique $r : X \to B$ with $r \mathbin{;} h_2 = q \mathbin{;} f'$ and $r \mathbin{;} g = p$; the left pullback then yields a unique $r' : X \to A$ with $r' \mathbin{;} f = r$ and $r' \mathbin{;} h_1 = q$. Hence $r' \mathbin{;} f \mathbin{;} g = p$ and $r' \mathbin{;} h_1 = q$. If $r_0$ also satisfies these, then $r_0 \mathbin{;} f$ satisfies the defining equations of $r$, so $r_0 \mathbin{;} f = r$, and then $r_0 = r'$ by uniqueness in the left square.

*Rectangle pullback $\Rightarrow$ left square pullback.* Given $r : X \to B$ and $q : X \to A'$ with $r \mathbin{;} h_2 = q \mathbin{;} f'$, set $p := r \mathbin{;} g$. Then $p \mathbin{;} h_3 = r \mathbin{;} h_2 \mathbin{;} g' = q \mathbin{;} f' \mathbin{;} g'$, so the rectangle yields a unique $r' : X \to A$ with $r' \mathbin{;} f \mathbin{;} g = p$ and $r' \mathbin{;} h_1 = q$. Now $r' \mathbin{;} f$ and $r$ both satisfy $(-) \mathbin{;} g = p$ and $(-) \mathbin{;} h_2 = q \mathbin{;} f'$, so by uniqueness in the right pullback $r' \mathbin{;} f = r$. Uniqueness of $r'$ follows from uniqueness for the rectangle. $\blacksquare$

> Sources: 7 Sketches, Exercise 7.4 and Solution A.7.
