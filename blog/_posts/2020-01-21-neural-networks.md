---
layout: post
title: "Demystifying Neural Networks"
date: 2020-01-21
---

First, a few definitions:

### __AI vs machine learning__

> The theory and development of computer systems able to perform tasks normally requiring human intelligence, such as visual perception, speech recognition, decision-making, and translation between languages.

Thank you google. What I wanted to say is, AI is really just a very broad definition of tasks which we think need human intelligence to do (which is highly subjective!!)

### __Machine learning vs programming__

The difference is, for programming you feed input and your rules (code), to get your output.

For Machine Learning, you feed input and your desired output, and you get your code! Using that code you predict other output (given some input).

![](https://user-images.githubusercontent.com/21985915/73129099-088b6500-4017-11ea-97bb-ec2df380f4e7.png)

Credits to: https://medium.com/@UdacityINDIA/difference-between-machine-learning-deep-learning-and-artificial-intelligence-e9073d43a4c3

### __Deep learning__

Deep learning is a subset of neural networks, which are a subset of machine learning

![](https://user-images.githubusercontent.com/21985915/73129125-8fd8d880-4017-11ea-9d74-7aec6133658c.png)

Credits to: https://www.stoodnt.com/blog/ann-neural-networks-deep-learning-machine-learning-artificial-intelligence-differences/

---

### __Building a Neural Network with Tensorflow__

There are four components to building a neural network with tensorflow:
- __Architecture__: which is how a network is structured
- __Loss function__: which tells you how far your prediction is from your labelled data
- __Optimisation function__: which is how your machine 'learns' i.e. get closer to the correct answer
- __Inputs/Outputs__: which is the data; all of it needs to be numerical, hence we vectorise things!

---

This is a good website to showcase basic neural networks and how they work: https://playground.tensorflow.org/

We start with x1 and x2, zero layers, and the dataset which looks like a doughnut with a blue center. Ideally you'd wanted a circular boundary to separate the orange and the blue points.

The resulting white separating boundary is simply just a slanted line, contributed by x1 and x2 (x1 contributes a lot more, hence the boundary is almost vertical).

As you can see, the data cannot be segregated with a single straight line.

![](https://user-images.githubusercontent.com/21985915/73129179-ac294500-4018-11ea-982b-f04e14e7dac3.png)

Add one hidden layer with two nodes. The hidden layer nodes are also simply just combining patterns from previous nodes.

The final boundary is made up of two lines, and as you can see - two lines doesn't segregate the data properly either.

![](https://user-images.githubusercontent.com/21985915/73129203-2d80d780-4019-11ea-85b4-c8bdec32b847.png)

We now add one hidden layer, with 3 nodes. Run ~100 epochs, and you should now see almost like a triangle forming in the centre, forming a boundary around the blue center.

This is because the three nodes, each a combination of x1 and x2, have formed three slopes, which then combined to form a triangle.

![](https://user-images.githubusercontent.com/21985915/73129206-31acf500-4019-11ea-8a34-da25d6f3f242.png)


Repeat the same (1 hidden layer) with 4 nodes, and you (might) see a square appear! eventually they do converge to an almost triangle/circle kind of shape, this is really random.

![](https://user-images.githubusercontent.com/21985915/73129208-3376b880-4019-11ea-8dcf-04799aa36d31.png)

---

I think the above has been the clearest illustration of neural networks I have seen so far.

Some random thoughts/comments:

- the hidden layer nodes are combining patterns from previous nodes. so does this mean that if my data has very intricate patterns, i should use more nodes...?

- also it seems people usually half the nodes every layer. e.g. if you begin with a (200,) vector, the first hidden layer can be ~100 nodes, then ~50, then ~20, then finally ending off with a 10 node output layer (if you're working with the mnist digits data set)?

- makes sense to first actually make the neural network actually work (even though it's really big), then trim it down.

- it seems you usually just begin with a simple x function. only if you have an expert that can advise on the usage of x^2 (for example), then use it?

- batch sizes affect the number of times the weights are tweaked! e.g. if you have 60,000 images of digits, and your batch size is 600, with 10 epochs, your weights are tweaked (60000/600) * 10 = 1000 times!

- apparently in some situations where your data set is so huge, you can afford to just run 1 epoch and the model is done. this has the advantage in that the model only sees each image once (1 epoch, see) - no chance of it memorising the image and overfitting!

---

so much more i have to learn oh my

---

A bonus article on weights and biases for those interested: https://medium.com/fintechexplained/neural-networks-bias-and-weights-10b53e6285da

---

major credits to http://reddragonai.com - i attended an introductory course by them during a meetup so all of this knowledge is from these guys!