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
[Keras](https://keras.io/) is a deep learning framework written in Python. It can be used to create Neural Networks in a relatively simple fashion ("Deep learning for humans" is one of their sloagans). However, Keras itself is not sufficient, for the actual execution (training and inference) of neural networks, a backend is needed. In our case (i.e. the Udacity lab environment), Tensorflow is used as the backend.

Keras comes with a number of tools to process data and even provides a large number of pretrained models.

We are using Keras in the following areas:

- a ResNet model that has been trained on ImageNet, a wide collection of images.
- the preprocessing cpapbilities to load images
- create the CNN based neural networks
- use a large pretrained model (one of VGG-19, ResNet-50, Inception, Xception)
- perform training, validation and prediction


## Steps in the development

### Human detector

In order to detect human faces, the OpenCV's Haar feature based classifier is used. If the classifier doesn't find any face in the picture, we will assume the picture in question doesn't show a human.

Applying this strategy to our test set of humans and dogs produces the follwoing result:

```
detection rate for human faces:  100.0 %
detection rate for human faces in dog pictures:  11.0 %
```

This means that we have about 11% of false positives. So we should apply a better approach to be sure that a dog is recognized as a dog and not as a human.

### Dog detector

In order to more reliably detect dogs, we are using the ResNet50 network that can be simply loaded from Keras. 

```
from keras.applications.resnet50 import ResNet50

# define ResNet50 model
ResNet50_model = ResNet50(weights='imagenet')
Downloading data from https://github.com/fchollet/deep-learning-models/releases/download/v0.2/resnet50_weights_tf_dim_ordering_tf_kernels.h5
102858752/102853048 [==============================] - 1s 0us/step
```

It is already trained on Imagenet (a huge collection of pictures) and produces results according to a predefined dictionary. The entries from 151 to 268 correspond to dog breeds:



```
151: 'Chihuahua',
152: 'Japanese spaniel',
153: 'Maltese dog, Maltese terrier, Maltese',
154: 'Pekinese, Pekingese, Peke',
155: 'Shih-Tzu',
156: 'Blenheim spaniel',
157: 'papillon',
158: 'toy terrier',
159: 'Rhodesian ridgeback',
160: 'Afghan hound, Afghan',
161: 'basset, basset hound',
...
...
...
262: 'Brabancon griffon',
263: 'Pembroke, Pembroke Welsh corgi',
264: 'Cardigan, Cardigan Welsh corgi',
265: 'toy poodle',
266: 'miniature poodle',
267: 'standard poodle',
268: 'Mexican hairless',
```

This new dog detector yields much better results:

```
detection rate for dog faces in human pictures:  0.0 %
detection rate for dog faces in dog pictures:  100.0 %
```

### Dog Classifier from Scratch

As a next step in the evolution we want to use a CNN from scratch using Keras in order to classify dog images.

A CNN classifier uses the following approach:

1. Hierarchical spacial features are extracted 
2. The resulting output - i.e. which features have been recognized is then classified and the most likely value is calculated by a softmax layer.

The way hierarchical spacial features can be imagined is visualized in this picture taken from "Deep Learning with Python", by Francois Chollet.

![](../images/cat-features.png)

The feature extraction part is basically a stack of Conv2D and MaxPooling2D layers. 

- Conv2D layers: Perform the mathematical operation of convolution is between the input image and a filter of a particular size MxM. By sliding the filter over the input image, the dot product is taken between the filter and the parts of the input image with respect to the size of the filter (MxM).
- MAxPooling2D layers: A Convolutional Layer is usually followed by a Pooling Layer. The primary aim of this layer is to decrease the size of the convolved feature map to reduce the computational costs. This is performed by decreasing the connections between layers and independently operates on each feature map.

## Conclusions and what to do next