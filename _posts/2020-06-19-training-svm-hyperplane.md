---
title: Training Hyperplane for Support Vector Machine
tags: ml ai svm
img_url: /assets/img/svm.png
layout: blog
---

We have this simple function of an hyperplane.
$$\mathbf{\omega \cdot x} + b = 0$$
However, getting the optimized hyperplane for SVM
is not so easy as this function looks.

To get an hyperplane that meets the condition,
$$y_n \left( \langle \mathbf{\omega , x_n}\rangle  + b \right) \geq 1 $$
and maximum margin, one can approach by minimizing a loss function of this
operation.

<div style="width: 100%; overflow: scroll;">
$$min_{w, b} \: {1 \over 2}\|\omega\|^{2} 
+ C \sum max\{0, 1 - y_n \left( \langle \mathbf{\omega , x_n}\rangle  + b \right)\}$$
</div>

This can be solved by (sub-) gradient descent methods. A [lecture note from
University of Utah](https://svivek.com/teaching/lectures/slides/svm/svm-sgd.pdf)
explains the procedure well. The lecture note suggests stochastic 
gradient descent for the computation speed.
