---
title: "[Android] Motion tracking application with Android Camera2 API and Tensorflow Lite"
description: My first time making android application
date: 2022-04-16T19:09:09-04:00
draft: false
toc: false
image: ""
tags: ["Android", "Development", "Tensorflow"]
categories: ["Android"]
---

# Overview
Recently, I finished working on a prototype application that does motion tracking.
Here is the repository: https://github.com/NameOnTree/androidCameraMotionTracker

Here is a simple view of the flow of the application:

1. Select & Open Camera device
2. Make a repeating camera capture request and get image data from the camera device
3. Read the image data (in YUV_888 format) and convert into RGBA format
4. Preprocess images into shapes that a tensorflow lite model can consume.
5. run the tensorflow lite inference and get torso keypoint xy coordinates
6. Draw the torso keypoint coordinates on a imageView.

The biggest challenge over the development was that the program was not optimized to run smoothly on every machines.

Specifically, there is a huge performance bottleneck in the preprocessing images.
Converting YUV_888 format to RGBA could delay the whole program upto 100ms ~ 120ms!

I believe there might be a clever trick to tackle this problem, like multithreading or hardware acceleration, but that's for a future project.