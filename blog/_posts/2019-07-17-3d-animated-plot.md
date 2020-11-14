---
layout: post
title: "3D Animated Plot using Matplotlib"
date: 2019-07-17
---

First, download the gyroscope sensors dataset from here:

https://github.com/mmalekzadeh/motion-sense

Install all the necessary libraries and stuff (mac users remember to `brew install ffmpeg` if not writing to mp4 does not work!!)

the below generates a mp4 file....!!!

```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
from mpl_toolkits.mplot3d import Axes3D # for 3D graph
from matplotlib.animation import FuncAnimation # for animation

# extract data
filename = "dws_1/sub_1.csv"
df = pd.read_csv(filename, index_col=0)

# -----
# actual main function
# -----

def plot_3dplane(i):
    
    """
    Given a dataframe with 3 columns x, y, z and an integer i
    Returns a plot of a 3D plane in the row i of the dataframe.
    """
    
    # clear previous plane
    ax.clear()
    ax.set_zlim(-5, 5)
    
    # plot new 3D plane
    x = np.linspace(-1,1,10)
    y = np.linspace(-1,1,10)

    X,Y = np.meshgrid(x,y)
    Z = df['x'][i+1]*X + df['y'][i+1]*Y + df['z'][i+1]

    surf = ax.plot_surface(X, Y, Z)
    

# set up canvas
fig = plt.figure()
ax = fig.gca(projection='3d', xlim=(-1,1), ylim=(-1,1), zlim=(-5, 5))

# plot first figure
i = 0
x = np.linspace(-1,1,10)
y = np.linspace(-1,1,10)

X,Y = np.meshgrid(x,y)
Z = df['x'][i]*X + df['y'][i]*Y + df['z'][i]
surf = ax.plot_surface(X, Y, Z)

anim = FuncAnimation(fig, plot_3dplane, frames=len(data)-1, blit=False)

anim.save('basic_animation.mp4', fps=30, extra_args=['-vcodec', 'libx264'])
```