---
layout: post
title: "Updating gitignore to clean up your git repo"
date: 2018-01-18
---

again, not GAN related but helpful in general.

amazing stuff to clean up your online git repo and preserve the local files.

```bash
git rm -r --cached .

git add .

git commit -m "fixing .gitignore"

```