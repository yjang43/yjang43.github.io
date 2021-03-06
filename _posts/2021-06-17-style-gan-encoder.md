---
title: A Method to Encode Real Data for StyleGAN
tags: ml, gan
img_url: /assets/img/style_mixing.jpeg
layout: blog
---

[StyleGAN developed by NVIDIA's research team](https://github.com/NVlabs/stylegan) has shown an exceptional result as to separating high level attributes, such as pose, face, hair, and etc., of generated data.
The success derived from the structural design of the network, via which each style is localized within the subset of the network. 
Style mixing performed by the network shows what it is capable of.

<!-- figure goes here -->
<p align="center">
    <img src="/assets/img/style_mixing.jpeg" alt="style mixing from style gan"  width="75%"/>
    <figcaption style="color: gray; font-size:12px; text-align:center">StyleGAN style mixing result from the paper</figcaption>
</p>

However, because of the common framework of GAN whose input is a noise vector from a normal distribution, mapping from an image to a latent code, the noise vector, is not explicit in contrast to the vice versa.
This limits generative model only to certain areas.
The limitation could be lifted off once there is a way to encode a real data to a latent code that model can understand.
The blog will mostly focus on this aspect after the simple explanation of styleGAN.

### Quick Overview of StyleGAN

The GAN framework consists two counterparts, generator and discriminator. 
Same architecture as one from progressive growing by [Karras et al](https://arxiv.org/pdf/1710.10196.pdf) is used for the discriminator.
 
Generator on the other hand is very unique to itself.
The generator is composed of mapping network and synthesis network
The first network maps a latent code to another intermediate latent code, which helps disentangle features.
The disentanglement is a state in which features lie in a sublinear space, and, thus, the output latent code could be considered a set of styles easy to distinguish (pose, hairstyle, gender, and etc.).
The second network then synthesize an image given the intermediate latent code.
This intermediate latent code is localized to each sublayers of the synthesis network through adaptive instance normalization which then allows separation of styles across the sublayers. 
This unique design hints its ability to localize high level styles.

<!-- style gan generator goes here -->
<p align="center">
    <img src="/assets/img/style_gan_structure.png" alt="style gan structure"  width="50%"/>
    <figcaption style="color: gray; font-size:12px; text-align:center">Structure of StyleGAN, mapping network on the left and synthesis network on the right</figcaption>
</p>

For simplicity, we denote latent code to $$z$$ and intermediate latent code to $$w$$ from now.


### Encoding Image for StyleGAN

The paper that introduced styleGAN is not explicit on a method to encode a real image to a latent code.
However, this does not make the potential of the task trivial as it widens the choice of real life applications of styleGAN.

I tried making a program to transfer hairstyle of a person to the user using style mixing but faced an issue of not being able to encode real images to feed through the generator.
This is just one example of applications if the issue is resolved. 
I thought of one way that can possibly remedy the issue, which is to train another model that inverses generator process.
However, some articles mentioned the difficulty of inversing generator of GAN framework.
Thus, I did more research and found out that I could directly optimize a latent code of an image with gradient descent.
Before the implementation, it needs to disambiguate the form of latent code because there are more than one latent codes to choose from in styleGAN: $$z$$ and $$w$$.
<p align="center">
    <img src="/assets/img/style_gan_pipeline.png" alt="style gan pipeline"  width="100%"/>
    <figcaption style="color: gray; font-size:12px; text-align:center">Where should we feed information of an image to generate the same image?</figcaption>
</p>
Thankfully, however, the study emprically proved that $$w$$ is somewhat able to disentangles features, and that was enough to make $$w$$ be the choice of latent code to convert the real images into.

I was able to find implementation of [styleGAN](https://github.com/rosinality/style-based-gan-pytorch) and its [encoder](https://github.com/jacobhallberg/pytorch_stylegan_encoder) that I could refer to.
With excitement, I decided to implement it myself in practice.
[__Here is the implementation of my code__](https://github.com/yjang43/style-based-gan-pytorch) and results of it will be further shared in this post.

The alogrithm for my initial attempt to optimize $$w$$ is the following:
<p align="center">
    <img src="/assets/img/style_gan_encoder_1.png" alt="style gan encoder algorithm"  width="75%"/>
    <figcaption style="color: gray; font-size:12px; text-align:center">Steps to optimize <em>w</em> with gradient descent</figcaption>
</p>
And below picture shows the progression of generated images from learned $$w$$.
Top pictures are data sampled from [CelebA](http://mmlab.ie.cuhk.edu.hk/projects/CelebA.html) dataset, and images below demostrate progression of learning.
<p align="center">
    <img src="/assets/img/encoder_progress_1.png" alt="style gan encoder algorithm result"  width="100%"/>
    <figcaption style="color: gray; font-size:12px; text-align:center">Generated images of optimized <em>w</em> through out training: <br>images are blur if loss is computed pixel by pixel</figcaption>
</p>
With the algorithm above, optimized $$w$$ resulted in blurred image.
To address this problem, I refered to other's implemenation where pretrained VGG-16 network is used to extract features from a generated image and a real image and then compute the loss between them.

The improved version to optimize $$w$$ is the following:
<p align="center">
    <img src="/assets/img/style_gan_encoder_2.png" alt="style gan encoder algorithm improved with VGG-16"  width="75%"/>
    <figcaption style="color: gray; font-size:12px; text-align:center">Steps to optimize <em>w</em> with gradient descent but improved with VGG-16 to extract features from</figcaption>
</p>
Picture below shows the progression, and the same sample is used for comparison.

<p align="center">
    <img src="/assets/img/encoder_progress_2.png" alt="style gan encoder algorithm improved result"  width="100%"/>
    <figcaption style="color: gray; font-size:12px; text-align:center">Generated images of optimized <em>w</em> through out training improved version with VGG-16: <br>fine images but some features are transformed</figcaption>
</p>
The generated pictures are less blur and tend to grab more specific features of an image, but some features are transformed amid optimization resulting in different person but similar looking.
This was very fascinating result as it proves that the meaning of extracted features are somewhat conceived similarly by both to human and the machine, but disappointing to realize that accurately converting an image to a latent code is a difficult task.

