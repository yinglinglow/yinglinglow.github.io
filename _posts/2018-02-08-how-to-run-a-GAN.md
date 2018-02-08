---
layout: post
title: "How to run a GAN for beginners"
date: 2018-02-08
---

__TLDR:__

_Run DCGAN on MNIST (Keras on Tensorflow)_

Use the code by Rowel Atienza here:[https://github.com/roatienza/Deep-Learning-Experiments/blob/master/Experiments/Tensorflow/GAN/dcgan_mnist.py]

Blogpost here: [https://towardsdatascience.com/gan-by-example-using-keras-on-tensorflow-backend-1a6d515a60d0]


_Run DCGAN on your own black and white dataset_

Resize around >1500 images into 28x28 squares using the `resize_centre()` function in 0_split_pics_svg.py


_Run DCGAN on MNIST/ Anime/ Korean Face datasets (Tensorflow)_

Use the code by carpedm20 here:
[https://github.com/carpedm20/DCGAN-tensorflow]

Or use the code by Lilian Weng here:
[https://github.com/lilianweng/unified-gan-tensorflow]


_Run Improved WGAN on MNIST (Keras on Tensorflow)_

Use the code on keras-contrib here:
[https://github.com/keras-team/keras-contrib/blob/master/examples/improved_wgan.py]


_Run WGAN on your own B&W/RGB dataset_
Pre-process at least 200 images (but ideally 2000 or more) with 1_resize_to_array.py, and run on 2_WGAN_bw for your black and white dataset, or 2_WGAN_rgb for coloured dataset.