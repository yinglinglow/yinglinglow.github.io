---
layout: post
title: "Object Detection with Tensorflow"
date: 2018-06-10
---

Talking to a bunch of friends yesterday about some data projects, and one of them involved scanning aerial videos of grasslands for endangered animals.
So here I am today, trying out the Google TensorFlow Object Detection API - one year late, but better late than never right? LOL

I'm beginning first with images, but I'd love to eventually progress to videos, and training it to identify custom objects.

---

__Installation__

I am installing the below on a Macbook (aka I have no GPU).


1) Install Tensorflow

Thankfully I already have Tensorflow installed. If you don't, follow the instructions here: https://www.tensorflow.org/install/


2) Install dependencies

Follow this: https://github.com/tensorflow/models/blob/master/research/object_detection/g3doc/installation.md

Some additional notes/ troubleshooting:
- I skipped downloading the COCO API as I was not interested in using the COCO evaluation metrics. All still good.

- When I was at the Protobuf Compilation step, it said: `'protoc' is not recognized as an internal or external command`
- To solve that, follow the steps here: http://tech.yipp.ca/linux/install-google-protocol-buffers-linux/

- When testing the installation, it said `No module named 'object_detection'`.
- To solve that, run the below:

```bash

$ python setup.py build
$ python setup.py install

```

Check out this thread if you have more issues: https://github.com/tensorflow/models/issues/2031

- When testing the installation, it said `ImportError: No module named 'tensorflow'`.
- To solve that, I made a copy of the entire research folder, and pasted it in another location, entirely separate/away from the tensorflow directory. 
(I was operating within the tensorflow directory before this.)

- Separately, my tensorflow version was NOT at 1.4.0. I tried to use `pip install tensorflow==1.4.0` (I use pip3 usually though but some complications about main and stuff) - couldn't resolve it (said successfully installed but it still wasn't 1.4.0. Thought I had too many versions of python/tensorflow in my computer but don't think any of them converted to 1.4.0). Long story short, I checked `tf.__version__`, found that I was on 1.4.1 and decided to ignore the warning.

- After this I kind of skipped running the test - instead I went to access the jupyter notebook directly. All good (thankfully!)


__Run the notebook!__

Being the impatient me, I ran all the cells, skipped to the end and checked that the output was there. Hurrah! It was. Haha

Then I decided to change up the images (second last cell - under the Detection heading. Change the TEST_IMAGE_PATHS to a list of your full image paths).

To save the photos instead of displaying it, instead of `plt.imsave(image_np)`, use `plt.imsave('output.jpg', image_np)`

Who knew, poodles (if they are sufficiently cute) can be classified as teddy bears!!!!!

<img width="500" src="https://user-images.githubusercontent.com/21985915/41198261-feced982-6ca7-11e8-8705-e591b2a0846e.jpg">


MORE TEDDY BEARS!!! LOL

<img width="500" src="https://user-images.githubusercontent.com/21985915/41198264-ff7cd906-6ca7-11e8-852e-bf5546cba958.jpg">


This is my dog! I need to level him up to teddy bear cuteness level LOL
<img width="500" src="https://user-images.githubusercontent.com/21985915/41198260-fe40d42a-6ca7-11e8-99be-ab68728463df.jpg">


Still my dog - still a dog LOL

<img width="500" src="https://user-images.githubusercontent.com/21985915/41198262-ff0e5d0a-6ca7-11e8-8ca1-328c9672da74.jpg">


Hypothesis: does it know it's a dog because of its position (lying down)? Teddy bears don't lie down.
Let's use a dog and a soft toy, both lying down on the floor, to try.

Result: soft toy detected as a CAT, dog detected as a BEAR!!!!!!
Love this HAHAHAHAHA

<img width="500" src="https://user-images.githubusercontent.com/21985915/41198263-ff3d90a2-6ca7-11e8-9de9-0562fc0e211d.jpg">



Next time, I will move towards real time object recognition! AKA videos!! 
https://towardsdatascience.com/building-a-real-time-object-recognition-app-with-tensorflow-and-opencv-b7a2b4ebdc32

Or maybe first, building an app using AWS lambda:
https://medium.com/tooso/serving-tensorflow-predictions-with-python-and-aws-lambda-facb4ab87ddd

Exciting times ahead!!!!