---
title: Bayesian Probabilities
tags: ml bishop
img_url: /assets/img/bayesian.png
layout: blog
---

The two interpretation of probability are frequentist and Bayesian views. 
One good example of frequentist view is training neural network, in which the model is trained minimizing negative log likelihood (maximizing likelihood in other words) observing train data.
Opposite to frequentist view which relies merely on observation of occurrences, Bayesian involves _prior probability_ to prevent some of the shortcomings of frequentist view such as overfitting on small dataset.

<p align="center">
    <img src="/assets/img/bayesian.png" alt="bayesian cartoon"  width="75%"/>
    <figcaption style="color: gray; font-size:12px; text-align:center"></figcaption>
</p>

Bayesian approach can be further explained from Bayes' rule.
Bayes' rule is the following,

<div style="width: 100%; overflow: scroll;">
$$p(D | \mathbf{w}) = {p(D | \mathbf{w}) p(\mathbf{w}) \over p(D)}$$
</div>

$$p(D | \mathbf{w})$$ being _posterior probability_, $$p(D | \mathbf{w})$$ being _likelihood_ $$p(\mathbf{w})$$ being _prior probability_.
Thus, the following relation can be drawn,

<div style="width: 100%; overflow: scroll;">
$$\text{posterior} \propto \text{likelihood} \times \text{prior}$$
</div>

This relation provides a profound note that maximizing likelihood function results in maximizing posterior probability.
However, the process of modeling with such process will only work with an appropriate prior probability.
I will further discuss regarding the details of other aspects of Bayesian view in later blog posts.


___
## Reference
[Bishop, Christopher M. (2006). Pattern recognition and machine learning. New York :Springer,](https://www.microsoft.com/en-us/research/uploads/prod/2006/01/Bishop-Pattern-Recognition-and-Machine-Learning-2006.pdf)
