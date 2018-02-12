---
layout: post
title: "GAN architecture explained"
date: 2018-01-28
---

first, an overview of GAN in more technical detail (pretty sure you know how things work by now on a high level overview):
https://danieltakeshi.github.io/2017/03/05/understanding-generative-adversarial-networks/

Code version of a standard GAN, in Keras:
https://github.com/wayaai/GAN-Sandbox/blob/master/gan.py


then, the different GANs we explore:

__Vanilla GAN to DCGAN (Deep Convolution GAN)__

http://guimperarnau.com/blog/2017/03/Fantastic-GANs-and-where-to-find-them#dcgans


__DCGAN to WGAN (Wasserstein GAN)__
https://lilianweng.github.io/lil-log/2017/08/20/from-GAN-to-WGAN.html


and then, let's go down into each of the individual parts in more detail below:


__Batch Normalisation__

For a summary, see the extract from paper by Sergey Ioffe, Christian Szegedy
https://arxiv.org/abs/1502.03167

Next, for a simpler breakdown, see this awesome explanation by Karl N. here:
https://gab41.lab41.org/batch-normalization-what-the-hey-d480039a9e3b


__Cross Entropy__

Amazing and simple explanation by Rob DiPietro here:
https://rdipietro.github.io/friendly-intro-to-cross-entropy-loss/

Very helpful to first understand entropy, then cross entropy, then KL divergence.


__RMSProp__
http://ruder.io/optimizing-gradient-descent/index.html#rmsprop


__tanh function__

Hyperbolic tangent function is essentially rescaling the sigmoid function from its range of 0 to 1, to -1 and 1.
There is horizontal scaling as well.

https://brenocon.com/blog/2013/10/tanh-is-a-rescaled-logistic-sigmoid-function/


__kernel initializer__
Initializations define the way to set the initial random weights of Keras layers.
https://faroit.github.io/keras-docs/1.2.2/initializations/

he_normal: Gaussian initialization scaled by fan_in (i.e. its number of inputs)

