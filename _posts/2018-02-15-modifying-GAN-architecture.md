
so difficult!

i added batch normalisation to the discriminator, but the losses converged to zero very quickly (less than 100 epochs out of 10000!)

i'll run this, see the results, then run another...

---

removed sigmoid function - discriminator loss huge, adversarial loss 0.

---

150218 11pm: decreased window size from 5 to 4 - params decreased from 4mil to 2mil.

losses looking better (0.6 compared to 1.2) - but then it is increasing!! now decreasing again to 0.8... and now 1.2. this is driving me nuts! 1.6 - so dead....

now, 4! crazy. but at least it works! perhaps i should make the window bigger...

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
conv window = 5
updated losses plot

---

run DCGAN for losses and conv window = 6