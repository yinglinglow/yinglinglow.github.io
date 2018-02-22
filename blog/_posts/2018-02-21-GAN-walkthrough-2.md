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

Hardik Vasa has written a very useful script to scrap images from Google, and you can find the script here: [https://github.com/hardikvasa/google-images-download](https://github.com/hardikvasa/google-images-download). Use various keywords such as _'logo'_, _'logo circle'_, _'logo simple'_, _'logo vector'_, etc to scrap different logos until you reach around 2000 images, and save them in a folder. 

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

<img width="330" src="https://user-images.githubusercontent.com/21985915/36363188-e31f908e-1575-11e8-9612-1b87209f1a81.png">


## Cleaning Dataset

Now that we have our dataset, let's clean it up which entails center-cropping and resizing them and converting them into one big array. In this case, we'll go with 56x56 (the model infrastructure is optimised for 56 x 56). It is possible to run on 28x28 (half the size) instead which will make the model will run faster(!!!) - however the quality of the output images will be lower. It is also possible to run on 112x112 (double the size) which will take a longer time for the model to run (gulps), but the quality of the output is (slightly) higher (whoppee!). Choose your poison:

<pre>
<div class="table-responsive">
  <table style="table">
      <tr>
        <td></td>
        <td align="center"><b>Runtime (using AWS p2x.large)</b></td>
      </tr>
      <tr>
        <td align="center">28x28</td> 
        <td align="center">~5 hours (5000 epochs)</td> 
      </tr>
      <tr>
        <td align="center">56x56</td> 
        <td align="center">~15 hours (3000 epochs)</td> 
      </tr>
      <tr>
        <td align="center">112x112</td> 
        <td align="center">~30 hours (3000 epochs)</td> 
      </tr>
  </table>
</div>
</pre>

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

Can I also say that I am working on this, BUT if you want to run WGAN, it only seems to work on the AWS environment for now :( I am going to try to work things out in Docker, but for the time being I would recommend:

1) Try DCGAN on GCP
2) Try WGAN on AWS

__For AWS__<br>
Set up your EC2 (p2x.large) instance using the ami 'ami-ccba4ab4' by Adrian Rosebrock on: 
[https://www.pyimagesearch.com/2017/09/20/pre-configured-amazon-aws-deep-learning-ami-with-python/](https://www.pyimagesearch.com/2017/09/20/pre-configured-amazon-aws-deep-learning-ami-with-python/)

Then, install AWSCLI and pandas. We are ready!!

```bash
pip3 install awscli
pip3 install pandas
```

__For GCP__<br>
Set up your gcloud compute instance using this: [https://medium.com/@howkhang/ultimate-guide-to-setting-up-a-google-cloud-machine-for-fast-ai-version-2-f374208be43](https://medium.com/@howkhang/ultimate-guide-to-setting-up-a-google-cloud-machine-for-fast-ai-version-2-f374208be43)

Then, install AWSCLI and Keras. We are ready!!

```bash
conda install -c anaconda keras-gpu
conda install -c conda-forge awscli
```

## Running the Model

We git clone and pull everything in (also copy from AWS your XTRAIN if it isn't in your github repo!)
`tmux` basically enables your model to continue running even if you close your terminal window. To re-connect to it, reopen your terminal window and ssh in to your instance as per normal. Then, enter `tmux attach` and you should see your model still running. For more details, see [here](https://askubuntu.com/questions/8653/how-to-keep-processes-running-after-ending-ssh-session).

the variables are just set in the environment (just makes it easier to change and run a different model or XTRAIN set) - so just change it to the names of the files you want to run. Date is just used in saved filenames.

BY THE WAY - I set the number of epochs to be 10,000 by default. Looking at my past loss plots, I gather that around 3000 or 5000 epochs is a good place to stop, so I force quit the training after I view the results and deem it sufficient. What is a good place to stop? When the losses stop decreasing (ideally it should tend towards 0). [Here](https://myurasov.github.io/2017/09/24/wasserstein-gan-keras.html) is a nice illustration, unforuntately my loss plots are not as pretty... something to improve on!

```bash
# git clone everything in
git clone https://github.com/yinglinglow/gan_walkthrough.git
cd gan_walkthrough
mkdir gan
mkdir gan_models

# open tmux
tmux

# change your variables accordingly if necessary
export XTRAIN=X_train_56_1366.pkl
export CODE=WGAN_180218_final
export DATE=210218

# run the model
python3 $CODE.py
```

Exciting stuff!!! You've got your model running and the results are OUT!!! CONGRATULATIONS!!!! <br>
Wait, wait, hang on a minute - where ARE the results? As it happens, it's saved in the directory `gan` and the models are saved in `gan_models`.

__To save to AWS S3 directly__<br>
aws s3 cp gan/* s3://yourbucketname/<br>
aws s3 cp gan_models/* s3://yourbucketname/

__To save files to your local computer__<br>
Run the below commands from your LOCAL terminal!!

```bash
# for AWS
scp -i yourpemfile.pem -r ubuntu@ec2-xx-xxx-xxx-xxx.us-west-2.compute.amazonaws.com:~/gan_walkthrough/gan/* .<br>
scp -i yourpemfile.pem -r ubuntu@ec2-xx-xxx-xxx-xxx.us-west-2.compute.amazonaws.com:~/gan_walkthrough/gan_models/* .

# for GCP
gcloud compute scp yourinstancename:gan_walkthrough/gan/* .
```

## Results

Here's a sneak peek for you... the results!!

__1) DCGAN (56x56)__<br>
Epoch: 3000 <br>
<img src='https://user-images.githubusercontent.com/21985915/36361986-a2bd0bac-156b-11e8-9d07-fb39dc348440.png' width="200"><br><br>

__2) WGAN-GP (56x56)__<br>
Epoch: 2000<br>
<img src='https://user-images.githubusercontent.com/21985915/36361988-a320d7e0-156b-11e8-961f-13719a3c1088.png' width="200"><br>
Epoch: 2500<br>
<img src='https://user-images.githubusercontent.com/21985915/36361989-a351681a-156b-11e8-9220-c514a66e1b1d.png' width="200"><br>

__3) WGAN-GP (112x112)__<br>
Epoch: 2500<br>
<img src='https://user-images.githubusercontent.com/21985915/36516668-1fc93008-17ba-11e8-8c44-7a66ebf8cbd5.png' width="200">
## Future Improvements

Great - you've managed to generate images using GANs!! Now what?

There are many other interesting things out there to try out - let me pique your interest a litte:
- [Variational Autoencoders](http://kvfrans.com/variational-autoencoders-explained/)
- [Generative Latent Optimization (GLO)](https://arxiv.org/abs/1707.05776)
- [Creative Adversarial Network (CAN)](https://arxiv.org/abs/1706.07068)

I'll leave you to it then... until the next time, adios!