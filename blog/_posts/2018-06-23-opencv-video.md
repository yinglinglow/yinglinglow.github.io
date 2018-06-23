---
layout: post
title: "OpenCV - Recording and Save Video"
date: 2018-06-23
---

So the issue is, everytime I close the window my video is showing on, it hangs and I have to force quit Python. I couldn't figure out for the life of me what the heck was going on and finally, FINALLY after a million years of googling I realised...the code doesn't work in Jupyter Notebook *faints*

In any case - thanks to a guy who raised that up and stopped my endless blind searching for solutions!!!!! I am now running the code in a .py file and it works!!

Follow the code below (credits to this guy: https://www.learnopencv.com/read-write-and-display-a-video-using-opencv-cpp-python/) if you want to record a live video. It opens the camera, records the video, closes the window when you press 'q', and saves the video in .avi format.

By the way, I am on macOS High Sierra 10.13.4, Python 3.6.5, OpenCV 3.4.1.

If you want to read a file instead, replace `cap = cv2.VideoCapture(0)` with `cap = cv2.VideoCapture('yourfilename.avi')`.


```python

import cv2
import numpy as np
 
# Create a VideoCapture object
cap = cv2.VideoCapture(0)
 
# Check if camera opened successfully
if (cap.isOpened() == False): 
  print("Unable to read camera feed")
 
# Default resolutions of the frame are obtained.The default resolutions are system dependent.
# We convert the resolutions from float to integer.
frame_width = int(cap.get(3))
frame_height = int(cap.get(4))
 
# Define the codec and create VideoWriter object.The output is stored in 'outpy.avi' file.
out = cv2.VideoWriter('outpy.avi',cv2.VideoWriter_fourcc('M','J','P','G'), 10, (frame_width,frame_height))
 
while(True):
  ret, frame = cap.read()
 
  if ret == True: 
     
    # Write the frame into the file 'output.avi'
    out.write(frame)
 
    # Display the resulting frame    
    cv2.imshow('frame',frame)
 
    # Press Q on keyboard to stop recording
    if cv2.waitKey(1) & 0xFF == ord('q'):
      break
 
  # Break the loop
  else:
    break 
 
# When everything done, release the video capture and video write objects
cap.release()
out.release()
 
# Closes all the frames
cv2.destroyAllWindows() 

```