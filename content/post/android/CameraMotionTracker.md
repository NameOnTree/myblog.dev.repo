---
title: "[Android] Motion tracking application with Android Camera2 API and Tensorflow Lite"
description: How did I make it? not quite simple... After all, this is my first time making android application!
date: 2022-04-16T19:09:09-04:00
draft: true
toc: false
image: ""
tags: []
categories: []
---



# Contents
- Overview
- Camera2 API setup
- 

# Overview
Hi, this is my first article here! I started working on this project when I wanted to make something out of neural network.
However, this application is somewhat a mess. However, it still works! I feel like I can do better organizing codes
My program may be break into several steps,

1. Select & Open Camera device
2. Make a repeating camera capture request and get image data from the camera device
3. Read the image data (in YUV_888 format) and convert into RGBA format
4. Preprocess images into shapes that a tensorflow lite model can consume.
5. run the tensorflow lite inference and get torso keypoint xy coordinates
6. Draw the torso keypoint coordinates on a imageView.

Some of these processes may require extra hardware performance to run the program smoothly. 

Converting YUV_888 format to RGBA in JNI function was the main performance bottleneck.
This operation could delay the whole program upto 100ms ~ 120ms!

Note that this is only for single operation step, so the frame rate can go even lower after summing up all other operations.

I really wished I could get RGB images directly from camera2 API, but unfortunately RGBA format is not supported in Android.
This is because YUV_

There was a fast and neat solution in my case: just decrease the camera capture resolution.

After the camera capture resolution has been reduced, the frame rate in overall was about 10 fps.
But as you see, this is still not ideal for a real time inference application. 

Here is some future improvements to make the application more smoothly:

- Use multi-thread to convert YUV_888 format to RGBA format
- 





![BigSteps](/CameraMotionTracker_FlowChart.png "Overall FlowChart")

To use Camera in Android, 

![firstStep](/makeProject.png "1. Make new Project")

First, You might want to create a basic empty activity

To do that, you can go to activity_main.xml to create the textureView for the camera preview. 
Make sure the constraints are all matched and has a gap of 0

![secondStep](/CameraMotionTracker_ActivityMain.png "2. Create an TextureView object")

Now we can grab the reference to the textureView fron the activity.


