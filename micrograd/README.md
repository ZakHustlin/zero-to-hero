# micrograd

A scalar-valued autograd engine and a tiny neural net library on top of it.

- [ ] Numerical derivatives
- [ ] `Value` class: `__add__`, `__mul__`, `_prev`, `_op`
- [ ] Manual backprop through an expression, then a tanh neuron
- [ ] `_backward` closures + topological sort → `backward()`
- [ ] Hit the `=` vs `+=` gradient bug myself
- [ ] `Neuron` → `Layer` → `MLP`, training loop
- [ ] Cross-check gradients against PyTorch

## Backprop in my own words
_(write from memory once the part is done)_
