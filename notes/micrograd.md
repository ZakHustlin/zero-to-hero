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

## Open question
Is a gradient a *causal* claim ("nudging a changes L") or only a mathematical one?
