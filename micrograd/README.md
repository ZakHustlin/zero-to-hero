# micrograd

A scalar-valued autograd engine and a tiny neural net library on top of it.

- [x] Numerical derivatives
- [x] `Value` class: `__add__`, `__mul__`, `_prev`, `_op`
- [x] Manual backprop through an expression, then a tanh neuron
- [x] `_backward` closures + topological sort → `backward()`
- [x] Hit the `=` vs `+=` gradient bug myself
- [x] tanh rebuilt from `exp`, `**`, `-`, `/` (same gradients)
- [x] `Neuron` → `Layer` → `MLP`, training loop
- [x] Cross-check gradients against PyTorch

## Backprop in my own words
_(written from memory, added to as each part is done)_

**What a derivative tells you about a node (micrograd 1).**
The derivative of L with respect to a node tells you how much L changes when you nudge that node.
For example, ∂L/∂e = −2 means a small increase h in e changes L by about −2h, so L goes *down*.
The sign gives the direction of the effect and the size gives its strength.
The `Value` graph records how L was built, which is what lets us work these out for every node.

**Why tanh? (micrograd 4–5)**
We apply tanh, acting as a hidden layer activation function, to the pre-activation value of a neuron.
We do this because a stack of layers without it collapses into a straight line, and a straight line cannot fit a curve.
Applying tanh makes each neuron's output a non-linear function of its inputs, so the network as a whole can represent curves.
Without tanh, fitting y = x² hit the floor we predicted by hand: loss 28/27, with the net settled on the flat line y = 4/9. With tanh it reached a loss of 0.0067.

**How I know my engine is right (micrograd 5)**
First we built the network with the `Value` class, then again using tensors in PyTorch, and compared all 41 gradients.
The biggest disagreement was around 10⁻¹⁶ (4.4e-16), because float addition isn't associative, so adding in a different order gives slightly different results.
A falling loss isn't evidence that the gradients are right: with an incorrect tanh derivative (1 − t instead of 1 − t²) the loss still fell, because between −1 and 1 both have the same sign, so every step still moved each weight the right way, just by the wrong amount.
The cross-check tells us the two programs compute the same thing, but not whether the model predicts well. It also only covers one network on the inputs we tried, and both use the same derivative formulas; a numerical check would be more independent, since it avoids backprop.
