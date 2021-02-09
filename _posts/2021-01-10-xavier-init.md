---
title: Learning Xavier Initialization
tags: ml
img_url: /assets/img/xavier_init.png
layout: blog
---

Learning PyTorch, I happened to come across a function called _reset_parameters()_.
This function included xavier initalization instead of standard initialization.
So, I decided to read Xavier Glorot and Yoshua Bengio's "Understanding the difficulty of training deep feedforward neural networks".
The origin paper of Xavier intialization gave me great undersatnding of its objective.

### Xavier Initialization

Objectives of Xavier initialization are simply all about managing variance, 

1. Maintaining variance of forward propagation.<br>
Each neuron should equally contribute to right prediction.

2. Maintaining variance of backward propagation<br>
Back propagation should not be susceptible to vanishing and exploding gradient.

The part that interested me the most is how variance can be calculated and multiplied and added according to the number of layers and hidden neurons within each layer.

<div style="width: 100%; overflow: scroll;">
$$ Var[z^i]  = Var[x]\prod^{i-1}_{i^{'}=0}n_{i^{'}}Var[W^{i^{'}}]$$
</div>

Moving forward, the authors present the following way to intialize weights from such distribution,

<div style="width: 100%; overflow: scroll;">
$$ W \sim U\Big[-{\sqrt{3}\over{\sqrt{n_{in} + n_{out}}}}, {\sqrt{3}\over{\sqrt{n_{in} + n_{out}}}}\Big] $$
</div>

### Additional Study
Although introduction of Xavier initialization is the main meal of the paper, I enjoyed learning other points made by the authors.
* It is important to choose a good activation function.
<br>
For example, sigmoid shows a quick saturation of hidden layer in the output.
It makes sense because in the early training period, bias overrules features in neurons.
This restricts lower layers to learn meaningful features, and thus slower learning.
Choosing zero-mean activation function line tanh can avoid the problem.

* It is important to choose a right loss function.
<br>
The authors compared the result between quadratic(LSE) and cross entropy. 
Result shows quadratic loss function gives more plateaus, which I assume it means slower training (flatness gives smaller gradient).