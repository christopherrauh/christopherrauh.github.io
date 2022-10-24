---
title: Developing a Dog Breed Classifier
---

![Beautiful Seattle](../images/dogs.jpg)

---
Using advanced Deep Neural Networks to identify Dog Breeds

---

## Introduction

This article outlines the development of a dog breed classifier using convolutional neural networks in Python and how to get to substantially improve the result. A special focus will be on the usage of pretrained neural networks.

We will be working with input images that contain either _human faces_ or _dogs_. 

For dogs we want to find the correct breed, for humans we want to find a dog breed that somehow resembles this human.

The classification will first have to distinguish between humans and dogs - _using different approaches_ - and then finds the corresponding dog breed - _using the same approach for both humans and dogs_.

### Technical Background

For our endavours we will be using Python and a number of specialized frameworks and libraries.

The most important components are OpenCV and Keras.

### OpenCV

[OpenCV](https://opencv.org/) - the Open Source Computer Vision Library - is a library to simplify real-time computer vision tasks. A detailled description is provided at the corresponding [Wikipedia Page](https://en.wikipedia.org/wiki/OpenCV).

We are using OpenCV's [Haar feature based cascade classifier](https://docs.opencv.org/3.4/db/d28/tutorial_cascade_classifier.html) to detect if an image contains a human face or not. Since our input images contain either dogs or human faces this is a first step to reach our goal.

### Keras 

![Prices per day](../images/price_per_day.png)


### 

## Conclusions and what to do next