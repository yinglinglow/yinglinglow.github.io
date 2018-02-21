---
layout: post
title: "A Beginner's Guide To GAN (Generative Adversarial Network) - Part 2"
date: 2018-02-20
---

In Part 2 of this guide, we are going to walkthrough the actual steps of running your very own GAN and create (real-looking) fake images! If you missed it and want to learn more about the general background behind GANs, check out [Part 1]('https://www.yinglinglow.com/blog/2018/02/13/GAN-walkthrough')!

<a href='https://www.yinglinglow.com/blog/2018/02/13/GAN-walkthrough'>__Part 1 - GANs in general__</a><br>
<a href='https://www.yinglinglow.com/blog/2018/02/13/GAN-walkthrough#introduction'>Introduction</a><br>
<a href='https://www.yinglinglow.com/blog/2018/02/13/GAN-walkthrough#general-model-structure'>General Model Structure</a><br>
<a href='https://www.yinglinglow.com/blog/2018/02/13/GAN-walkthrough#types-of-gan'>Types of GAN</a>

__Part 2 - GAN Walkthrough__<br>
<a href='#obtaining-dataset'>Obtaining Dataset</a><br>
<a href='#cleaning-dataset'>Cleaning Dataset</a><br>
<a href='#modelling'>Modelling</a><br>
<a href='#set-up-cloud-platform'>Set up Cloud Platform</a><br>
<a href='#running-the-model'>Running the Model</a><br>
<a href='#results'>Results</a><br>
<a href='#future-improvements'>Future Improvements</a><br>

The focus of this project will be to generate logos - yes, brand logos! A brand logo can be a business’ most important asset - and companies can spend up to millions of dollars on the logo design! (Random: Do you know which logo cost the most to design? Check [this](https://thinkmarketingmagazine.com/7-most-expensive-logos-world/) out!) Can we leverage on machine learning and existing logo designs to generate logos (for free)?

All the necessary scripts (and data, if you wish) are available at [my Github](https://github.com/yinglinglow/gan_walkthrough). Feel free to open up the .py files to see what's actually going inside (I try to comment as much as possible)! Now let us begin, as we do with all data projects, by gathering our data.

## Obtaining Dataset

If you have had experience with other machine algorithms before, you might think that we would need many images for our training set (upwards of 10,000, and more!). The good news is, GANs learn via the fake images (created by the Generator) as well and hence do not require such a large dataset! In fact running on around 1,000 - 2,000 is more than sufficient in this case!

__Scrape 2,000 logos from Google Images__

Hardik Vasa has written a very useful script to scrap images from Google, and you can find the script here:[https://github.com/hardikvasa/google-images-download](https://github.com/hardikvasa/google-images-download). Use various keywords such as _'logo'_, _'logo circle'_, _'logo simple'_, _'logo vector'_, etc to scrap different logos until you reach around 2000 images, and save them in a folder. 

Be sure to look through your logos manually and ensure that they are of good quality; as much as possible, choose logos which are clean and simple (not many complicated lines or words, or designs), and have a white background. Don't worry too much about white space around the logo as we will be center-cropping those out later using a script. If you have an image with multiple logos arranged in a grid and need to split them into individual photos, use this script below:

```bash
python3 1_3_split_pic.py --filename=abc.jpeg --col=3 --row=2
```

ALTERNATIVELY, you can simply download the folder of pictures I used, from `logos_originals_1367.zip` [here](https://github.com/yinglinglow/gan_walkthrough/blob/master/logos_originals_1700.zip?raw=true).

<img width="400" alt="logo_originals" src="https://user-images.githubusercontent.com/21985915/36466353-aefc2484-1714-11e8-94b2-b35364527a01.png">


Now the above is the only dataset that you need, but if you want to try it out on other datasets, you can try the below:
- Scrap 80,000 logos from a [Wikipedia page](https://commons.wikimedia.org/wiki/Category:Unidentified_logos), using:

```bash
# scrape all the links for the images from and save it to an items.csv file
scrapy runspider 1_1_wikispider.py -o items.csv -t csv

# download all the images from the links and save it locally
python3 1_2_downloading_wiki_pics.py --filename=items.csv --local=True
```

<img width="400" src="https://user-images.githubusercontent.com/21985915/36363186-dd9d4e80-1575-11e8-98d5-aa797107ee4c.png">


- Download 800 logos from [Font Awesome](https://fontawesome.com/) (black and white logos). You will need to unzip and navigate into the folder 'advanced-options', and 'raw-svg'. This folder contains all the svg files (svg file types, unlike jpeg and png, means that the images are stored as vectors instead of pixels). To convert them into png files, use:

```bash
python3 1_4_convert_svg_png.py --path=/Users/xxx/svgtopng/
```

<img width="400" src="https://user-images.githubusercontent.com/21985915/36363188-e31f908e-1575-11e8-9612-1b87209f1a81.png">


## Cleaning Dataset

Now that we have our dataset, let's clean it up which entails center-cropping and resizing them and converting them into one big array. In this case, we'll go with 56x56 (the model infrastructure is optimised for 56 x 56). It is possible to run on 28x28 (half the size) instead which will make the model will run faster(!!!) - however the quality of the output images will be lower. It is also possible to run on 112x112 (double the size) which will take a longer time for the model to run (gulps), but the quality of the output is (slightly) higher (whoppee!). Choose your poison:

 | Runtime
 ---|---
28x28 | ~3-5 hours
 ---|---
56x56 | ~15 hours
 ---|---
112x112 | ~24 hours


Subsequently, if your array size is less than 100MB, you can upload it to github together with the scripts (so that when you do a git clone you pull all your scripts and data in, all at once)! This is likely the case if you are dealing with less than 2000 images of size 28x28 or 56x56.

Else, I would recommend to upload it to AWS S3 using the AWS CLI - remember to upload it to the same region as where you are spinning your instance up in! If you're following mine below later, set it up in Oregon (even though my human self is based here in Singapore :'( )

If you need a tutorial on how to set up an AWS S3 bucket, the [in-house one by AWS](http://docs.aws.amazon.com/AmazonS3/latest/gsg/SigningUpforS3.html) is pretty good I must say.

```bash
# to center crop and resize the images
python3 1_5_resize_centre.py --path=/Users/xxx/to_resize/ --size=56

# to convert all pictures to one big array and pickle it
python3 1_6_resize_to_array.py --path=/Users/xxx/resized/ --height=56 --target_path=/Users/xxx/ --augment=True

# optional: to upload to AWS S3 using the AWS CLI
aws s3 cp /Users/xxx/X_train_56_1700.pkl s3://yourbucketname/
```

## Modelling

Long story short - it is extremely challenging to build your GAN model from scratch, especially if you don't have much background in Tensorflow!! As such I would recommend to start off first by working on someone else's base code, familiarise yourself with what is actually going on first, encourage yourself with some output/verify things can actually work (lol) and THEN code it yourself. I actually tried to code it from scratch but honestly... it was wayyyy too painful given my lack of knowledge in and too time consuming (I had time constraints!!). But I encourage you to try it. Try it!!

Anyway, the DCGAN code we are working with today comes from [https://github.com/roatienza/Deep-Learning-Experiments/blob/master/Experiments/Tensorflow/GAN/dcgan_mnist.py](https://github.com/roatienza/Deep-Learning-Experiments/blob/master/Experiments/Tensorflow/GAN/dcgan_mnist.py). The WGAN-GP base code was from [https://github.com/keras-team/keras-contrib/blob/master/examples/improved_wgan.py](https://github.com/keras-team/keras-contrib/blob/master/examples/improved_wgan.py).

Honestly, I chose these two above because they are Keras code using a Tensorflow backend (basically Keras is a simplified interface to Tensorflow). DCGAN has a cool write-up [here](https://towardsdatascience.com/gan-by-example-using-keras-on-tensorflow-backend-1a6d515a60d0) so check that out for a breakdown first, and the WGAN code is heavily commented, so that makes things simpler. For a more detailed breakdown, check mine out! #selfplug lol. Basically I added more comments (to try and simplify things a little more, even for myself!), swapped them out into RGB models (instead of black and white), added in some image augmentation (I thought 1300 images wasn't going to be enough oops), and some loss tracking plots and stuff. Minor edits mainly. Oh yes I must say, so many other models out there only work on MNIST/black and white datasets! grrr. Otherwise they were too complicated for me to understand... sad.  

For guys who want to run the images on a smaller or bigger image sizes, honestly the quick hack is just to remove/add a chunk of this code in the Generator, which basically just halves/doubles the size of the final output:

```python
    model.add(Conv2DTranspose(f2, conv_window, strides=stride, padding='same'))
    model.add(BatchNormalization())
    model.add(LeakyReLU())
```

Things become more complicated if you want to run on a black and white dataset instead (because mine is configured for RGB), but essentially because black and white images are just 1 channel (instead of 3), and hence instead of having an array shape of [num_of_images, 56, 56, 3], should have an array shape of [num_of_images, 56, 56]. For that I gotta admit, you have to change it yourself, OR just use their original code lol.

## Set up Cloud Platform
__(if you do not have a GPU)__

Let's set up the environment to run our scripts in - this requires a GPU! If you don't have one (like me) - let's get this up on AWS (Amazon Web Services) or GCP (Google Cloud Platform). I personally prefer AWS (which is also used in many more companies), but GCP gives $300 free credits (see [here](https://cloud.google.com/free/docs/frequently-asked-questions) - caveat is that to use GPU you need to pay $35 dollars in advance which is returned in credits!). Check out Microsoft Azure or even Alibabacloud - I think I heard about free credits from Alibabacloud too (see [here](https://www.alibabacloud.com/campaign/free-trial))!





Use `setup-aws.sh` for AWS EC2, or `setup-gcp.sh` for Google Cloud Platform.
*In progress - DCGAN works fine on GCP but WGAN has some issues, potentially due to installation problems :(



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