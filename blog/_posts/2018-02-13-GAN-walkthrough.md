---
layout: post
title: "A GAN walkthrough (DCGAN and WGAN)"
date: 2018-02-13
---

## Introduction

Generative adversarial networks (GANs) - introduced by Ian Goodfellow in 2014, GANs can be used to generate new images that look real. Since 2016, advances have been made to generate images from text.

Generating new Pokemons:<br>
<img width="500" alt="pokemon-gan" src="https://user-images.githubusercontent.com/21985915/36364048-ba6e05d8-157c-11e8-91ee-87a25c60eb14.png">
<br>
_Credits: https://lilianweng.github.io/lil-log/2017/08/20/from-GAN-to-WGAN.html_


Generating flowers from text:<br>
<img width="500" alt="text-flower" src="https://user-images.githubusercontent.com/21985915/36364285-201dad42-157e-11e8-8bc3-0cb98ff84594.png">
<br>
_Credits: Generative Adversarial Text to Image Synthesis (Scott Reed et al 2016), https://arxiv.org/pdf/1605.05396.pdf_


## Overview

The idea is to train two models at the same time.

One model is the __Generator__: it takes random noise as input and produces fake images.<br>
The second model is the __Discriminator__: it takes both real images and fake images, and tries to identify which is real and which is fake.

<img height="400" alt="gan-model" src="https://user-images.githubusercontent.com/21985915/36364510-6a5329fe-157f-11e8-81b3-ee3a7d5d8d48.jpg">
<br>
_Credits: Chris Olah, https://twitter.com/ch402/status/793911806494261248/photo/1_


1) for people who are new to deep learning and GAN 

2) to learn about GAN, 

3) and be able to run their own GAN.

Disclaimer: All of the below is purely for educational purposes!

__Inspiration__

The brand’s logo can be a business’ most important asset - and companies can spend up to millions of dollars on the logo design. Can we leverage on machine learning and existing logo designs to generate logos (for free)?

__Goal__

Generate new logos from logos designed by humans

__Obtaining Dataset__

- 80,000 logos scraped from Wikipedia (for volume)
* To be frank the results using 80k images were not fantastic - so if it is too much trouble, skip downloading this and just scrape from Google Images.

To scrap, use `1_1_1_wikispider.py`, which saves all the links into 'items.csv' in a tab delimited csv.

```bash
scrapy runspider wikispider.py -o items.csv -t csv
```
Subsequently, there are a few options (I recommend the third!!):
1) download the files to your local drive and store them there
2) download the files directly into AWS (saves space on your local drive, but slow)
3) download the files to your local drive and upload to AWS via awscli (much faster)

Edit from line 54/55 for csv_filename and bucketname if necessary.
To download, use:
```bash
1_1_2_downloading_wiki_pics.py
```

- 2,000 logos scraped from Google Images (for quality)
Use this: [https://github.com/hardikvasa/google-images-download](https://github.com/hardikvasa/google-images-download) from Hardik Vasa
Use various keywords such as 'logo', 'logo circle', 'logo simple', 'logo vector', etc
Be sure to look through your logos manually and ensure that:
    - All white background
    - Avoid words as much as possible

Alternatively, you can download the folder of pictures I used, here: 


- 800 logos downloaded from Font Awesome (for black and white logos)
Download from here: https://fontawesome.com/, unzip and navigate into advanced-options, and raw-svg.
This contains all the svg files (meaning they are stored as vectors instead of pixels). 
To convert them into png files, 



Markdown | Less | Pretty
--- | --- | ---
*Still* | `renders` | **nicely**
1 | 2 | 3



__Cleaning Dataset__

__Modelling__

Overall GANs:

- Vanilla GAN: minimises the f-divergence between the real data distribution and the generated data distribution
- DCGAN (Deep Convolutional GAN): first major improvement on GAN architecture - comes with a set of constraints to make them stable to train. Usually the baseline to compare with other GANs
- cGAN (Conditional GAN): takes in conditional information that describes some aspect of the data (labeled points for eyes, nose for a face)
- WGAN (Wasserstein GAN): uses Wasserstein-1 distance (Earth-mover distance) so that even if the true and fake distributions do not overlap, the distance describes how far apart they are (instead of just returning 0 or infinity)

1) DCGAN

2) WGAN-GP

Reason: even WGANs can fail to converge. For the approximation of the Wasserstein (Earth-Mover) distance to be valid, WGAN imposed weight clipping constraints on the critic (discriminator) causing:
- The optimizer with gradient clipping to search the discriminator in a space smaller than 1-Lipschitz, biasing the discriminator toward simpler functions.
- Clipped gradients vanish or explode as they back-propagate through network layers.

Architecture guidelines:
- WGAN-GP (instead of clipping weights) penalises the norm of gradient of the discriminator with respect to its input.

http://mathworld.wolfram.com/LipschitzFunction.html


Intuitively, a Lipschitz continuous function is limited in how fast it can change: there exists a definite real number such that, for every pair of points on the graph of this function, the absolute value of the slope of the line connecting them is not greater than this real number; this bound is called a Lipschitz constant of the function (or modulus of uniform continuity). For instance, every function that has bounded first derivatives is Lipschitz.[1]

[https://en.wikipedia.org/wiki/Lipschitz_continuity]

https://lernapparat.de/improved-wasserstein-gan/




Results (loss graphs)

Tuning the best model