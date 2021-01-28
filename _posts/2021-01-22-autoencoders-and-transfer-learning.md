---
title: Autoencoders and Transfer Learning
tags: ml
img_url: /assets/img/autoencoder.png
layout: blog
---

## Autoencoders
Autoencoders is an unsupervised learning method to reduce a dimensionality of input.
The main idea is very similar to PCA, but more can be done with autoencoder, for example adding non-linearity or regularization. 
The main idea is to reconstruct an input with limitted features.
Here is a diagram of a simple autoencoder.


<p align="center">
    <img src="/assets/img/autoencoder.png" alt="JVP"  width="50%"/>
</p>

Basically, the network is composed of encoder and decoder, and lantent features (code) gives a high-level representation of input. 
Some tricks on autoencoder can result in better representation of latent features.
One I will mention is denosing autoencoders invented by Vincet(2008). By masking portion of input, it provides robust representation to partially destructed input, which aligns with the definition author proposed to be a "good" representation. Here is a [link](https://github.com/yjang43/ml_practice/blob/master/autoencoder.ipynb) that compares the results of autoencoders and desnoising autoencoder.

## Transfer Learning with Autoencoder
Transfer learning is to use information from source domains, abundant data, to support tasks in target domains, small data.
Glorot(2011) proposes that stacked denoising autoencoder (SDA) can extract intermediate representations from both source and target domain, and use that to help classfication task in target domain.
This idea extends to transfer learning with deep autoencoders (TLDA) by Zhuang (2015), which is a form of supervised learning unlike the usual approach with autoencoder. I have conducted an experiment to check its better performance [here](https://github.com/yjang43/ml_practice/blob/master/TLDA.ipynb).
Instead of using the same dataset from the paper, I used MNIST data.
The task was to check if a binary classfication of a number (ex. is the input '1' when compared with '3') can be transfered to another binary classification (ex. is the input '1' when compared with '5'). 
Surprisingly, TLDA performed much better than the baseline, in which I shared parameters trained in simple DNN to another task.
Here is a comparsion of learned feature between TLDA and baseline.
You can see the learned feature from TLDA is more generalized to both the source and target domains.

<p align="center">
    <img src="/assets/img/tlda_result.png" alt="JVP"  width="100%"/>
</p>