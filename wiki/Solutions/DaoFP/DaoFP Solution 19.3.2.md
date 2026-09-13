#solution #proof

**Solution to [[DaoFP Exercise 19.3.2|Exercise 19.3.2]].**

Whiskering $\sigma$ with $R$: $\sigma \circ R = (\alpha \circ L \circ R) \cdot (G \circ \eta \circ R) : G R \to L R$. Following with the counit $\varepsilon : L R \to \mathrm{Id}$ gives $\varepsilon \cdot (\alpha \circ L R) \cdot (G \circ \eta \circ R)$. By the interchange law, $\varepsilon \cdot (\alpha \circ L R) = \alpha \cdot (G \circ R \varepsilon)$ (slide $\alpha$ past $\varepsilon$: they act on different strings). So we get $\alpha \cdot (G \circ R \varepsilon) \cdot (G \circ \eta R) = \alpha \cdot (G \circ (R\varepsilon \cdot \eta R)) = \alpha \cdot (G \circ \mathrm{id}_R) = \alpha$ by the triangle identity $R \varepsilon \cdot \eta R = \mathrm{id}_R$. In [[String Diagram|string diagrams]]: the zigzag formed by the cup $\eta$ and the cap $\varepsilon$ on the $R$-string is pulled straight.

> Sources: DaoFP Exercise 19.3.2.
