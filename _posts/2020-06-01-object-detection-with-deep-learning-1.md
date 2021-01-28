---
title: Readings about Object Detection
tags: ml ai paper
img_url: /assets/img/object_detection.jpeg
layout: blog
---

I am reading a journal from _IEEE_ and decided to 
record some of the learning points from the reading.
_Object Detection With Deep Learning: A Review_ by Zhao
and others thoroughly reviews on the development of
object detection and its future as well.

There are two requirements to detect object:
1. _object localization_ - where the object is located
from the frame
2. _object classification_ - what is the object

The author states that _supported vector machine_ (SVM)
was used often for classification, so let's learn more
about that. 

SVM is a classification technique that can be used
in supervised learning model. The idea is that
if there are _k_-dimension vectors than it separates
vectors with _(k-1)_-dimension hyperplane that 
maximizes distance from nearest data point from
each side is maximized.

However, occurrence of convolutional neural network (CNN)
was the game changer. Changing technology brings
these advantages to CNN:
1. ImageNet - big ass data set to train the model
2. faster GPU specs
3. better design of structures like _autoencoder_ or 
_restricted Boltzmann machine_ to avoid preexisting
issues


