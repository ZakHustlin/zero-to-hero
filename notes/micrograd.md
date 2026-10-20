# micrograd notes

## Session 1: 19 Oct 2026 (hand predictions for Block 1)

f(x) = 3x² − 4x + 5, so f′(x) = 6x − 4

| Point | Predicted | Correct | Notes |
|---|---|---|---|
| slope at x = 3 | 14 | 14 | ✓ |
| slope at x = −3 | −14 | −22 | Arithmetic slip, corrected to −22 |
| turning point | ⅔ | ⅔ | ✓ |

- f′(x) gives the gradient of the function at any x.
- The parabola is symmetric about x = ⅔, not x = 0. x = 3 is 7/3 from it and x = −3 is 11/3 from it, so the slopes aren't mirror images.
- Why |slope| = 6 × distance from the turning point: f″(x) = 6, so the slope rises by 6 for every unit along x, and it's 0 at x = ⅔. In factored form, f′(x) = 6(x − ⅔).

**Next (Block 1 code):** check 14, −22 and ⅔ numerically with (f(x+h) − f(x)) / h. Then try h = 1e-15.

## Session 2: 20 Oct 2026 (Block 0 smoke test + Block 1 code)

- Smoke test ✓. Surprise: Graphviz draws nodes as ovals by default.
- f(3) = 20 ✓; minimum at (⅔, 11/3) ✓.
- h = 0.001 at x = 3 → 14.00300000000243. Predicted "slightly above 14" ✓. The error is exactly 3h; with h = −0.001 it's 13.997.
  - Corrections to my reasoning: f″ = 6 is constant, so the gradient rises at a *steady* rate, by 6 per unit of x (adding, not multiplying).
- ✗ Predicted the slope at x = −3 is the negative of the slope at x = 3 (symmetry). Wrong again: the symmetry is about x = ⅔, so the slope is −22. Same trap as last night.
- x = ⅔ → about 0 (actually 3h = 0.003).
- Extension 1, h = 1e-15 → 10.66 (true value 14). Ran it without predicting first; explanation still to come.
- Extension 1, explained: a float holds about 16 *significant* figures. The gap between storable numbers is about 4.4e-16 near 3 and about 3.6e-15 near 20, so 3 + 1e-15 rounds to 3 + 4.4e-16. f(3+h) and f(3) then agree in almost every digit, and the subtraction leaves mostly rounding noise, which dividing by h amplifies. Trade-off: if h is too big you get the 3h error; if h is too small, rounding dominates.
  - Confusion: I read the "e" in 1e-15 as Euler's number. It actually means × 10^.
- Extension 2, central difference (f(x+h) − f(x−h)) / 2h: exactly 14 for the quadratic, because the +3h and −3h errors cancel. The output 13.999999999999346 is off by 6.5e-13, all rounding. (I first miscounted it as 1e-15.)
- Tested on g(x) = x³ at x = 3 (true slope 27):
  - forward: 27.009 → 27.0009, error ∝ h (×10 smaller)
  - central: 27.000001 → 27.00000001, error ∝ h² (×100 smaller). ✗ I predicted both would shrink ×10.
  - Why: (3+h)³ − (3−h)³ = 54h + 2h³. The even-power terms cancel and the odd ones double, so ÷2h gives 27 + h². A balanced step cancels the h-error term. (I slipped on the signs in the subtraction first.)
- Block 1 complete.

### Block 2: several inputs (d = a*b + c, a=2, b=−3, c=10)
- d = 4 ✓. Signs for nudging a (down), b (up) and c (up) all correct ✓.
- First reported raw differences instead of dividing by h. My `dif` line computed d1 − d2 (backwards) and one print label was wrong.
- ∂d/∂a = b = −3, ∂d/∂b = a = 2 ✓ (I swapped them at first).
- ✗ Said ∂d/∂c = ab, then = d. It's 1: the derivative is the *rate of change*, not the value of the output.
- Key sentence: nudging a by h changes a*b by b·h, so ∂d/∂a = b. For a product, the local derivative with respect to one input is the other input. This is the local step of backprop.
- Block 2 complete.

### Block 3: Value class
- `__repr__`, `__add__`, `__mul__`, `__radd__`/`__rmul__` (needed for 1 + a; fine because + and × are commutative, would fail for subtraction).
- `self` = the object the method is called on; `a*b` → `Value.__mul__(a, b)`.
- `_prev` holds parent *Values* (direct parents only), not operations. It's a set: `e = a + a` gives `{a}`, so the duplicate is lost. That's a clue for the = vs += gradient bug in micrograd 2.
- `trace` is a depth-first search back through `_prev`. The `if v not in nodes` check stops shared subgraphs being walked again.
- ✗ Bug I wrote: `__mul__` used `+`, so L came out as 7 instead of −8. My hand prediction caught it.
- The graph links objects, not names: reassigning `e` afterwards doesn't change the drawing.
- If `__add__` mutated self, inputs would be overwritten and d and e would be the same node. Every operation must return a new Value.
- micrograd 1 complete.

### Reflection
- Learned: classes can record how values were made, giving a graph you can walk. Error orders: forward ∝ h, central ∝ h².
- Surprise: making h smaller made the answer *worse* (floating point).
- Question: how does backprop get ∂L/∂a for a node several steps from L, without nudging every input?

Is a gradient a *causal* claim ("nudging a changes L") or only a mathematical one?
