---
layout: post
title: "Pipenv + Jupyter Notebooks"
date: 2018-11-25
---

i think in the midst of getting my head around using pipenv and all i've kind of messed up all my installations *cries*

in any case, if you (like me) need to use jupyter notebooks still, use the below to work with pipenv!

```bash

pipenv install ipykernel
pipenv shell

python -m ipykernel install --user --name=my-virtualenv-name

jupyter lab

# in the notebook, select Change Kernal and pick my-virtualenv-name

```

credits to: https://stackoverflow.com/questions/47295871/is-there-a-way-to-use-pipenv-with-jupyter-notebook

now it's going to be a headache to unwind the painful situation i am in...