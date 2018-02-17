---
layout: post
title: "Resource Exhuasted Error while running GPU"
date: 2018-02-08
---

if you ever come across this Resource Exhausted error, (if you're using nvidia GPU) check your GPU usage with:
`sudo fuser -v /dev/nvidia*`

then kill the process you don't need with:
`sudo kill -9 PID` where you replace `PID` with the PID of the process to kill.
for example, i killed my python3 with `sudo kill -9 20450`

thanks to:
https://stackoverflow.com/questions/15197286/how-can-i-flush-gpu-memory-using-cuda-physical-reset-is-unavailable