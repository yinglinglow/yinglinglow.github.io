---
layout: post
title: "Installing npm"
date: 2018-09-01
---

Easy to follow instructions here: https://www.npmjs.com/get-npm

When you get to the step of `npm install npm@latest -g` it might prompt an error "Please try running this command again as root/Administrator."

This is because -g means to install it globally, and requires admin permissions. In mac, to address this issue run `sudo npm install npm@latest -g` instead and input your login password.

