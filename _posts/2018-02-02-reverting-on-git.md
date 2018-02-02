1) create a new branch based on the current one (to store everything, just in case).
git checkout -b new-branch existing-branch

if you want to work on this new branch, stay here.
if you want to work on the old one, `git checkout old-branch-name`

2) `git push origin my_new_branch` to reflect the changes online (i double check on github.com just to be sure.)

2) `git log` to count the number of reverts (or get the id). scroll with arrow bars, press q to quit.

3) `git reset --hard HEAD~x` where x is the number of commits to roll back to. THIS DELETES ALL THE COMMITS AFTER THAT so please make sure you have already kept another branch with all your other commits if necessary (step 1).


apparently there are faster ways to do this but somehow i managed to screw them up lol


much thanks to:

http://dont-be-afraid-to-commit.readthedocs.io/en/latest/git/commandlinegit.html

https://stackoverflow.com/questions/2816715/branch-from-a-previous-commit-using-git/2816728

https://stackoverflow.com/questions/31301051/github-windows-new-branch-created-from-command-line-not-showing-up-on-github-co
