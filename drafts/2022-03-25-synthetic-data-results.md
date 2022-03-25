---
toc: true
layout: post
description:
  "I show how adding synthetic data has improved my redaction model's performance. TL;DR it has helped, but not as much as I would have expected."
categories: [tools, redactionmodel, computervision]
comments: true
author: Alex Strick van Linschoten
title: Performance boosts after training with synthetic data
image: images/synthetic-data-results/synthetic-results-cover.jpg
---

INSERT THE INTRO ITALIC THING

the story again till this point
last post was about figuring out why model underperforming
some 
also wrote about synthetic data creation
BUT WHAT RESULTS?

show the graphs

my hunch even before I look at how fiftyone has interpreted things?
- yes something was added into the mix, but since the crux of the model performance seems to relate to the hard examples (as we showed in previous post), our synthetic data needs to tackle that

What does fiftyone analysis and views show?

- hard images are still hard, even with the synthetic data
- the model is already pretty good at single redactions in images, esp if they're in the standard format

Next steps