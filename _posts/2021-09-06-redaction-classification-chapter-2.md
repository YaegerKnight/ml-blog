---
toc: true
layout: post
description: Explanation of how I trained a model to detect redactions in FOIA requests.
categories: [fastai, redactionmodel, computervision, datalabelling]
comments: true
author: Alex Strick van Linschoten
title: Training a classifier to detect redacted documents with fastai
---
# Test
I am working my way through [the fastai course](https://course.fast.ai) as part of an [online meetup group](https://www.meetup.com/delft-fast-ai-study-group) I host.[^1]

This week we finished the first and second chapters of the book, during which you train a model that can recognise if an image contains a cat or a dog. Later on, you train another model that distinguishes between different types of bears ('grizzly', 'black' and 'teddy').

[Jeremy Howard](https://twitter.com/jeremyphoward), who is teaching the course, then prompts you to take what you learned and apply it to something that has meaning for you. (This is something that most of those who've found any success with the course [emphasise repeatedly](https://sanyambhutani.com/how-not-to-do-fast-ai--or-any-ml-mooc-/).)

I decided to work on something adjacent to my previous life / work, where I knew there was some real-world value to be gained from such a model. I chose to train an image classifier model which would classify whether a particular image was redacted or not.

## The Problem Domain: Image Redaction

Under the [Freedom of Information Act](https://en.wikipedia.org/wiki/Freedom_of_Information_Act_(United_States)) (FOIA), individuals can request records and information from the US government.[^2] This is [one collection](https://www.esd.whs.mil/FOIA/Reading-Room/Reading-Room-List_2/) of some of the responses to this requests, sorted into various categories. You can read, for example, responses relating to UFOs and alien visits [here](https://www.esd.whs.mil/FOIA/Reading-Room/Reading-Room-List/UFO/).

Quite often, however, these images are censored or redacted.

![]({{ site.baseurl }}/images/redacted_sample.png "a sample image")

## Summary of chapters 1 and 2

## The Whole Pipeline

ingestion
pdf processing

## Importing images with ddg import tool

## Labelling data with Prodigy

## Transferring the data to Paperspace with `magic-wormhole`


## Using the labelled data in our training

## Initial results

## Experimenting with augmentations

## Experimenting with different architectures

## Hosting the model with MyBinder

## Possible next steps

- more labelled data?
- simple binary classification?
- different types of redaction (handwritten vs computer) and a way to combine?
- segmentation model?

## Footnotes

[^1]: You can find our thread in the fastai forum [here](https://forums.fast.ai/t/virtual-study-group-delft-the-netherlands-europe-time-zone/90521).
[^2]: Other countries have variations of this law, like [this](https://www.gov.uk/make-a-freedom-of-information-request) from the United Kingdom.