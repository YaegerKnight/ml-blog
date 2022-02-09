---
toc: true
layout: post
description:
  "I iterated through several prototypes to get to a script that could autogenerate synthetic training data for my computer vision model. I hoped to bootstrap my training to get a bit jump in model performance."
categories: [redactionmodel, computervision, python, tools]
comments: true
author: Alex Strick van Linschoten
title: "It's raining bboxes: how I wrote a Python script to create 2097 synthetic images to help improve my machine learning model"
image: images/synthetic-images-data/synthetic-images-cover.jpg
---

## What I did — the summary TL;DR of what I did

## why synthetic images in the first place?

## (Short project summary)

## Level 1: First naive experiment

- some notebook experiments with Pillow
- paste redaction snippets on unredacted images (found with fastai model)
- random location and resize

## Detour: get stuck pretty quickly, esp in bbox land

- bbox confusion
- experiment then think then modularise

## Level 2: generate our own base images

- start with Pillow
- borg for synthetic image generation

## level 3: generate our own redactions

- pillow

## Bringing it all together

SHOW THE FLOW CHART

### IceVision for content box recognition

## level 4: make the images look old

- augraphy
- (other options)
- PIL vs OpenCV

## Running the pipeline: a bit slow

- Parallel Execution experiment (fail)

## then create the annotations

## Things for future improvement

- more customisable in terms of tweaking the various probabilities of various things happening
- data classes vs dictionaries vs passing images between functions

# Next time

## Combining the real / original annotations with the synthetic annotations

(for next time?)

## dealing with the whole workflow / process

- no data leakage
- versioning data