#exercise #solution

**Exercise 5.69.** $U : \mathbf{Mat}(R) \to \mathbf{Set}$, $n \mapsto R^n$, $M \mapsto (v \mapsto vM)$. 1. Show $U$ preserves unit and product. 2. Show it carries monoid objects to monoids. 3. Which monoid structure on $\mathbb{R}$ does $(1, \text{add}, \text{zero})$ give?

## Solution

1. $R^0 \cong \{1\}$ and $R^m \times R^n \cong R^{m+n}$ canonically. 2. A [[Monoidal Functor]] preserves the monoid diagrams: $U(\eta)(1) = (0, \dots, 0)$, $U(\mu)(a, b) = a + b$ componentwise. 3. The additive one: $(5, 3) \mapsto 8$.
