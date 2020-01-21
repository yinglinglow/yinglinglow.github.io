---
layout: post
title: demystifying neural networks"
date: 2020-01-21
---

this blog post is pretty good for a few things: https://medium.com/@UdacityINDIA/difference-between-machine-learning-deep-learning-and-artificial-intelligence-e9073d43a4c3

it defines:
- AI vs machine learning
- machine learning vs programming
- deep learning as a subset of neural networks as a subset of machine learning

---

there are four components to building a neural network with tensorflow:
- __Architecture__: which is how a network is structured
- __Loss function__: which tells you how far your prediction is from your labelled data
- __Optimisation function__: which is how your machine 'learns' i.e. get closer to the correct answer
- __Inputs/Outputs__: which is the data; all of it needs to be numerical, hence we vectorise things!

---

a diversion into weights and biases: https://medium.com/fintechexplained/neural-networks-bias-and-weights-10b53e6285da

---

https://playground.tensorflow.org/

good website to showcase basic neural networks and how they work!

we start with x1 and x2, zero layers, and the dataset which looks like a doughnut with a blue center.

the data cannot be segregated with a single straight line.

we now add one hidden layer, with 3 nodes. run ~100 epochs, and you should now see almost like a triangle forming in the centre, forming a boundary around the blue center.

this is because the three nodes, each a combination of x1 and x2, have formed three slopes, which then combined to form a triangle.

repeat the same (1 hidden layer) with 4, 5 and 6 nodes, and you (might) see a square, pentagon and hexagon appear! eventually they do converge to an almost triangle/circle kind of shape, yes. I think this has been the clearest illustration of neural networks i have seen so far.

---

the hidden layer nodes are combining patterns from previous nodes. so does this mean that if my data has very intricate patterns, i should use more nodes...?

also it seems like they usually half the nodes every layer. e.g. if you begin with a (200,) vector, the first hidden layer can be ~100 nodes, then ~50, then ~20, then a 10 node output layer (if you're working with the mnist digits data set)?

first you make the neural network actually work, then trim it.

---

also it seems you usually just begin with a simple x function. only if you have an expert that can advise on the usage of x^2 (for example), then use it?

---

also, batch sizes affect the number of times the weights are tweaked! e.g. if you have 60,000 images of digits, and you batch size is 600, with 10 epochs, your weights are tweaked (60000/600) * 10 = 1000 times!

apparently in some situations where your data set is so huge, you can afford to just run 1 epoch and the model is done. this has the advantage in that the model only sees each image once (1 epoch, see) - no chance of it memorising the image and overfitting!

---

so much more i have to learn oh my