#solution #program

**Solution to [[DaoFP Exercise 5.1.3|Exercise 5.1.3]].**

A map out of $b + a \times b$ is a pair $[h_1, h_2]$ with $h_1 : b \to (1 + a) \times b$ and $h_2 : a \times b \to (1 + a) \times b$, each a map into a product: $h_1 = \langle \mathsf{Left} \circ !, \mathrm{id} \rangle$, $h_2 = \langle \mathsf{Right} \circ \mathsf{fst}, \mathsf{snd} \rangle$. So $h = [\langle \mathsf{Left} \circ !, \mathrm{id} \rangle, \langle \mathsf{Right} \circ \mathsf{fst}, \mathsf{snd} \rangle]$ — the same function as before, reached by decomposing in the other order (`either (\b -> (Left (), b)) (\(a, b) -> (Right a, b))`).

> Sources: DaoFP Exercise 5.1.3.
