---
title: "Micrograd, written by hand"
date: 2026-05-04
pillars: "llm"
tags: ["karpathy", "neural-networks", "from-scratch"]
summary: "The first lecture of Karpathy's series and what it teaches you that's not the math."
---

Karpathy's "Neural Networks: Zero to Hero" series starts with micrograd, a 150-line implementation of automatic differentiation. You build it from scratch in a Jupyter notebook over the course of two and a half hours. By the end, you have a tiny library that can train a multi-layer perceptron on a 2D classification problem.

The math is high-school calculus dressed up: derivatives, the chain rule, gradient descent. None of it is hard. What's hard is the architectural intuition Karpathy is teaching beneath the math.

{{< break >}}

## The thing the lecture is actually about

It's not really about backprop. Backprop is the vehicle. The lecture is about *the value object* — a node in a computation graph that knows its data, its gradient, its parents, and how to propagate gradient backward through itself.

```python
class Value:
    def __init__(self, data, _children=(), _op=''):
        self.data = data
        self.grad = 0
        self._backward = lambda: None
        self._prev = set(_children)
        self._op = _op
```

Once you have this, the rest follows mechanically. Add two `Value` objects, you produce a new `Value` whose `_backward` knows that the upstream gradient flows equally to both parents. Multiply two `Value` objects, the upstream gradient flows scaled by the *other* operand. tanh, ReLU, exp — same pattern, different `_backward` closure.

The architectural move is making the graph *explicit*. Most ML libraries hide the computation graph behind tensor operations. Karpathy's pedagogical choice is the opposite: make the graph the unit of attention, build everything around it.

{{< break >}}

## What this is preparing me for

The follow-on lectures — makemore, GPT-from-scratch — all assume you internalized the graph mental model. Tensors stop being magic boxes; they become *batched value objects* whose backward passes are vectorized for performance but conceptually identical.

I wrote the implementation by hand rather than copying. Two and a half hours became four — there's a real difference between watching Karpathy type and typing it yourself, hitting the same shape errors he warns about, watching the loss curve descend on the moons dataset. The body of the work is in your fingers afterward, not just your eyes.

{{< break >}}

## Cross-pillar note

The note + ornament token representation I've been planning for the music dev v2 melody work has an interesting parallel here. A `Value` is a piece of data plus a function for how it propagates information backward. A `(swara, gamaka)` token is a piece of pitch data plus a function for how it realizes into continuous pitch contour at output time. Same architectural shape: data plus a transformation rule, treated as a single unit.

Probably not a meaningful analogy in any technical sense. But noticing the structural rhyme is itself useful — the kind of thing that happens when you work several thinking-domains in parallel.

Onto makemore next weekend.
