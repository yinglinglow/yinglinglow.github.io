---
layout: post
title: "How to Set Up your Mac for coding Python"
date: 2018-08-12
---

Since I lost everything on my computer, I have to reinstall everything!

I'll take this chance to write a guide on how to set up your Mac for coding Python!
(Post-edit: Ok this turned out more like common FAQs which I myself was asking just a few months ago...!
This is probably more relevant to programming beginners.)

__How do I install Python?__
1) Using Anaconda for Python here: https://anaconda.org/anaconda/python
2) Installing Python by itself: https://docs.python-guide.org/starting/install3/osx/#install3-osx

__What's the difference? What is Anaconda for Python?__
Anaconda is basically Python packaged together with other stuff you generally use together with Python like Jupyter Notebook, etc.

Jupyter notebook is a web application that allows you to code interactively - you can type your code, run it line by line or chunk by chunk, 
and see your outputs! It is very useful for beginners for learning, and also experienced coders for sharing code, debugging (aka finding and removing errors 
from your code), etc.

Anaconda also helps to install common packages used when using Python. (Packages are code that others have written which you can install to use.) 

So basically - instead of installing a million things, Anaconda simplifies it so that you just have to do one installation
to get Python and all other things you need when using Python.

__So... why shouldn't I just install Python using Anaconda?__
You should if you are a programming beginner!

It's like, a beginner baking cupcakes for the first time. If I can just go to a shop and buy an entire baking set that consists of
the cupcake mix, cupcake paper cups, mould for the cupcakes, icing, etc - why not? 

But say in the future I only want to bake the cupcakes - I don't want icing and don't need to put them in paper cups.
I shouldn't buy the entire baking set again! It is much cheaper for me to just buy the cupcake mix on its own.

So in the future when you run your Python programs on the cloud (as they call it) or basically any virtual machine or container, 
it's unlikely that you're going to install Anaconda if you only need Python (it takes too long to install all the other things
which you don't need!). 

If you want to install Python on your MacOS: https://docs.python-guide.org/starting/install3/osx/#install3-osx
If you need it on Linux: https://docs.python-guide.org/starting/install3/linux/#install3-linux

*In the step which says to add the line to your ~/.profile file: if you don't have a .profile file, just create it (I used VisualStudioCode to create it and just pasted the line in and saved it).

__If you installed Python its own... install pipenv!__
While pip can install packages, pipenv is recommended to simplify dependency management. See here: https://docs.pipenv.org/install/

__I am using Jupyter Notebook to code. What's Spyder and Visual Studio Code and all these things called IDEs?__
You can run your code in Jupyter Notebook, but that's not a script, it's a Jupyter Notebook file type - you have to save it as a .py file 
in order to be able to run it using 'python yourscript.py' in the terminal. And you want to do that because if I have many many scripts to run,
I am not going to open many many Jupyter Notebooks and run them one by one manually, I want to call all my scripts together and run them at one go!

IDE: Integrated Development Environment is a code editor that has other facilities such as intelligent code completion, 
debuggers, version control (git), object browser (see the products of your code!), etc to allow all code development to be done in one single program.

So in the ideal case scenario I will code and debug and git push and run my code and do everything in my IDE.
In the imperfect world as I am still a noob, I do most things in my IDE but still rely on Jupyter to test my code snippets.

I use Visual Studio Code for my IDE - I haven't actually tried other IDEs so not too sure which one is the best!
Spyder has a fairly similar layout to RStudio in case anyone is interested. To install VSC, go here: https://code.visualstudio.com/docs/?dv=osx

You can also install the Python extension in VSC for linting (analyzes the code for potential error) and intelligent code completion.
Install Git here: https://git-scm.com/downloads
Git is for version control - basically it's like you save a copy of your current work as Version 1.0. You make some changes, commit those changes and save it as
Version 1.1. You make more changes and save them as 1.2, 1.3, 1.4. Oops! You realise something is wrong in 1.4 - you can undo your changes back to 1.3 and continue working from there!
In VSC, install the Gitlens extension for easier navigation of your repository.


---

Ok that's all for today!!
For now. lol

For anyone who is interested, some questions popped up into my head as I was writing today's post - feel free to read up in the links below:
- Difference between MacOS and Linux: https://itsfoss.com/mac-linux-difference/
- Packages and modules in Python: https://realpython.com/python-modules-packages/
- Version control: https://www2.le.ac.uk/services/research-data/organise-data/version-control
- Containers vs Virtual Machines (VM): https://blog.netapp.com/blogs/containers-vs-vms/
