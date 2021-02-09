---
title: Distillation in Neural Networks
tags: ml
img_url: /assets/img/distillation.jpg
layout: blog
---

First I approach the idea distillation from the reading about DistilBERT (Sanh, 2019). 
I decided to learn more about distillation technique, and headed to where the paper directed me: _"Distillling the Knowledge in a Neural Network" (Hinton, 2015)_.
Distillation is one of trasfer learning techniques __to achieve the same or similar performance as big model with a small model.__ 

## Motif
At a glance, converting a big model to a small model seems redundant because both models' performance will be similar (becase that is the goal of distillation -- duh!). 
However, distillation provides speed in inference phase, and this is important in real-time applications such as autonomous driving or chatbot customer service. 

## Distillation: Big Model to Small Model
Idea seems direct, big model to small model, but steps need a closer look.
Prior to the discussion of nitty gritty of its implementation, intuition of where the knowledge comes from is important.
The authors find this information from soft label, or soft target.
Unlike hard label that deterministically indicates a label of an input, soft label indicates probability, and what the distribution of probability provides is useful information.
For example, if model was to classify dogs from other objects such as cats and ships, it may misclassify dogs to be cats rather than to be ships. Probability distribution contains information that describes such cases.

<div style="width: 100%; overflow: scroll;">
$$ \text{soft label: } \begin{pmatrix}\text{dog: }0.8 & \text{cat: }0.19 & \text{ship: }0.01\end{pmatrix} \\
\text{hard label: } \begin{pmatrix}\text{dog: }1 & \text{cat: }0 & \text{ship: }0\end{pmatrix} $$
</div>

With this intuition, we naturally move towards softmax to generate the probability distribution. However, softmax alone hardly provide a useable distribution. 
Thus, we need to add and adjust additional term, temperature.
This is where the word, distillation, comes from

<div style="width: 100%; overflow: scroll;">
$$ q_i = {exp(z_i/T)\over \sum_j exp(z_j / T)}\text{, T is temperature} $$
</div>

<h4 align="center">
    "... to raise the temperature of the final softmax until the cumbersome [big] model produces a suitably soft set of targets".
</h4>

<p align="center">
    <img src="/assets/img/distillation.jpg" alt="distillation"  width="50%"/>
    <figcaption style="color: gray; font-size:12px; text-align:center">Distillation is the process of separating components of a mixture based on different boiling points.</figcaption>
</p>

The result of increaseing a temperature is explained with the following diagram. 
As temperature rises, more consideration is put on unlikely categories. 
In other words, when model was to predict an input to be a dog, it more acknowledges the similarity between cats and dogs as temperature grows. 


<p align="center">
    <img src="/assets/img/temperature.png" alt="probability distribution as temperature increases"  width="80%"/>
</p>

## Final Loss Function
There can be many ways to include distillation to reduce big model to smaller model. The authors suggest to take weighted sum of two loss functions for classficiation task.

* Set the same temperature for both big and small models and cross entropy of with soft labels.
* Like normal classficiation task, cross entropy with hard label.

Then, $$ T^2 $$  should be multiplied to the first loss function as the temperature included in the loss function will decrease the flow of its gradient by $$ 1 \over T^2 $$.