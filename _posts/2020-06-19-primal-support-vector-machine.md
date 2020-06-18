---
title: Primal Support Vector Machine
tags: ml ai
---

In a case where you need to draw a hyperplane that does
binary classification, there can be multiple hyperplanes.
Primal SVM is one way to solve this issue by bringing the
concept that maximizes a margin of the hyper vector.

Before anything, definition of margin needs to be settled.
Margin is a distance from a hyperplane to its closest data points.

_"Vladimir Vapnik and Alexey Chervonenkis said when the margin
is large, the "complexity" of the function is low, and hence learning
is possible."_

From one of the methods that derives the margin called
_hard margin SVM_, you see it focuses on getting the
minimal value of

$$1 \over 2 \|\omega\|^{2}$$

while assuming the set margin of length 1.
This seems to contradict the goal, to find the max
margin. Through mathematical tweaks, however, this
equation is equivalent to the meaning of getting
the max margin.
