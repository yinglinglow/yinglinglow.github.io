---
layout: post
title: "Modifying GAN architecture"
date: 2018-02-15
---

Ran WGAN_010218.py - 7pm 3 feb. cut at 500. Works!
Ran pokemon_gan.py - 9pm 3feb. 

---

ran 8 feb WGAN_010218_bw 3pm 3feb 10000 epochs
Ran 9 feb POC_050218_bw 7am X_train_28_813_1984

---

POC_increase_1_aws: 
Dense, upsampling x 4, 2 maxpooling, conv2d, output conv2d
4 discri

POC_increase_2_aws: 
Dense, upsampling x 4, 2 maxpooling, conv2d, output conv2d
6 discri
A loss goes to 0 very quickly

---

so difficult!

i added batch normalisation to the discriminator, but the losses converged to zero very quickly (less than 100 epochs out of 10000!)

i'll run this, see the results, then run another...

---

removed sigmoid function - discriminator loss huge, adversarial loss 0.

---

DCGAN
150218 11pm: decreased window size from 5 to 4 - params decreased from 4mil to 2mil.

losses looking better (0.6 compared to 1.2) - but then it is increasing!! now decreasing again to 0.8... and now 1.2. this is driving me nuts! 1.6 - so dead....

now, 4! crazy. but at least it works! perhaps i should make the window bigger...


discr_model_5999150218 - uploaded to s3
adv_model_5999150218
gen_5999150218
---

150218 1am:

running wgan! with the same parameters as previously but results seem much better?
the inputs do matter... LOL
it takes way longer though... no? 2000 epochs took about 5hours (wat!)

somehow its all circles, and black and white... did i do something wrong???

- check models - no other models
- check xtrain - no other xtrain except X_train_56_1700.pkl

is it the augmentation that is screwing things up...? dont really get why it is black and blue though....?

i will run it until 5500...can?

---

will try WGAN without augmentation
and increase the layers back
conv window = 4
updated losses plot

- doesnt seem to be working! still greyish bg. stopped after 500.

---

run DCGAN for losses and conv window = 6
seems more big picture
less detail

got the losses

---

rerunning WGAN_010218 as 10pm.
just changed augmentation (no shifting) and loss statement prints

something wrong!!! all grey. gulps.

---

trying WGAN_010218.py from cloud
on X_train_56_1700.pkl
fingers crossed! try check on when 500.

so dead - all grey!!!

is something corrupted??

---

oh wait lol maybe i forgot to divide by 255.
lalalala

ok it doesnt work
---

17 feb 2018, 11pm:

back to the original 010218, and original dataset.
please just replicate!
forgot to tmux. good job, girl, good job.
now i have to stop after 500.

MODEL=WGAN_010218.py
XTRAIN=X_train_56_1503

---

