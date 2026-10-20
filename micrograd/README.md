# micrograd

A scalar-valued autograd engine and a tiny neural net library on top of it.

- [x] Numerical derivatives
- [x] `Value` class: `__add__`, `__mul__`, `_prev`, `_op`
- [ ] Manual backprop through an expression, then a tanh neuron
- [ ] `_backward` closures + topological sort → `backward()`
- [ ] Hit the `=` vs `+=` gradient bug myself
- [ ] `Neuron` → `Layer` → `MLP`, training loop
- [ ] Cross-check gradients against PyTorch

## Backprop in my own words
_(written from memory, added to as each part is done)_

**What a derivative tells you about a node (micrograd 1).**
The derivative of L with respect to a node tells you how much L changes when you nudge that node.
For example, ∂L/∂e = −2 means a small increase h in e changes L by about −2h, so L goes *down*.
The sign gives the direction of the effect and the size gives its strength.
The `Value` graph records how L was built, which is what lets us work these out for every node.
