#exercise #solution #proof

**Exercise 5.1.1.** Show that the bijection in the proof of the left unit law $1 \times a \cong a$ is natural ([[Cartesian Category]]).

## Solution

The bijection $\mathcal{C}(x, 1 \times a) \cong \mathcal{C}(x, a)$ sends $h = \langle !, f \rangle$ to $f = \mathsf{snd} \circ h$ (the component into $1$ is forced). Changing focus along $g : a \to b$: post-composing $h$ with $\mathrm{id}_1 \times g$ gives $\langle !, g \circ f \rangle$, whose image is $g \circ f$; applying the bijection first and then $g \circ -$ gives $g \circ f$ too. Naturality in $x$ (pre-composition with $k : x' \to x$) is equally immediate. Hence $\lambda = \mathsf{snd}$ is a natural isomorphism.

> Sources: DaoFP Exercise 5.1.1.
