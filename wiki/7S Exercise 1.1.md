#exercise #solution

**Exercise 1.1.** A function $f : \mathbb{R} \to \mathbb{R}$ is called (a) **order-preserving** if $x \leq y$ implies $f(x) \leq f(y)$; (b) **metric-preserving** if $|x - y| = |f(x) - f(y)|$; (c) **addition-preserving** if $f(x + y) = f(x) + f(y)$. For each property *foo*, find an $f$ that is foo-preserving and one that is not.

> 7 Sketches §1.1; context: [[Generative Effect]], [[Monotone Map]].

## Solution

- **Order**: $f(x) = x + 5$ preserves order; $g(x) = -x$ does not ($1 \leq 2$ but $-1 \not\leq -2$).
- **Metric**: $f(x) = x + 5$ preserves the metric; $g(x) = 2x$ does not ($|1 - 2| = 1$ but $|2 - 4| = 2$).
- **Addition**: $f(x) = 3x$ preserves addition; $g(x) = x + 1$ does not ($g(0 + 0) = 1 \neq 2 = g(0) + g(0)$).

The moral: "asking which aspects of $X$ one wants to preserve under the observation $f$ becomes the question *what category are you working in?*"
