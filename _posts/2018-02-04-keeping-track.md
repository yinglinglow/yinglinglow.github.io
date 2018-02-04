---
layout: post
title: "keeping track"
date: 2018-02-04
---

name your files properly!! time date stamp.

otherwise you might end up in git hell (like i did) when u can't figure out which is the correct file to use, make wrong changes and commits, and have to struggle to roll back (and mess things up even more by pushing ur AWS access key online a SECOND TIME. who is so stupid?! gosh).

WELL all's done now, so my piece of advice is - set the name of your training dataset as an argument to input. that way you KNOW you're not changing anything in the model, and hence it's the dataset that is actually giving you problems!

first i forgot to divide the data by 255 (smacks head); second somehow the code doesnt run well on small code sets (like 268). i regenerated and bumped it up to 1280 - code's running well again!

so anyway, below you will see my keeping track so i don't end up in the same situation again (faints):

3rd Feb (7pm): WGAN_010218.py with X_train_56_1503 (500 epoch test run) - success!
3rd Feb (9pm): pokemon_gan.py with X_train_72_1503 (full run) - failed :( almost all the same images! which is...a flower???
4th Feb (7am): WGAN_010218.py with X_train_56_1280_frmcircles (full run) - in progress!

fingers crossed...
