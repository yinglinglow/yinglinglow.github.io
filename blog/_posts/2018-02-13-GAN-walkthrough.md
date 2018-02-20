---
layout: post
title: "A GAN walkthrough (DCGAN and WGAN)"
date: 2018-02-13
---

<a href='#gans-in-general'>__Part 1 - GANs in general__</a><br>
<a href='#introduction'>Introduction</a><br>
<a href='#overview'>Overview</a><br>
<a href='#general-model-structure'>General Model Structure</a><br>
<a href='#types-of-gan'>Types of GAN</a>

<a href='https://www.yinglinglow.com/blog/2018/02/20/GAN-walkthrough-2'>__Part 2 - GAN Walkthrough__</a><br>
<a href='https://www.yinglinglow.com/blog/2018/02/20/GAN-walkthrough-2/#obtaining-dataset'>Obtaining Dataset</a><br>
<a href='https://www.yinglinglow.com/blog/2018/02/20/GAN-walkthrough-2/#cleaning-dataset'>Cleaning Dataset</a><br>
<a href='https://www.yinglinglow.com/blog/2018/02/20/GAN-walkthrough-2/#modelling'>Modelling</a><br>


# GANs in general
## Introduction

Generative adversarial networks (GANs) were introduced by Ian Goodfellow in 2014. GANs can be used to generate new images that look real and since 2016, even to generate images from text.

Generating new Pokemons:<br>
<img width="500" alt="pokemon-gan" src="https://user-images.githubusercontent.com/21985915/36364048-ba6e05d8-157c-11e8-91ee-87a25c60eb14.png">
<br>
_Credits: https://lilianweng.github.io/lil-log/2017/08/20/from-GAN-to-WGAN.html_


Generating flowers from text:<br>
<img width="500" alt="text-flower" src="https://user-images.githubusercontent.com/21985915/36364285-201dad42-157e-11e8-8bc3-0cb98ff84594.png">
<br>
_Credits: Generative Adversarial Text to Image Synthesis (Scott Reed et al 2016), https://arxiv.org/pdf/1605.05396.pdf_


## Overview

The whole idea of GAN is to train two models at the same time.

One model is the __Generator__: it takes random noise as input and produces fake images.<br>
The second model is the __Discriminator__: it takes both real images and fake images, and tries to identify which is real and which is fake.

<img height="400" alt="gan-model" src="https://user-images.githubusercontent.com/21985915/36364510-6a5329fe-157f-11e8-81b3-ee3a7d5d8d48.jpg">
<br>
_Credits: Chris Olah, https://twitter.com/ch402/status/793911806494261248/photo/1_


## General Model Structure

First, we build the generator. The generator captures the distribution of the training data and produces the most likely outcome, to maximise the probability of the discriminator making a mistake. 

It takes in random numbers (say, an array of 100 points) as an input, and projects it to a 3D array. Using transposed convolution (also known as fractionally strided convolutions/ deconvolution), the image size increases (e.g. from a 2x2 to a 4x4).

The output is an image, e.g. a 56x56x3 array which gives a 56x56 RGB image (3 channels).

<table style="width:100%">
  <tr>
    <td><b>Transposed convolution</b></td>
  </tr>
  <tr>
    <td>No padding, no strides, transposed</td> 
    <td>No padding, strides, transposed</td> 
    <td>Padding, strides, transposed</td>
  </tr>
  <tr>
    <td align="center"><img height="100" src="https://user-images.githubusercontent.com/21985915/36368254-5d7d38d4-1591-11e8-9850-5db58dcf21e8.gif"></td>
    <td align="center"><img height="100" src="https://user-images.githubusercontent.com/21985915/36368255-5dae0c98-1591-11e8-8c04-69b0ac63f68b.gif"></td>
    <td align="center"><img height="100" src="https://user-images.githubusercontent.com/21985915/36368257-5de348d6-1591-11e8-8fb7-ad6d6348c44b.gif"></td>
  </tr>
</table>
_Credits: vdumoulin, https://github.com/vdumoulin/conv_arithmetic_

__Generator architecture__<br>
<img width="500" src='https://user-images.githubusercontent.com/21985915/36368270-6860664a-1591-11e8-969d-d500396dca84.png'>
<br>
_Credits: https://towardsdatascience.com/gans-part2-dcgans-deep-convolution-gans-for-generating-images-c5d3c7c3510e_


Then, we build the discriminator. It is a discriminative model to estimate the probability that a sample came from the training data rather than generator.

The discriminator takes in an image (either real or fake), passes the image through convolution layers and reduces it in size (e.g. 4x4 to 2x2). The output is from -1 to 1, where -1 is fake, and 1 is real.

__Convolution__<br>
<img src='https://user-images.githubusercontent.com/21985915/36372514-3e585254-15a0-11e8-8976-901a19b7c3f7.gif'>
_Credits: vdumoulin, https://github.com/vdumoulin/conv_arithmetic_

__Discriminator architecture__<br>
<img width="500" src='https://user-images.githubusercontent.com/21985915/36368309-9c303e3c-1591-11e8-84ee-ccdaff524ab2.png'>
<br>
_Credits: https://hackernoon.com/how-do-gans-intuitively-work-2dda07f247a1_

## Types of GAN

There are several varieties of GAN: 

  __Vanilla GAN:__ minimises the f-divergence between the real data distribution and the generated data distribution
    
  __DCGAN (Deep Convolutional GAN):__ first major improvement on GAN architecture - comes with a set of constraints to make them stable to train. Usually the baseline to compare with other GANs
    
  __cGAN (Conditional GAN):__ takes in conditional information that describes some aspect of the data (labeled points for eyes, nose for a face)
    
  __WGAN (Wasserstein GAN):__ uses Wasserstein-1 distance (Earth-mover distance) so that even if the true and fake distributions do not overlap, the distance describes how far apart they are (instead of just returning 0 or infinity)


In <a href='#GAN-walkthrough-2'>__Part 2 - GAN Walkthrough__</a>, we will go through the actual steps of running your very own GAN!

