---
layout: post
title: "refining the GAN model"
date: 2018-01-25
---

__Model 1 - 1,000 images [POC_adapted_small.py]__
DCGAN
Dense, upsampling x 2, conv2d, output conv2d sigmoid
4 discri


__Model 1 - 80,000 images [POC_adapted_28_aws.py]__
DCGAN
Dense, upsampling x 2, conv2d, output conv2d sigmoid
4 discri


__Model 2 - 80,000 images [POC_increase_1_aws.py]__
DCGAN
Dense, upsampling x 4, 2 maxpooling, conv2d, output conv2d sigmoid
4 discri

Maxpool 2x2 and stride 2 reduces the image exactly into half. goes in a pair with an upsampling!
https://wiseodd.github.io/techblog/2016/07/18/convnet-maxpool-layer/


__Model 3 - 80,000 images [POC_increase_2_aws.py]__
DCGAN
Dense, upsampling x 4, 2 maxpooling, conv2d, output conv2d sigmoid
6 discri

Adv loss goes to 0 very quickly


__Model 4 - 80,000 images [POC_250118.py]__
DCGAN
Dense, upsampling x 2, conv2d, output conv2d tanh
4 discri


__To try:__
- increase batch size 1024
- reduce number of images (40,000)
- group similar images together (flatten and compare arrays, group similar ones together)
- try WGAN
- try vanilla GAN
- batch normalisation vs xavier initialisation