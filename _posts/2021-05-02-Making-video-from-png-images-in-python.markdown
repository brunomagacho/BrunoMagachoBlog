---
layout: post
title:  "Making video from png images in python"
date:   2021-05-02 10:33:54 -0300
categories: jekyll update
author: "Bruno Magacho"
---

In this example a video is  made from the images in the directory path. The images should be ordered, since the function glob will import them in a list ordered according to the ordering name of the pictures in the folder. The size of the video will be the same as the one from the images and in this example the fps is set to 20. It works for other extensions also.


```python
import cv2
import numpy as np
import glob


fps = 20

path = "C:/Users/Bruni/Documents/PythonScripts/BGK/"

extension = "*.png"

img_array = []

print("Importing images")

for filename in glob.glob(path+extension):
    img = cv2.imread(filename)
    height, width, layers = img.shape
    size = (width,height)
    img_array.append(img)
 
 
print("Making video")

out = cv2.VideoWriter('project.avi',cv2.VideoWriter_fourcc(*'DIVX'), fps, size)
 
for i in range(len(img_array)):
    if i == int(len(img_array)/4):
        print("25%")
    if i == int(len(img_array)/2):
        print("50%")
    if i == int(3*len(img_array)/4):
        print("75%")
    if i == len(img_array)-1:
        print("Video created")
    out.write(img_array[i])
    
out.release()
```

The output should be like 

```
Importing images
Making video
25%
50%
75%
Video created
```
