---
layout: post
title: "git - pull requests"
date: 2019-02-12
---

extremely late to the game BUT better late than never right? lol

---

so it's good practice to do pull requests even if you are the only person working on your own project.

here are a few useful guides to help you avoid git hell:
- https://www.thinkful.com/learn/github-pull-request-tutorial/#Writing-a-Good-Commit-Message
- https://git-scm.com/book/en/v2/Git-Branching-Basic-Branching-and-Merging

but the main gist of how to do a pull request is:

__1) create a new branch and switch to it__

`git checkout -b mynewbranchname`

__2) make your changes on the branch__

do your commits and pull and push etc

__3) create pull request__

on the github page, click on 'compare & pull request', and then 'create pull request'

__4) approve and merge__

__5) switch back to master and git pull__

`git checkout master`

`git pull`

