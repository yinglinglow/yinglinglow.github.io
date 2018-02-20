---
layout: post
title: "A GAN walkthrough (DCGAN and WGAN)"
date: 2018-02-20
---


<a href='https://www.yinglinglow.com/blog/2018/02/13/#gans-in-general'>__Part 1 - GANs in general__</a><br>
<a href='https://www.yinglinglow.com/blog/2018/02/13/#introduction'>Introduction</a><br>
<a href='https://www.yinglinglow.com/blog/2018/02/13/#overview'>Overview</a><br>
<a href='https://www.yinglinglow.com/blog/2018/02/13/#general-model-structure'>General Model Structure</a><br>
<a href='https://www.yinglinglow.com/blog/2018/02/13/#types-of-gan'>Types of GAN</a>

<a href='#GAN-walkthrough-2'>__Part 2 - GAN Walkthrough__</a><br>
<a href='#obtaining-dataset'></a>Obtaining Dataset</a><br>
<a href='#cleaning-dataset'></a>Cleaning Dataset</a><br>
<a href='#set-up-cloud-platform'></a>Set up Cloud Platform</a><br>
<a href='#modelling'></a>Modelling</a><br>

# GAN Walkthrough

## Obtaining Dataset

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


## Cleaning Dataset

__1) Center crop and resize to 56 x 56__

To center crop and resize them, use:
```bash
python3 1_5_resize_centre.py --path=/Users/xxx/to_resize/
```

__2) Append to array__

To convert all pictures to one big array and pickle it, use:
```bash
python3 1_6_resize_to_array.py --path=/Users/xxx/resized/ --height=56 --target_path=/Users/xxx/ to_augment=True
```

__3) Upload array to S3__

To upload to AWS S3 using AWS CLI, use:
```bash
aws s3 cp /Users/xxx/X_train_56_1700.pkl s3://gan-project/
```

## Set up Cloud Platform
__(if you do not have a GPU)__

Use `setup-aws.sh` for AWS EC2, or `setup-gcp.sh` for Google Cloud Platform.
*In progress - DCGAN works fine on GCP but WGAN has some issues, potentially due to installation problems :(

## Modelling

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