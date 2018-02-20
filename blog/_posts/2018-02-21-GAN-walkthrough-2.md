---
layout: post
title: "A GAN walkthrough (DCGAN and WGAN) - Part 2"
date: 2018-02-20
---


<a href='https://www.yinglinglow.com/blog/2018/02/13/GAN-walkthrough'>__Part 1 - GANs in general__</a><br>
<a href='https://www.yinglinglow.com/blog/2018/02/13/GAN-walkthrough#introduction'>Introduction</a><br>
<a href='https://www.yinglinglow.com/blog/2018/02/13/GAN-walkthrough#overview'>Overview</a><br>
<a href='https://www.yinglinglow.com/blog/2018/02/13/GAN-walkthrough#general-model-structure'>General Model Structure</a><br>
<a href='https://www.yinglinglow.com/blog/2018/02/13/GAN-walkthrough#types-of-gan'>Types of GAN</a>

__Part 2 - GAN Walkthrough__<br>
<a href='#obtaining-dataset'>Obtaining Dataset</a><br>
<a href='#cleaning-dataset'>Cleaning Dataset</a><br>
<a href='#set-up-cloud-platform'>Set up Cloud Platform</a><br>
<a href='#running-the-model'>Running the Model</a><br>
<a href='#results'>Results</a><br>
<a href='#future-improvements'>Future Improvements</a>

# GAN Walkthrough
## Obtaining Dataset

There are 3 different ways to obtain your starting images - I recommend method 2 (scraping from Google Images).

__1) Scrape 80,000 logos from Wikipedia__

Scrape all the links for the images from [Wikipedia page](https://commons.wikimedia.org/wiki/Category:Unidentified_logos) and save it to and items.csv file, using:
```bash
scrapy runspider 1_1_wikispider.py -o items.csv -t csv
```

To download all the images, use:
```bash
python3 1_2_downloading_wiki_pics.py --filename=items.csv --local=True
```

<img width="400" src="https://user-images.githubusercontent.com/21985915/36363186-dd9d4e80-1575-11e8-98d5-aa797107ee4c.png">

__2) Scrape 2,000 logos scraped from Google Images__

Use this: [https://github.com/hardikvasa/google-images-download](https://github.com/hardikvasa/google-images-download) from Hardik Vasa

Use various keywords such as _'logo'_, _'logo circle'_, _'logo simple'_, _'logo vector'_, etc. Be sure to look through your logos manually and ensure that they are of good quality. If you need to split logos arranged in a grid into individual photos, use:

```bash
python3 1_3_split_pic.py --filename=abc.jpeg --col=3 --row=2
```

Alternatively, you can simply download the folder of pictures I used, from `logos_originals_1700.zip`.

<img width="400" src="https://user-images.githubusercontent.com/21985915/36361926-0df0aa24-156b-11e8-964e-42cb13c0de9c.png">


__3) Download 800 logos from Font Awesome (black and white)__

Download from here: [https://fontawesome.com/](https://fontawesome.com/)

Unzip and navigate into advanced-options, and raw-svg.

This contains all the svg files (meaning they are stored as vectors instead of pixels). To convert them into png files, use:
```bash
python3 1_4_convert_svg_png.py --path=/Users/xxx/svgtopng/
```

<img width="400" src="https://user-images.githubusercontent.com/21985915/36363188-e31f908e-1575-11e8-9612-1b87209f1a81.png">


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

## Running the Model

DCGAN base code was from [https://github.com/roatienza/Deep-Learning-Experiments/blob/master/Experiments/Tensorflow/GAN/dcgan_mnist.py](https://github.com/roatienza/Deep-Learning-Experiments/blob/master/Experiments/Tensorflow/GAN/dcgan_mnist.py)

WGAN-GP base code was from [https://github.com/keras-team/keras-contrib/blob/master/examples/improved_wgan.py](https://github.com/keras-team/keras-contrib/blob/master/examples/improved_wgan.py)

Use `run-model.sh` to run the model. 
Change the variables accordingly to whichever model or XTRAIN set you are using.

```bash
git clone https://github.com/yinglinglow/gan_walkthrough.git
cd gan_walkthrough
mkdir gan
mkdir gan_models

# open tmux
tmux

# change your variables accordingly if necessary
export XTRAIN=X_train_56_1700.pkl
export CODE=WGAN_180218_11am
export DATE=180218

# run the model
python3 $CODE.py
```


## Results

__1) DCGAN__

<br><br>
Epoch: 3000

<img src='https://user-images.githubusercontent.com/21985915/36361986-a2bd0bac-156b-11e8-9d07-fb39dc348440.png' width="200">

<br><br>

__2) WGAN-GP__

<br><br>
Epoch: 2000

<img src='https://user-images.githubusercontent.com/21985915/36361988-a320d7e0-156b-11e8-961f-13719a3c1088.png' width="200">


Epoch: 2500

<img src='https://user-images.githubusercontent.com/21985915/36361989-a351681a-156b-11e8-9220-c514a66e1b1d.png' width="200">


Epoch: 3000

<img src='https://user-images.githubusercontent.com/21985915/36361990-a3885618-156b-11e8-9975-dc16a7ca323a.png' width="200">


## Future Improvements

Other models to attempt: 
- Variational Autoencoders
- Generative Latent Optimization (GLO)
- Creative Adversarial Network (CAN)