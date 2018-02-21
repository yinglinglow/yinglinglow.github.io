---
layout: post
title: "A Beginner's Guide To GAN (Generative Adversarial Network)"
date: 2018-02-13
---

Imagine a world where we can generate endless pokemon, (real-looking) fake human faces, fake cats, dogs, rabbits... the list goes on forever!! Well, that future is NOW. Pretty sure Ian Goodfellow, the good gentleman who introduced us to the world of GANs, wasn't thinking about pokemon, dogs and cats when he came up with the idea of GANs (actually he was having an argument in a bar) but that's all right... right? <br>

I know we all want to jump right in to running our own GANs (as did I), but let's go a little into the background of GANs first so that we can fully appreciate its tremendous power...

__Part 1 - GANs in general__<br>
<a href='#introduction'>Introduction</a><br>
<a href='#general-model-structure'>General Model Structure</a><br>
<a href='#types-of-gan'>Types of GAN</a>

<a href='https://www.yinglinglow.com/blog/2018/02/20/GAN-walkthrough-2'>__Part 2 - GAN Walkthrough__</a><br>
<a href='https://www.yinglinglow.com/blog/2018/02/20/GAN-walkthrough-2#obtaining-dataset'>Obtaining Dataset</a><br>
<a href='https://www.yinglinglow.com/blog/2018/02/20/GAN-walkthrough-2#cleaning-dataset'>Cleaning Dataset</a><br>
<a href='https://www.yinglinglow.com/blog/2018/02/20/GAN-walkthrough-2#modelling'>Modelling</a><br>
<a href='https://www.yinglinglow.com/blog/2018/02/20/GAN-walkthrough-2#set-up-cloud-platform'>Set up Cloud Platform</a><br>
<a href='https://www.yinglinglow.com/blog/2018/02/20/GAN-walkthrough-2#running-the-model'>Running the Model</a><br>
<a href='https://www.yinglinglow.com/blog/2018/02/20/GAN-walkthrough-2#results'>Results</a><br>
<a href='https://www.yinglinglow.com/blog/2018/02/20/GAN-walkthrough-2#future-improvements'>Future Improvements</a><br>


## Introduction

Before GANs, how did we generate new images? Apart from picking up a pencil and drawing them ourselves, we attempted to teach machines to generate new images by showing them many many MANY images, looking at their results and telling them what is right, and what is wrong. Machines learn differently from humans - say a child could learn to identify an apple after he is shown an apple picture 5 times, but a machine needs upwards of 10,000 images (or more!) to learn to identify an apple! Imagine being a teacher to a machine... pretty painful. Also unfortunately (or fortunately?), there is only so much labelled data and human effort we can dedicate to teaching the machines. 

In any case, that was what motivated Ian Goodfellow to develop GANs - he thought (disclaimer, not his exact words lol) why not create another machine to teach the machine? Hence, the GAN was born (more details [here](https://www.wired.com/2017/04/googles-dueling-neural-networks-spar-get-smarter-no-humans-required/)! Also listen to the podcast [here](https://blogs.nvidia.com/blog/2017/06/08/ai-podcast-an-argument-in-a-bar-led-to-the-generative-adversarial-networks-revolutionizing-deep-learning/) - interesting stuff!).

Let's try to understand this on a conceptual level. A GAN is made up of two models: a __Generator__ to generate fake images (with the aim of fooling the Discriminator), and a __Discriminator__ to differentiate on the real and fake images (but it should not reveal too much - otherwise the Generator learns to make better fakes, making life more difficult for himself!). Each party gets better at what they do as they compete with each other (the Generator at producing real-looking fake images, and the Discriminator at classifying). 

This adversarial type of modelling is not so foreign to us - recall the Nash equilibrium concept in Economics. In the prisoner's dilemma (more details [here](https://www.economist.com/blogs/economist-explains/2016/09/economist-explains-economics)), collectively both prisoners should not confess to the crime, and they will get off with a minimum sentence. Yet by the Nash equilibrium, by considering what my other prisoner will do, it predicts that both prisoners will confess, even though they are both worse off. Because if you confess, I should confess too (a few years sentence versus if I don't confess and get a lifetime sentence). If you don't confess and I do, I get off free! No matter what you do, I should confess. This is true for both parties.

Similarly for GANs:
<pre>
<div class="table-responsive">
  <table style="border:1;" class="table">
    <tr>
      <td></td>
      <td align="center"><b>Confess (Make real looking images)</b></td>
      <td align="center"><b>Lie (Make fake looking images)</b></td>
    </tr>
    <tr>
      <td align="center"><b>Confess (Give correct labelling)</b></td> 
      <td align="center">(-10, -10)</td> 
      <td align="center">(-10, 20)</td>
    </tr>
    <tr>
      <td align="center"><b>Lie (Give wrong labelling)</b></td>
      <td align="center">(20, -10)</td>
      <td align="center">(0, 0)</td>
    </tr>
  </table>
</div>
</pre>

If a model does the right thing (make real images or gives correct labelling), the competing model learns about it and improves its competing techniques, making life more difficult for itself subsequently (hence, -10). If a model lies but the competing model does the right thing, it 'wins' because it gathers more information about the competing model, without revealing any information about itself (hence, 20). If both lie, neither learns anything about each other (hence, 0). By the concept of the Nash equilibrium, it predicts that both models should try their best to make real images/give the correct labelling always, and at the equilibrium, the generator network is able to produce real looking images perfectly, and the discriminator network cannot distinguish the real images from the fake images.

We will see later that there are limitations to this (see [here](https://lilianweng.github.io/lil-log/2017/08/20/from-GAN-to-WGAN.html#hard-to-achieve-nash-equilibrium) for more!), but let's take this for now. In fact, since the introduction of GANs in 2014, the tech community has made significant progress in terms of improving the original model. You will be amazed at how fast people innovate - we can even generate images from text now!! Imagine reading a Harry Potter book and images generating as you go along...

<pre>
<b>Generating new Pokemons:</b><br>
<img width="500" alt="pokemon-gan" src="https://user-images.githubusercontent.com/21985915/36364048-ba6e05d8-157c-11e8-91ee-87a25c60eb14.png"><br>
<small><i>Credits: https://lilianweng.github.io/lil-log/2017/08/20/from-GAN-to-WGAN.html</i></small>

<b>Generating fake bedrooms:</b><br>
<img width="500" alt="bedroom" src="https://user-images.githubusercontent.com/21985915/36432837-21398386-1696-11e8-9dc7-f9bbb53e6ae6.png"><br>
<small><i>Credits:https://arxiv.org/abs/1511.06434v2</i></small>

<b>Generating flowers from text:</b><br>
<img width="500" alt="text-flower" src="https://user-images.githubusercontent.com/21985915/36364285-201dad42-157e-11e8-8bc3-0cb98ff84594.png"><br>
<small><i>Credits: Generative Adversarial Text to Image Synthesis (Scott Reed et al 2016), https://arxiv.org/pdf/1605.05396.pdf</i></small>
</pre>

## General Model Structure

Let's look at the general structure of a GAN from a more practical perspective. In a GAN, two models are trained at the same time.One model is the __Generator__: it takes random noise as input and produces fake images. The second model is the __Discriminator__: it takes both real images and fake images (created by the Generator) as input, and has to figure out how to identify which are real images and which are fake.

<pre>
<img height="400" alt="gan-model" src="https://user-images.githubusercontent.com/21985915/36364510-6a5329fe-157f-11e8-81b3-ee3a7d5d8d48.jpg"><br>
<small><i>Credits: Chris Olah, https://twitter.com/ch402/status/793911806494261248/photo/1</i></small>
</pre>

Basically, the Generator tries to learn all the possibilities of a real image, and returns the most likely image of a dog. For example, it can learn that images of dogs with black, brown or white fur can all be real; an image of a dog with two eyes is real, but an image of a dog with three eyes is not! Hence it can return a two eyed black dog or a two eyed brown dog, but not likely a three eyed black dog. In such a way, it tries to produce fake images that are as real as possible, so as to trick the Discriminator. In technical terms, the Generator is trying to capture the distribution of the training data and produce the most likely outcome, to maximise the probability of the Discriminator making a mistake.

But how does the Generator actually 'learn'? Honestly, at the beginning things are quite random. The Generator generates random images (it can even be just ranomly coloured pixels!) and gives them to the Discriminator. Perhaps the Discriminator gets tricked, and says that a few of the images are real! Aha, now the generator reviews the 'real' images and attempts to make more fake images similar to those. Slowly, it gets better at tricking the Discriminator.

You might wonder, how will the Discriminator be so stupid as to think random images are real? In fact the Discriminator at the beginning does not know what is real or fake as well! It is randomly picking, before it receives feedback as to which images are actually real images or fake images produced by the Generator. From this feedback, it begins to learn all the different possibilities of a real image (capturing the distribution of the true data), and from there learn to differentiate between real or fake images. Technically, it assesses the likelihood of the picture coming from the true data distribution; the lower this likelihood, the lower the chances of it being a real image.

From the above example, we can see that it is important for the Generator and the Discriminator to be at similar levels, so that they can teach each other! Imagine if the Discriminator is so good at identifying fake images that it knows that all of the Generator images are fake and rejects all of them - the Generator will have no feedback to improve! This is one of the more troubling problems when training a GAN.

__Generator architecture__<br>
Zooming into the Generator model (which produces the fake image) - specifically, the Generator takes in random numbers (say, an array of 100 points) as an input, and projects it to a 3D array. In between, transposed convolution (also known as fractionally strided convolutions/ deconvolution) is increasing the size of the image (e.g. from a 2x2 to a 4x4). The animations below show the different manners in which a smaller image (blue squares) can be formed up into a larger image (green). The final output is an image, e.g. a 56x56x3 array which gives a 56x56 RGB image (3 colour channels).

<small>For those interested, Adit did a [fantastic writeup here](https://adeshpande3.github.io/adeshpande3.github.io/A-Beginner's-Guide-To-Understanding-Convolutional-Neural-Networks/) explaining in detail the workings of CNNs.</small>

<pre>

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
  <small><i>Credits: vdumoulin, https://github.com/vdumoulin/conv_arithmetic</i></small>


  <img width="500" src='https://user-images.githubusercontent.com/21985915/36368270-6860664a-1591-11e8-969d-d500396dca84.png'>
  <br>
  <small><i>Credits: https://towardsdatascience.com/gans-part2-dcgans-deep-convolution-gans-for-generating-images-c5d3c7c3510e</i></small>
  
</pre>

__Discriminator architecture__<br>
For the discriminator, it takes in an image (either real or fake), passes the image through convolution layers and reduces it in size (e.g. 4x4 to 2x2). The convolution gif belows shows how the original number of values (in blue) is reduced (green). Eventually it returns a binary output, classifying the image as real or fake.


<b>Convolution<b><br>
<img height="70" src='https://user-images.githubusercontent.com/21985915/36372514-3e585254-15a0-11e8-8976-901a19b7c3f7.gif'><br>
<small><i>Credits: vdumoulin, https://github.com/vdumoulin/conv_arithmetic</i></small>


<img width="500" src='https://user-images.githubusercontent.com/21985915/36368309-9c303e3c-1591-11e8-84ee-ccdaff524ab2.png'><br>
<small><i>Credits: https://hackernoon.com/how-do-gans-intuitively-work-2dda07f247a1</i></small>


## Types of GAN

Now that we understand in general how a GAN works, let's look at the different types of GAN. Yes, just like pokemon (and all living things), they evolve. People got very excited after Ian introduced GANs, and given how difficult it was to train stable GANs (imagine all the different things you can tweak!), everyone started trying new things to improve GANs.

There are many different varieties of GAN - you can refer [here](https://github.com/GKalliatakis/Delving-deep-into-GANs) for a (highly) extended list, but I shall briefly touch on a few of the more popular ones:

  __Vanilla GAN:__ <br>
  This is the original GAN by Ian. Basically, it minimises the f-divergence (read: difference in two distributions) between the real data distribution and the generated data distribution. 
    
  __DCGAN (Deep Convolutional GAN):__<br> 
  This was the first major improvement on GAN architecture - basically the authors proposed a set of constraints to make GANs stable to train. Apparently, they are usually used as the baseline to compare with other newer GANs (aka if your fancier GAN doesn't perform better than a DCGAN, you're out of the game buddy.)
    
  __cGAN (Conditional GAN):__<br> 
  This GAN is interesting, it gets by with a little bit of help - it takes in conditional information that describes some aspect of the data (aka labeled points for eyes, nose for a face). So we are giving our GANs a little help here, if you will.
    
  __WGAN (Wasserstein GAN):__<br> 
  Remember the original GAN just looks at the difference between the real data distribution and the generated data distribution? There's a caveat - if the true and fake distributions do not overlap, the feedback given is just 0 or infinity. How can the Discriminator or Generator learn effectively in that case? The feedback is not useful as to how they should change. That's part of the reason why GANs are so difficult to train. Enter the Wasserstein-1 distance (Earth-Mover distance) - basically in this case, even if the two distributions have no overlap, at least it describes how far apart they are so that they can learn (instead of just returning 0 or infinity!)


You would be pleased to know that we will be running DCGAN and WGAN in our walkthrough later on. In the meantime, let me talk a little more about WGAN since we will be running a slight variation of that.

We will be using WGAN-GP (GP for Gradient Penalty) instead of WGAN - because even WGANs can fail to converge! In human terms: for the Wasserstein distance to work, it has to be limited by how fast it can change. To enforce this, the authors of WGAN forced (aka 'clipped') the values to stay within a certain threshold. For example, if the threshold is (-1,1), anything greater than 1 will be 1, and those less than -1 will be -1. However, forcing numbers to stay within such a range can significantly impact the final results (in practical terms, your picture of a bedroom can suddenly have empty white spaces in the middle of the image!). 

In mathematical terms, for the approximation of the Wasserstein (Earth-Mover) distance to be valid, weight clipping constraints had to be imposed on the Discriminator. This resulted in:
- The optimizer with gradient clipping to search the discriminator in a space smaller than 1-Lipschitz, biasing the discriminator toward simpler functions.
- Clipped gradients vanishing or exploding as they back-propagate through network layers.

Hence, instead of clipping weights as in WGAN, WGAN-GP penalises the norm of gradient of the discriminator with respect to its input. In human terms, this means that the bigger your gradient norm (maximum rate of change), the more you will be penalised. It incentivises the model towards the threshold (remember (-1,1)?) so that WGAN remains valid.


WHEW! You made it through all that - well done!! Let's get down to business now (finally!) and head over to <br><a href='https://www.yinglinglow.com/blog/2018/02/20/GAN-walkthrough-2'>__Part 2 - GAN Walkthrough__</a>, where we go through the actual steps of running your very own GAN!