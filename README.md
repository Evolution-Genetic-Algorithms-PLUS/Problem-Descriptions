# Compact Genetic Algorithm (cGA) - Benchmark Problems

## Global cGA Setup

### cGA Parameters

- Probability vector initialization: p<sub>i</sub> = 0.5 for all i
- Update step after winner/loser comparison: &Delta;p<sub>i</sub> = &plusmn; 1/n

### Randomness Control

- Use fixed seeds for reproducibility
- Run each configuration with 30 independent seeds
- Recommended seeds: 0-29

---

## 1. Easy Task - OneMax

Maximize the number of ones in a binary string of length L.

- Parameters: L = 100
- Fitness: f(x) = sum of ones in x
- Termination: stop at 5,000 iterations or earlier if all p<sub>i</sub> values are converged (near 0 or 1)

### Purpose
This serves as a "sanity check." If your cGA cannot solve OneMax quickly, there
is a fundamental bug in the implementation or parameter settings.

---

## 2. Medium Task - Deceptive Trap Function

Partition the bitstring of length L into m blocks of k bits each and maximize deceptive block fitness.

- Parameters: L = 100, k = 5, m = L / k = 20
- Block fitness with u = number of ones in a block:
  - if u = k, return k
  - else return k - u - 1
- Total fitness: F(x) = sum of all block fitness values
- Termination: stop at 20,000 iterations or earlier if all p<sub>i</sub> values are converged (near 0 or 1)

### Purpose
- Tests the cGA's diversity and its ability to avoid premature convergence.
- Highlights the impact of the "virtual population size" on escaping local optima: with
smaller n, step size 1/n is smaller, thus probabilities fix on 0 before the
convergence shows that 1 is global optimum …
- more steps => higher chance of generating block of all 1s, where fitness = k, so
that convergence starts in correct direction towards 1
---

## 3. Hard Task - Circle Packing

Place circles in the unit square to avoid overlap and boundary violations.

- Parameters: N = 30 circles, radius r = 0.1
- Domain: [0, 1] &times; [0, 1]
- Representation: 10 bits per coordinate (x, y), genome length = N &times; 2 &times; 10
- Decoding: map each 10-bit segment to [0, 1]
$$
\mathrm{val}_{\mathrm{float}} =
\frac{\sum_{i=0}^{k-1} b_i \cdot 2^{k-1-i}}{2^k - 1}
$$
- Fitness: maximize negative total penalty (boundary violations + pairwise overlaps)
$$Fitness = \sum_{i=1}^{N} (P_{bound}(x_i) + P_{bound}(y_i)) + P_{overlap}$$

$$P_{bound}(x) = \max(0, r - x) + \max(0, x + r - 1)$$

$$P_{bound}(y) = \max(0, r - y) + \max(0, y + r - 1)$$

$$P_{overlap} = \sum_{i < j} \max(0, 2r - d_{ij})^2$$

- Termination: stop at 50,000 iterations or earlier if all p<sub>i</sub> values are converged (near 0 or 1)

---

## Common Convergence Rule

For all problems, convergence can be used as an early stop when every p<sub>i</sub> value is sufficiently close to 0 or 1.

