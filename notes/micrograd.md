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

## Open question
Is a gradient a *causal* claim ("nudging a changes L") or only a mathematical one?
