---
layout: post
title: "reverting on git"
date: 2018-02-02
---

1) create a new branch based on the current one (to store everything, just in case).
git checkout -b new-branch existing-branch

if you want to work on this new branch, stay here.
if you want to work on the old one, `git checkout old-branch-name`

2) `git push origin my_new_branch` to reflect the changes online (i double check on github.com just to be sure.)

3) `git log` to count the number of reverts (or get the id). scroll with arrow bars, press q to quit.

4) `git reset --hard HEAD~x` where x is the number of commits to roll back to. THIS DELETES ALL THE COMMITS AFTER THAT so please make sure you have already kept another branch with all your other commits if necessary (step 1). __update:__ it is good practice to reference via the commit id rather than the number of commits, eg `git reset --hard c14809fa` just to be sure. i learnt the hard way when i somehow managed to roll back, squash all my commits into ONE commit, then i rolled by again by 10 commits, hence bringing me NINE commits earlier than i wanted to. moral of the story, just use the id instead of hard coding in the counts.

5) __IF__ you get an error saying you are behind the head by x number of commits and they ask you to git pull, AND you are sure that it is the local copy of your git branch you want to keep, use this: `git pull origin branch-name -s ours` where s is for strategy, and the strategy is to use 'ours' aka the local copy. otherwise it will bring you up to date with the master again (which i didn't want).


also apparently there are faster ways to do this but somehow i managed to screw them up lol


much thanks to:

http://dont-be-afraid-to-commit.readthedocs.io/en/latest/git/commandlinegit.html

https://stackoverflow.com/questions/2816715/branch-from-a-previous-commit-using-git/2816728

https://stackoverflow.com/questions/31301051/github-windows-new-branch-created-from-command-line-not-showing-up-on-github-co

and of course my personal git advisor :p