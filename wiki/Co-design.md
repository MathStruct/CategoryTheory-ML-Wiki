#example #annotation

**Collaborative design (co-design)** is the hierarchical design process in which a project is divided among teams, each *providing* functionality and *requiring* resources, recursively. Teams are dependent yet must work independently; design-change notices ("I need more space", "I can't provide that torque") ripple through feedback loops and can make projects fail. 7 Sketches presents Andrea Censi's mathematical theory of co-design [Cen15; Cen17].

> Sources: 7 Sketches §4.1 ("Can we build it?"), §4.2.3 ("Back to co-design diagrams"), §4.5.2, §4.6.

## Co-design diagrams

Each part (chassis, motor, battery, robot) is a box with **functionalities** provided on the left and **resources** required on the right; wires are [[Preorder|preorders]] of resources ordered "from less useful to more useful" ($x \leq y$: $x$ is available given $y$; $\$10 \leq \$20$, $5\mathrm{W} \leq 6\mathrm{W}$). A $\leq$ on a wire says a requirement must be at most what is produced. Boxes marked $\Sigma$ sum inputs (chassis carries the weight of motor and battery: a feedback loop — "is the extra power worth the heavier load?").

```tikz
\usepackage{tikz}
\begin{document}
\begin{tikzpicture}[box/.style={draw, minimum height=1.2cm, minimum width=1.4cm}]
\node[box] (ch) at (0,0) {Chassis};
\node[box] (mo) at (3,0) {Motor};
\node[box] (ba) at (6,0) {Battery};
\draw (-1.6,0.3) node[left]{load} -- (ch.west |- 0,0.3);
\draw (-1.6,-0.3) node[left]{velocity} -- (ch.west |- 0,-0.3);
\draw (ch.east |- 0,0.3) -- node[above]{torque} (mo.west |- 0,0.3);
\draw (ch.east |- 0,-0.3) -- node[below]{speed} (mo.west |- 0,-0.3);
\draw (mo.east |- 0,0.3) -- node[above]{voltage} (ba.west |- 0,0.3);
\draw (mo.east |- 0,-0.3) -- node[below]{current} (ba.west |- 0,-0.3);
\draw (ba.east) -- (7.6,0) node[right]{\$};
\draw[rounded corners, dashed] (-1.3,-1.2) rectangle (7.2,1.6);
\node at (3,1.3) {Robot};
\end{tikzpicture}
\end{document}
```

Each box is a [[Feasibility Relation]] $\Phi : P \times R \to \mathbb{B}$ ("can I provide $p$ given $r$?") which is *monotone* $P^{\mathrm{op}} \times R \to \mathbf{Bool}$: producing less with the same resources, or the same with more resources, stays feasible. A co-design problem asks for the **composite** feasibility relation of the outer box, and "the mathematics provides an algorithm producing the feasibility relation of the whole from those of the parts", recursively down the hierarchy. Ports on one side are combined by the [[Product Preorder]]: $\mathsf{Chassis} : \mathsf{load} \times \mathsf{velocity} \nrightarrow \mathsf{torque} \times \mathsf{speed} \times \$$.

**Movie example** (§4.2.3): tone $T = \{\mathsf{mean} \leq \mathsf{good\text{-}natured}\}$, entertainment $E = \{\mathsf{boring} \leq \mathsf{funny}\}$, cost $\$ = \{100\mathrm{K} \leq 500\mathrm{K} \leq 1\mathrm{M}\}$; a feasibility relation $\Phi : T \times E \nrightarrow \$$ says e.g. a good-natured boring movie costs $\$500\mathrm{K}$ (and the producers would happily take $\$1\mathrm{M}$). Arrows in the [[Collage]] read "I can provide the source given the target". A node like $(\mathsf{g/n}, \mathsf{funny})$ with no outgoing bridge is valid: it is infeasible at any cost ([[7S Exercise 4.18]]).

The framework: feasibility relations are $\mathbf{Bool}$-[[Profunctor|profunctors]]; they compose ([[Category of Profunctors|$\mathbf{Feas}$]]); $\mathbf{Feas}$ is a [[Compact Closed Category]], which is what makes the feedback wiring diagrams meaningful. Replacing $\mathbf{Bool}$ by [[Cost]] answers "what is the minimum cost?" instead of "is it possible?".
