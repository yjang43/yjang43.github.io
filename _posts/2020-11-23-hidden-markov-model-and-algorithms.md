---
title: Hidden Markov Model and Algorithms - 1
tags: ml hmm
img_url: /assets/img/hmm.png
layout: blog
---

### Hidden Markov Model
HMM is easy to understand knowing its components first.
HMM can represent a sequence of _states_ and _symbol_.
Symbol is what we observe, 
and state is an information about the symbol.
Simply said, HMM focuses on understanding the meaning of the observed sequence (implication under observation).

If state at location _i_ is denoted as $$\pi_i$$.
Probability of a state _k_ to state _l_ is expressed as,

<div style="width: 100%; overflow: scroll;">
$$a_{kl} = P(\pi=l|\pi=k)$$
</div>

This is called _transition probability_ and carries an important role in explaining the first part of HMM, states.
Next, the probability of a symbol in a given state can be denoted as,

<div style="width: 100%; overflow: scroll;">
$$e_k(b) = P(x_i=b|\pi=k)$$
</div>
This is called _emission probability_ and likewise this explains the second part of HMM, symbol.

### Viterbi
One question to ask while working with a squential data whose formation is determined by probability is, 
"what is the most probable meaning of the observed sequence?"
This in other words is to ask what the most probable _state path_ is.

<div style="width: 100%; overflow: scroll;">
$$\tilde{\pi} = argmax_\pi P(x, \pi)$$
</div>

Viterbi algorithm provides a solution to this question. The heart of the algorithm is dynamic programming, and the idea comes from the following:

_If the last state $$\pi_n$$ completes the most probable state path, it is completed by connecting to the most probable state path up to $$\pi_{n-1}$$_

<div style="width: 100%; overflow: scroll;">
$$v_l(i+1) = e_l(x_{i+1})\max_k(v_k(i)a_{kl})$$
</div>

Here _i_, _l_, _k_ indicate a position in the squence, a state in question, and a previous state respectively.

### Forward Algorithm

Forward algorithm gives a probabilty of a sequence, $$P(x)$$ 
simple as that.

<div style="width: 100%; overflow: scroll;">
$$P(x)=\sum_\pi P(x, \pi)$$
</div>

This is a marginalization of states. This can be solved with dynamic programming because 
<div style="width: 100%; overflow: scroll;">
$$P(x_1...x_{i}, \pi_i) = \sum_k^l P(x_1...x_i, \pi_i, \pi_{i-1}=k)$$
</div>

Thus,

<div style="width: 100%; overflow: scroll;">
$$when \ f_k(i) = P(x_1...x_i, \pi_i=k)$$
</div>

<div style="width: 100%; overflow: scroll;">
$$f_l(i+1) = e_l(x_{i+1})\sum^l_k f_k(i)a_{kl}$$
</div>

The weighted sum of the probabilities of previous sequence from each state is quite intuitive. 

[comment]: <> (### Backward Algorithm)

[comment]: <> (This algorithm is very similar to the forward algorithm. Dyanmic programming is used again but starts from the end. The goal is to )