---
title: Vanilla RNN
tags: ml, nlp
img_url: /assets/img/rnn.png
layout: blog
---

### Motif
Language model is the probability of a sequence.
This can be approximated as the product of _n_-gram,

$$P(w_1,w_2,...,w_m)=\prod^{i=m}_{i=1}P(w_i|w_1,...,w_{i-1})\approx\prod^{i=m}_{i=1}P(w_i|w_1,...,w_{i-1})$$

So, _n_-gram was a natural approach to do language modeling.
However, this traditional approach has two limitations.
* sparsity of _n_-gram
* exponential growth in model size as _n_ increases

### Vanila RNN
<p align="center">
    <img src="/assets/img/rnn_structure.png" alt="RNN structure"  width="40%"/>
</p>

In contrast to the complex network of RNN,
equations to express the network is surprisingly very simple.
This is because weights are repeated used throughout timestamps.

$$h_t=\sigma(W_hh_{t-1} + W_ex_t)$$

$$\hat{y}=softmax(Uh_t)$$

Here are short description of dimensions of each parameter.

$$x_t\in\mathbb{R}^d$$

$$W_e\in\mathbb{R}^{D_h\times d}$$

$$W_h\in\mathbb{R}^{D_h\times D_h}$$

$$h_{t-1}\in\mathbb{R}^{D_h}$$

$$\hat{y}\in\mathbb{R}^{|V|}$$

$$U\in\mathbb{R}^{|V| \times D_h}$$

$$d$$ indicates dimension of word embeddings _(possibly with Word2Vec)_, 
$$D_h$$ is a dimension chosen in design. 
$$|V|$$ is the cardinality of the corpus.

Now the structure of RNN is clear, I will discuss the loss and gradient of RNN.
First, loss function looks like this.

$$J^{(t)}(\Theta) = - \sum^{|V|}_{j=1}y_{t,j}\times log(\hat{y}_{t,j})$$

$$J = {1\over T} \sum^{T}_{t=1}J^{(t)}(\Theta)$$

Here, cross entropy is used to define a distance of proabability.
The loss function of RNN looks a little bit complicated, but it is explained intuitively by looking at the structure of RNN.
$$J^{(t)}$$ indicates a loss at a timestamp $$t$$. 
Then $$J$$ indicates the overall loss of the RNN which is a summation over each timeframe and averaged by dividing the sum by the corpus size.

Second, gradient looks complicated as well.

$${\partial J \over \partial W} = \sum^T_{t=1}{\partial J_t \over \partial W}$$

$${\partial J_t \over \partial W} = \sum^t_{k=1} {\partial J_t \over \partial y_t}{\partial y_t \over \partial h_t}{\partial h_t \over \partial h_k}{\partial h_k \over \partial W}$$

However, this again is explained intuitively. Similar to how loss at each timestamp is added for an overall loss, overall gradient can decompose into a sum of gradient at each timestamp. Then, gradient at each timestamp is a sum of gradient that imposes change to $$y_t$$. This is a multiplication of local gradients in sequence.

### Problem With Vanila RNN
With a close look at how gradient is computed in vanila RNN, we find a problem. if elements in each local gradient is bigger than 1, the overall gradient will explode, and vanish otherwise. Each we call _exploding gradient_ and _vanishing gradient_ respectively. Thus, variation of RNNs are devised in newer literatures.
