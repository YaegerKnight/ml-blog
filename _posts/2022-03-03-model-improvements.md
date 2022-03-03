---
toc: true
layout: post
description:
  "I used a series of techniques to improve the performance of my model while creating a pathway to (hopefully) bigger gains going forward."
categories: [redactionmodel, computervision, tools]
comments: true
author: Alex Strick van Linschoten
title: Incremental Improvements to my Redaction Detection Model
image: images/model-improvements/coco_scores.png
---

*(This is part of a series of blog posts documenting my work to train a model that detects redactions in documents. To read other posts, check out [the `redactionmodel` taglist](https://mlops.systems/categories/#redactionmodel).)*

Last time I wrote about my work on this project, I'd just finished [creating synthetic image data](https://mlops.systems/redactionmodel/computervision/python/tools/2022/02/10/synthetic-image-data.html) to supplement my manual annotations. Before integrating those into the model training, I wanted to make changes to a few hyper parameters to ensure that I'm getting as much out of the current configuration as possible.

I focused on three ways of improving the model's performance, each of which ended up having an effect albeit in different ways.

# Increasing the image size

When I started the project, I set the size of the images that would be passed into the training as the training dataset to 384x384 pixels. (It is a convention relating to how some of the older pre-trained models (like EfficientDet) were such that the image size must be divisible by 128.) This turned out to be too small.

The next steps up were 512 and 640. The GPU / hardware on which I was training my model seemed to have no problem with either of these image sizes and the performance increased as I worked with either 512 or 640 as the base image sizes.

# Increasing the batch size

Another important lever at my disposal was either to increase the batch size (the number of images that are used in each epoch) or to decrease the learning rate. ([A useful Twitter thread by Boris Dayma](https://twitter.com/borisdayma/status/1488297953429266433) explains some of the tradeoffs for one versus the other, along with some references to things to read).

I had started off with a batch size of 8, but increasing to 16 and then 32 had a big effect on my model's performance:

![]({{ site.baseurl
}}/images/model-improvements/batch-size-experiments.png "COCOMetric and validation loss charts for the different batch size increases")